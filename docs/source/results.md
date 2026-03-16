# Results

### Assembly Statistics

| Metric | Trinity | rnaSPAdes | Gene Models (Mock et al., 2017) |
|---|---|---|---|
| Total transcripts (after cd-hit) | 67,058 | 39,126 | 21,066 |
| Total bases | 62,179,287 | 47,863,325 | 27,463,593 |
| Min length (bp) | 191 | 226 | 150 |
| Avg length (bp) | 927.2 | 1,223.3 | 1,303.7 |
| Max length (bp) | 16,710 | 17,246 | 17,613 |
| Q1 length (bp) | 259 | 410 | 705 |
| Median length (bp) | 426 | 775 | 1,060 |
| Q3 length (bp) | 1,174 | 1,604 | 1,584 |
| N50 (bp) | 1,817 | 1,908 | 1,560 |

### BUSCO Completeness Assessment

BUSCO was run in transcriptome mode on the cd-hit filtered assemblies against two lineage datasets: Bacillariophyta (`bacillariophyta_odb12`, n=2,944) for clade-specific resolution and Stramenopiles (`stramenopiles_odb10`, n=100) for broader comparability with published diatom assemblies.

#### Bacillariophyta (`bacillariophyta_odb12`)

| Metric | Trinity | rnaSPAdes | Gene Models (Mock et al., 2017) |
|---|---|---|---|
| Complete (C) | 85.2% | 80.9% | 87.6% |
| Complete & Single-copy (S) | 62.6% | 68.0% | 76.3% |
| Complete & Duplicated (D) | 22.6% | 12.9% | 11.3% |
| Fragmented (F) | 10.3% | 12.2% | 10.4% |
| Missing (M) | 4.5% | 6.9% | 2.0% |
| Total BUSCOs searched (n) | 2,944 | 2,944 | 2,944 |

#### Stramenopiles (`stramenopiles_odb10`)

| Metric | Trinity | rnaSPAdes | Gene Models (Mock et al., 2017) |
|---|---|---|---|
| Complete (C) | 98.0% | 95.0% | 95.0% |
| Complete & Single-copy (S) | 60.0% | 74.0% | 83.0% |
| Complete & Duplicated (D) | 38.0% | 21.0% | 12.0% |
| Fragmented (F) | 2.0% | 4.0% | 2.0% |
| Missing (M) | 0.0% | 1.0% | 3.0% |

### Salmon Mapping Rates

Reads from each sample were mapped back to all three references using `Salmon` in quasi-mapping mode with `--validateMappings`. Mapping rate indicates the percentage of reads that successfully aligned to the reference, serving as a measure of how well each reference captures the expressed transcript content.

| Sample | Treatment | Trinity | rnaSPAdes | Gene Models (Mock et al., 2017) |
|---|---|---|---|---|
| SRR22322224 | Acclimated to full light | 96.4% | 94.8% | 61.4% |
| SRR22322220 | 6h of darkness | 96.7% | 94.5% | 64.1% |
| SRR22322213 | 3 days of darkness | 96.8% | 95.4% | 61.4% |
| SRR22322206 | 2 months in darkness | 95.5% | 92.9% | 53.8% |
| SRR22322190 | 6h after light return | 95.8% | 93.9% | 61.3% |
| SRR22322179 | 30 min after light return | 88.7% | 95.1% | 28.8% |
| **Average** | | **95.0%** | **94.4%** | **55.1%** |

### Functional Annotation (EnTAP)

EnTAP was run on the cd-hit filtered assemblies using `uniprot_sprot` for similarity searching and EggNOG for ortholog/gene family assignment. Results from the published *F. cylindrus* gene models (Mock et al., 2017) are included as a reference point.

#### Annotation Summary

| Metric | Trinity | rnaSPAdes | Gene Models (Mock et al., 2017) |
|---|---|---|---|
| Total annotated sequences | 19,595 | 17,497 | 13,512 |
| Complete ORFs | 52.5% | 48.9% | 74.5% |
| Sequence search hits (UniProt) | 33.2% | 31.2% | 41.3% |
| Informative hits | 31.8% | 29.8% | 39.6% |
| EggNOG hits | 100% | 100% | 100% |
| GO terms (UniProt) | 32.4% | 30.4% | 40.1% |
| KEGG assignments (EggNOG) | 43.7% | 43.7% | 36.9% |
| Protein domains (UniProt) | 29.9% | 28.0% | 36.5% |
| Contaminants flagged | 0% | 0% | 0% |

#### EggNOG Taxonomic Scope

The proportion of transcripts mapping to Bacillariophyta-level orthologs in EggNOG provides an indicator of how well the assembly captures diatom-specific gene content.

| Taxonomic Scope | Trinity | rnaSPAdes | Gene Models (Mock et al., 2017) |
|---|---|---|---|
| Bacillariophyta | 49.0% | 49.7% | 61.6% |
| Eukaryota | 19.8% | 20.0% | 16.9% |
| Fungi | 6.6% | 6.2% | 2.5% |
| Streptophyta | 4.3% | 4.0% | 2.3% |
| Metazoa | 4.4% | 4.3% | 3.2% |
