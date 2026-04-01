# Commands

Commands for each step in the de novo transcriptome assembly pipeline, from raw data download through annotation and quality assessment. All examples assume a SLURM-based HPC environment like the TACC environment I worked in. See the references for specifics on installing and running tools.


## Prerequisites

### Module-Loaded Tools

The following tools are available via TACC's Lmod system and require no manual installation:

#### SRA Toolkit and parallel gzip
```
module load biocontainers

module load sra-tools/ctr-3.1.1--h4304569_0

module load pbgzip/ctr-2016.08.04--he4cf2ce_0
```

#### Quality trimming and reporting
```
module load biocontainers

module load fastp/ctr-0.19.7--hdbcaa40_0

module load multiqc/ctr-1.7--py_1
```

#### Assemblers
```
module load trinity/ctr-2.8.4--py36pl526h447964c_0

module load spades/ctr-3.13.0--0
```
#### Redundancy Filtering
```
module load biocontainers

module load cd-hit/ctr-4.6.8--0

module load mmseqs2/ctr-7.4e23d--h21aa3a5_1
```
### Apptainer Tools

#### BUSCO - Pulled directly from Docker:
```
module load tacc-apptainer

apptainer pull docker://staphb/busco
```
This creates ```busco_latest.sif``` in the current directory.

#### EnTAP - Compiled from the [EnTAP GitLab](https://gitlab.com/PlantGenomicsLab/EnTAP) 

