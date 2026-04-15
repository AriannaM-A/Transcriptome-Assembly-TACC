# Results

Two independent assembly runs were performed to evaluate how sample size affects de novo transcriptome assembly quality for *Fragilariopsis cylindrus* on TACC Stampede3:

- **Run 1 — Pilot (10 samples):** Initial assembly using a subset of 10 paired-end RNA-seq samples to validate the pipeline end-to-end.
- **Run 2 — Full (67 samples):** Production assembly using the complete set of 67 paired-end samples spanning 15 experimental conditions (darkness time series and light return time series, 3 replicates each).

All metrics below compare the two runs against each other and against the published *F. cylindrus* gene models (Mock et al., 2017) as an external reference point.

---

## Assembly Statistics

### Trinity

| Metric | Run 1 (10 samples) | Run 2 (66 samples) | Gene Models (Mock et al., 2017) |
| --- | --- | --- | --- |
| Total transcripts (after cd-hit) | 67,058 | _TBD_ | 21,066 |
| Total bases | 62,179,287 | _TBD_ | 27,463,593 |
| Min length (bp) | 191 | _TBD_ | 150 |
| Avg length (bp) | 927.2 | _TBD_ | 1,303.7 |
| Max length (bp) | 16,710 | _TBD_ | 17,613 |
| Q1 length (bp) | 259 | _TBD_ | 705 |
| Median length (bp) | 426 | _TBD_ | 1,060 |
| Q3 length (bp) | 1,174 | _TBD_ | 1,584 |
| N50 (bp) | 1,817 | _TBD_ | 1,560 |

### rnaSPAdes

| Metric | Run 1 (10 samples) | Run 2 (66 samples) | Gene Models (Mock et al., 2017) |
| --- | --- | --- | --- |
| Total transcripts (after cd-hit) | 39,126 | _TBD_ | 21,066 |
| Total bases | 47,863,325 | _TBD_ | 27,463,593 |
| Min length (bp) | 226 | _TBD_ | 150 |
| Avg length (bp) | 1,223.3 | _TBD_ | 1,303.7 |
| Max length (bp) | 17,246 | _TBD_ | 17,613 |
| Q1 length (bp) | 410 | _TBD_ | 705 |
| Median length (bp) | 775 | _TBD_ | 1,060 |
| Q3 length (bp) | 1,604 | _TBD_ | 1,584 |
| N50 (bp) | 1,908 | _TBD_ | 1,560 |

---

## BUSCO Completeness Assessment

BUSCO was run in transcriptome mode on the cd-hit filtered assemblies against two lineage datasets: Bacillariophyta (`bacillariophyta_odb12`, n=2,944) for clade-specific resolution and Stramenopiles (`stramenopiles_odb10`, n=100) for broader comparability with published diatom assemblies.

### Bacillariophyta (`bacillariophyta_odb12`) — Trinity

| Metric | Run 1 (10 samples) | Run 2 (66 samples) | Gene Models (Mock et al., 2017) |
| --- | --- | --- | --- |
| Complete (C) | 85.2% | _TBD_ | 87.6% |
| Complete & Single-copy (S) | 62.6% | _TBD_ | 76.3% |
| Complete & Duplicated (D) | 22.6% | _TBD_ | 11.3% |
| Fragmented (F) | 10.3% | _TBD_ | 10.4% |
| Missing (M) | 4.5% | _TBD_ | 2.0% |
| Total BUSCOs searched (n) | 2,944 | 2,944 | 2,944 |

### Bacillariophyta (`bacillariophyta_odb12`) — rnaSPAdes

| Metric | Run 1 (10 samples) | Run 2 (66 samples) | Gene Models (Mock et al., 2017) |
| --- | --- | --- | --- |
| Complete (C) | 80.9% | _TBD_ | 87.6% |
| Complete & Single-copy (S) | 68.0% | _TBD_ | 76.3% |
| Complete & Duplicated (D) | 12.9% | _TBD_ | 11.3% |
| Fragmented (F) | 12.2% | _TBD_ | 10.4% |
| Missing (M) | 6.9% | _TBD_ | 2.0% |
| Total BUSCOs searched (n) | 2,944 | 2,944 | 2,944 |

