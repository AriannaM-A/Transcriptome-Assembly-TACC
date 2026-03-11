# Results

## Assembly Statistics

| Metric | Trinity | rnaSPAdes |
|---|---|---|
| Total raw transcripts | 91,195 | `TBD` |
| Transcripts after cd-hit (90% identity) | `TBD` | `TBD` |
| Transcripts after MMseqs2 (90% identity) | `TBD` | `TBD` |

## BUSCO Completeness Assessment

BUSCO was run in transcriptome mode on the cd-hit filtered assemblies against two lineage datasets: Bacillariophyta (`bacillariophyta_odb12`, n=`TBD`) for clade-specific resolution and Stramenopiles (`stramenopiles_odb10`, n=100) for broader comparability with published diatom assemblies.

### Bacillariophyta (`bacillariophyta_odb12`)

| Metric | Trinity | rnaSPAdes | Gene Models (Mock et al., 2017) |
|---|---|---|---|
| Complete (C) | `TBD` | `TBD` | `TBD` |
| Complete & Single-copy (S) | `TBD` | `TBD` | `TBD` |
| Complete & Duplicated (D) | `TBD` | `TBD` | `TBD` |
| Fragmented (F) | `TBD` | `TBD` | `TBD` |
| Missing (M) | `TBD` | `TBD` | `TBD` |

### Stramenopiles (`stramenopiles_odb10`)

| Metric | Trinity | rnaSPAdes | Gene Models (Mock et al., 2017) |
|---|---|---|---|
| Complete (C) | `TBD` | 95.0% | `TBD` |
| Complete & Single-copy (S) | `TBD` | 63.0% | `TBD` |
| Complete & Duplicated (D) | `TBD` | 32.0% | `TBD` |
| Fragmented (F) | `TBD` | 4.0% | `TBD` |
| Missing (M) | `TBD` | 1.0% | `TBD` |

## Functional Annotation (EnTAP)

EnTAP was run on the cd-hit filtered assemblies using `uniprot_sprot` for similarity searching and EggNOG for ortholog/gene family assignment. Results from the published *F. cylindrus* gene models (Mock et al., 2017) are included as a reference point.

### Annotation Summary

| Metric | Trinity | rnaSPAdes | Gene Models (Mock et al., 2017) |
|---|---|---|---|
| Total input sequences | `TBD` | `TBD` | 13,512 |
| Retained after frame selection | `TBD` | `TBD` | 13,512 |
| Complete ORFs | `TBD` | `TBD` | 74.5% |
| Sequence search hits (UniProt) | `TBD` | `TBD` | 41.3% |
| Informative hits | `TBD` | `TBD` | 39.6% |
| EggNOG hits | `TBD` | `TBD` | 100% |
| GO terms (UniProt) | `TBD` | `TBD` | 40.1% |
| KEGG assignments (EggNOG) | `TBD` | `TBD` | 36.9% |
| Protein domains (UniProt) | `TBD` | `TBD` | 36.5% |
| Contaminants flagged | `TBD` | `TBD` | 0% |

### EggNOG Taxonomic Scope

The proportion of transcripts mapping to Bacillariophyta-level orthologs in EggNOG provides an indicator of how well the assembly captures diatom-specific gene content.

| Taxonomic Scope | Trinity | rnaSPAdes | Gene Models (Mock et al., 2017) |
|---|---|---|---|
| Bacillariophyta | `TBD` | `TBD` | 61.6% |
| Eukaryota | `TBD` | `TBD` | 16.9% |
| Fungi | `TBD` | `TBD` | 2.5% |
| Streptophyta | `TBD` | `TBD` | 2.3% |
| Metazoa | `TBD` | `TBD` | 3.2% |

### Top Species Hits (Similarity Search)

Top species from DIAMOND blastp against UniProt/SwissProt. The dominance of model organisms reflects database composition bias rather than contamination, as confirmed by the EggNOG taxonomic scope mapping predominantly to Bacillariophyta.

| Species | Gene Models (Mock et al., 2017) |
|---|---|
| *Arabidopsis thaliana* | 8.5% |
| *Homo sapiens* | 2.6% |
| *Mus musculus* | 2.3% |
| *Dictyostelium discoideum* | 2.0% |
| *Schizosaccharomyces pombe* | 1.4% |