After building, the resulting image is used as entap.sif. Refer to the [EnTAP documentation](https://entap.readthedocs.io/en/latest/) for compilation and database configuration steps (entap_config.ini)

## Running the Tools
### 1. SRA Download 
Download raw sequencing data from NCBI's Sequence Read Archive and convert to FASTQ format.

```
#Prefetch SRA files from an accession list
prefetch --option-file <ACCESSION_FILE> -O <SRA_DIR>

#Convert each .sra file to paired-end FASTQ and compress
find <SRA_DIR> -name "*.sra" | while read file; do
    name=$(basename $file .sra)
    # Dump to FASTQ with 16 threads; custom deflines for downstream compatibility
    fasterq-dump "$file" \
        --outdir <FASTQ_DIR> \
        -e 16 \
        -t <TEMP_DIR> \
        --seq-defline '@$sn[_$rn]/$ri' \
        --qual-defline '+'
    # Parallel gzip compress each mate file
    pbgzip <FASTQ_DIR>/${name}_1.fastq
    pbgzip <FASTQ_DIR>/${name}_2.fastq
done
```
### 2. Quality Trimming 
Trim adapters and filter low-quality reads with FASTP and generate a combined QC report with MultiQC

#### Setup Notes
On TACC, `fastp` is available as a biocontainer and is loaded as a shell function rather than a
standard binary. Because the pipeline's Python wrapper invokes fastp via `subprocess`, it cannot
inherit shell functions from the module environment. The full Singularity command must be passed
explicitly via the `-c` flag instead of just `fastp`.

Before running, ensure your fastq directory contains no hidden or empty files (e.g. `.fastq.gz`),
as the wrapper script will attempt to process any file matching FASTQ extensions, including
zero-byte hidden files.

```
#Run fastp via the project's Python wrapper script
#-i: input directory   -o: output directory   -r: report directory
#-1 / -2: mate pair suffixes   -c: tool name for report naming
python <PY_FILE> \
    -i <FASTQ_DIR> \
    -o <TRIMMED_DIR> \
    -r <REPORT_DIR> \
    -1 _1 -2 _2 \
    -c "singularity exec ${BIOCONTAINER_DIR}/biocontainers/fastp/fastp-0.19.7--hdbcaa40_0.simg fastp"

#Aggregate all fastp reports into a single MultiQC report
multiqc <REPORT_DIR> -o <PROJECT_ROOT>/multiqc_report -s
```

### 3a. Assembly with Trinity
De novo transcriptome assembly using Trinity. See [documentation](https://github.com/trinityrnaseq/trinityrnaseq/wiki) for complete configuration options.

```
#Run Trinity on paired-end reads
Trinity --seqType fq \
        --max_memory 800G \
        --left <TRIMMED_DIR>/pooled_6_1.fastq.gz \
        --right <TRIMMED_DIR>/pooled_6_2.fastq.gz \
        --CPU 64 \
        --output <TRINITY_OUT_DIR>
```

### 3b. Assembly with Spades
De novo transcriptome assembly using Spades. See [documentation](https://ablab.github.io/spades/) for complete configuration options.

```
#Run rnaSPAdes
rnaspades.py \
    -1 pooled_6_1.fastq.gz \
    -2 pooled_6_2.fastq.gz \
    -o <SPADES_OUT_DIR> \
    -t 64 \
    -m 250 \
    --temp <TEMP_DIR>
```

### 4a. Redundancy Filtering
Both cd-hit and MMseqs2 were used to cluster and remove redundant transcripts from each assembly.

**CD-hit**
```
#Cluster at default 90% identity threshold
cd-hit-est -i <ASSEMBLY_FASTA> -o <ASSEMBLY_CDHIT_FASTA> -M 1000000 -T 0 -c 0.90 -n 10 -d 0
```

**mmseqs**
```
#Cluster at 90% identity and 90% coverage
mmseqs easy-cluster <ASSEMBLY_FASTA> <OUTPUT_PREFIX> <TEMP_DIR> \
    --min-seq-id 0.90 \
    -c 0.90 --cov-mode 1 \
    --cluster-mode 2 \
    --threads 16
```

### 4b. Quality Assessment — BUSCO
Assess assembly completeness against a lineage-specific ortholog database.

```
#Run BUSCO in transcriptome mode against the bacillariophyta (narrower) database
apptainer exec busco_latest.sif busco \
    --mode tran \
    -l bacillariophyta_odb12 \
    -c 32 \
    -i <ASSEMBLY_FASTA> \
    -o <BUSCO_OUT_DIR>

#Run BUSCO in transcriptome mode against the stramenopiles (broader) database
apptainer exec busco_latest.sif busco \
    --mode tran \
    -l stramenopiles_odb10 \
    -c 32 \
    -i <ASSEMBLY_FASTA> \
    -o <BUSCO_OUT_DIR>
```


### 5. Annotation — EnTAP
Functional annotation of assemblies using EnTAP.

```
#Run EnTAP
apptainer exec entap.sif EnTAP \
    --run \
    --run-ini <PROJECT_ROOT>/assembly_run.params \
    --entap-ini <PROJECT_ROOT>/entap_config.ini \
    -t 32
```

### 7. Transcript Quantification — Salmon

Reads were mapped back to each assembly using `Salmon` in quasi-mapping mode to generate per-sample mapping rates and transcript abundance estimates.

#### Build Index
```bash
# Load modules
module load biocontainers
module load salmon

# Index the assembly
salmon index -t  -i  -k 31
```
#### Quantify Per Sample
```bash
# Map each sample against the index
for sample in SRR22322224 SRR22322220 SRR22322213 SRR22322206 SRR22322190 SRR22322179; do
    salmon quant \
        -i  \
        -l A \
        -1 /${sample}_1.fastq.gz \
        -2 /${sample}_2.fastq.gz \
        -o /${sample} \
        --validateMappings \
        -p 16
done
```
#### Aggregate Reports
```bash
# Aggregate per-sample Salmon reports with MultiQC
module load multiqc
multiqc  -o  -n salmon_report
```

---

### Placeholder Reference

| Placeholder | Description |
|---|---|
| `<ACCESSION_FILE>` | Text file with one SRA accession per line |
| `<SRA_DIR>` | Directory for prefetched `.sra` files |
| `<FASTQ_DIR>` | Directory for raw FASTQ output |
| `<TRIMMED_DIR>` | Directory for quality-trimmed FASTQ files |
| `<TEMP_DIR>` | Scratch space for temporary files |
| `<REPORT_DIR>` | Directory for fastp JSON/HTML reports |
| `<PROJECT_ROOT>` | Top-level project directory |
| `<PY_FILE>` | Path to the fastp Python wrapper script |
| `<TRINITY_OUT_DIR>` | Trinity assembly output directory |
| `<SPADES_OUT_DIR>` | rnaSPAdes assembly output directory |
| `<ASSEMBLY_FASTA>` | Input FASTA file (e.g., `Trinity_cd-hit.fasta`, `Spades_cd-hit.fasta`) |
| `<BUSCO_BACILLARIOPHYTA_OUT>` | BUSCO results directory (Bacillariophyta lineage) |
| `<BUSCO_STRAMENOPILES_OUT>` | BUSCO results directory (Stramenopiles lineage) |
| `<SALMON_INDEX_DIR>` | Salmon index directory for a given assembly |
| `<SALMON_OUT_DIR>` | Parent directory for per-sample Salmon quantification output |
| `<MULTIQC_SALMON_OUT>` | MultiQC output directory for Salmon reports |