### Stramenopiles (`stramenopiles_odb10`) — Trinity

| Metric | Run 1 (10 samples) | Run 2 (66 samples) | Gene Models (Mock et al., 2017) |
| --- | --- | --- | --- |
| Complete (C) | 98.0% | 95.0% | 95.0% |
| Complete & Single-copy (S) | 60.0% | 79.0% | 83.0% |
| Complete & Duplicated (D) | 38.0% | 16.0% | 12.0% |
| Fragmented (F) | 2.0% | 5.0% | 2.0% |
| Missing (M) | 0.0% | 0.0% | 3.0% |

### Stramenopiles (`stramenopiles_odb10`) — rnaSPAdes

| Metric | Run 1 (10 samples) | Run 2 (66 samples) | Gene Models (Mock et al., 2017) |
| --- | --- | --- | --- |
| Complete (C) | 95.0% | 99.0% | 95.0% |
| Complete & Single-copy (S) | 74.0% | 74.0% | 83.0% |
| Complete & Duplicated (D) | 21.0% | 25.0% | 12.0% |
| Fragmented (F) | 4.0% | 1.0% | 2.0% |
| Missing (M) | 1.0% | 0% | 3.0% |

---

## Salmon Mapping Rates

Reads from each sample were mapped back to all references using `Salmon` in quasi-mapping mode with `--validateMappings`. Mapping rate indicates the percentage of reads that successfully aligned to the reference, serving as a measure of how well each reference captures the expressed transcript content.

### Trinity

| Sample | Treatment | Run 1 (10 samples) | Run 2 (66 samples) | Gene Models (Mock et al., 2017) |
| --- | --- | --- | --- | --- |
| SRR22322224 | Acclimated to full light | 96.4% | _TBD_ | 61.4% |
| SRR22322220 | 6h of darkness | 96.7% | _TBD_ | 64.1% |
| SRR22322213 | 3 days of darkness | 96.8% | _TBD_ | 61.4% |
| SRR22322206 | 2 months in darkness | 95.5% | _TBD_ | 53.8% |
| SRR22322190 | 6h after light return | 95.8% | _TBD_ | 61.3% |
| SRR22322179 | 30 min after light return | 88.7% | _TBD_ | 28.8% |
| **Average** |  | **95.0%** | _TBD_ | **55.1%** |

### rnaSPAdes

| Sample | Treatment | Run 1 (10 samples) | Run 2 (66 samples) | Gene Models (Mock et al., 2017) |
| --- | --- | --- | --- | --- |
| SRR22322224 | Acclimated to full light | 94.8% | _TBD_ | 61.4% |
| SRR22322220 | 6h of darkness | 94.5% | _TBD_ | 64.1% |
| SRR22322213 | 3 days of darkness | 95.4% | _TBD_ | 61.4% |
| SRR22322206 | 2 months in darkness | 92.9% | _TBD_ | 53.8% |
| SRR22322190 | 6h after light return | 93.9% | _TBD_ | 61.3% |
| SRR22322179 | 30 min after light return | 95.1% | _TBD_ | 28.8% |
| **Average** |  | **94.4%** | _TBD_ | **55.1%** |

> **Note:** For Run 2, consider reporting mapping rates averaged across all 67 samples (grouped by the 15 experimental conditions) rather than the 6-sample subset shown above, so the comparison reflects the full dataset.

---

## Functional Annotation (EnTAP)

EnTAP was run on the cd-hit filtered assemblies using `uniprot_sprot` for similarity searching and EggNOG for ortholog/gene family assignment. Results from the published *F. cylindrus* gene models (Mock et al., 2017) are included as a reference point.

### Annotation Summary — Trinity

| Metric | Run 1 (10 samples) | Run 2 (66 samples) | Gene Models (Mock et al., 2017) |
| --- | --- | --- | --- |
| Total annotated sequences | 19,595 | _TBD_ | 13,512 |
| Total ORFs predicted | 33,132 | _TBD_ | 50,758 |
| Complete ORFs | 13,743 (41.5%) | _TBD_ | 23,146 (45.6%) |
| Internal ORFs | 8,625 (26.0%) | _TBD_ | 11,065 (21.8%) |
| Partial ORFs | 10,764 (32.5%) | _TBD_ | 16,547 (32.6%) |
| Sequence search hits (UniProt) | 33.2% | _TBD_ | 41.3% |
| Informative hits | 31.8% | _TBD_ | 39.6% |
| EggNOG hits | 100% | _TBD_ | 100% |
| GO terms (UniProt) | 32.4% | _TBD_ | 40.1% |
| KEGG assignments (EggNOG) | 43.7% | _TBD_ | 36.9% |
| Protein domains (UniProt) | 29.9% | _TBD_ | 36.5% |
| Contaminants flagged | 0% | _TBD_ | 0% |

### Annotation Summary — rnaSPAdes

| Metric | Run 1 (10 samples) | Run 2 (66 samples) | Gene Models (Mock et al., 2017) |
| --- | --- | --- | --- |
| Total annotated sequences | 17,497 | _TBD_ | 13,512 |
| Total ORFs predicted | 35,567 | _TBD_ | 50,758 |
| Complete ORFs | 15,343 (43.1%) | _TBD_ | 23,146 (45.6%) |
| Internal ORFs | 8,796 (24.7%) | _TBD_ | 11,065 (21.8%) |
| Partial ORFs | 11,428 (32.1%) | _TBD_ | 16,547 (32.6%) |
| Sequence search hits (UniProt) | 31.2% | _TBD_ | 41.3% |
| Informative hits | 29.8% | _TBD_ | 39.6% |
| EggNOG hits | 100% | _TBD_ | 100% |
| GO terms (UniProt) | 30.4% | _TBD_ | 40.1% |
| KEGG assignments (EggNOG) | 43.7% | _TBD_ | 36.9% |
| Protein domains (UniProt) | 28.0% | _TBD_ | 36.5% |
| Contaminants flagged | 0% | _TBD_ | 0% |

### EggNOG Taxonomic Scope — Trinity

The proportion of transcripts mapping to Bacillariophyta-level orthologs in EggNOG provides an indicator of how well the assembly captures diatom-specific gene content.

| Taxonomic Scope | Run 1 (10 samples) | Run 2 (66 samples) | Gene Models (Mock et al., 2017) |
| --- | --- | --- | --- |
| Bacillariophyta | 49.0% | _TBD_ | 61.6% |
| Eukaryota | 19.8% | _TBD_ | 16.9% |
| Fungi | 6.6% | _TBD_ | 2.5% |
| Streptophyta | 4.3% | _TBD_ | 2.3% |
| Metazoa | 4.4% | _TBD_ | 3.2% |

### EggNOG Taxonomic Scope — rnaSPAdes

| Taxonomic Scope | Run 1 (10 samples) | Run 2 (66 samples) | Gene Models (Mock et al., 2017) |
| --- | --- | --- | --- |
| Bacillariophyta | 49.7% | _TBD_ | 61.6% |
| Eukaryota | 20.0% | _TBD_ | 16.9% |
| Fungi | 6.2% | _TBD_ | 2.5% |
| Streptophyta | 4.0% | _TBD_ | 2.3% |
| Metazoa | 4.3% | _TBD_ | 3.2% |

---

## Run 1 vs Run 2 Summary

_To be completed after Run 2 metrics are available. Suggested discussion points:_

- Change in total transcript count and whether added samples primarily added novel transcripts or expanded coverage of existing ones
- N50 and length distribution shifts — does the larger input improve contiguity?
- BUSCO completeness gains, and whether duplication rates change with more input reads
- Mapping rate differences across the full condition set (darkness time series vs light return)
- Annotation yield (UniProt hits, Bacillariophyta-scoped EggNOG hits) relative to transcript count growth
- Resource use comparison (wall time, peak memory) between the two runs on Stampede3
