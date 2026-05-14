# Tiered Sample Size Study

The pilot assembly described in the previous Results page produced two transcriptomes with strong BUSCO completeness (98% Stramenopiles for Trinity, 95% for rnaSPAdes) and high Salmon mapping rates (~95% average across the six pilot samples). On those metrics alone the pipeline appeared to be working well, and the resulting transcriptomes were good candidates for downstream use.

That left a follow-up question worth answering before declaring the project finished: at what point does adding more RNA-seq samples stop improving *de novo* transcriptome assembly quality, and does read normalization undermine the value of large sample inputs? To explore that, four additional assembly tiers were generated for each assembler, with each tier a strict superset of the previous: **12, 24, 36, 48, and 65** paired-end samples. Sample selection prioritized completing biological triplicates before adding new conditions so that treatment representation stayed balanced as the input grew.

## Normalization Strategy

| Tier | Trinity | rnaSPAdes |
|---|---|---|
| 6  | BBNorm-normalized input (`--no_normalize_reads`) | BBNorm-normalized input |
| 12 | BBNorm-normalized input (`--no_normalize_reads`) | BBNorm-normalized input |
| 24 | BBNorm-normalized input (`--no_normalize_reads`) | BBNorm-normalized input |
| 36 | BBNorm-normalized input (`--no_normalize_reads`) | BBNorm-normalized input |
| 48 | BBNorm-normalized input (`--no_normalize_reads`) | BBNorm-normalized input |
| 65 | BBNorm-normalized input (`--no_normalize_reads`) | BBNorm-normalized input |

Trinity's built-in in-silico normalization is a known failure point at very large input scales, and Spades does not use a built in normalization tool, so the reads were pre-normalized with BBNorm (target=100, min=5) and fed to Trinity with the `--no_normalize_reads` flag. BBNorm reduced the ~2.29 billion read pairs in the 65-sample input down to ~78.9 million pairs (~97% reduction), peaking at ~146 GB RAM on an ICX node.

## Mapping & Evaluation Strategy

For comparability across tiers of different input sizes, all five tiered assemblies were evaluated by mapping the **complete 65-sample read set** back against each one with `Salmon`, regardless of how many samples went into building that particular assembly. This differs from the pilot Results page, where the 6-sample assembly was evaluated against the 6 samples used to build it.

---

## Assembly Statistics

### Trinity

| Metric | 6 Samples | 12 Samples | 24 Samples | 36 Samples | 48 Samples | 65 Samples |
|---|---|---|---|---|---|---|
| Total transcripts (after filtering) | 67,058 | 88,078 | 118,431 | 93,919 | 105,721 | 118,086 |
| Total bases | 62,179,287 | 74,587,327 | 92,310,339 | 95,095,436 | 113,630,052 | 123,464,696 |
| Min length (bp) | 191 | 195 | 188 | 165 | 193 | 186 |
| Avg length (bp) | 927.2 | 846.8 | 779.4 | 1,012.5 | 1,074.8 | 1,045.5 |
| Max length (bp) | 16,710 | 18,181 | 18,433 | 20,362 | 19,291 | 20,959 |
| Q1 length (bp) | 259 | 252 | 243 | 278 | 286 | 279 |
| Median length (bp) | 426 | 384 | 339 | 460 | 493 | 465 |
| Q3 length (bp) | 1,174 | 967 | 766 | 1,189 | 1,311 | 1,215 |
| N50 (bp) | 1,817 | 1,697 | 1,650 | 2,054 | 2,141 | 2,142 |

### rnaSPAdes

| Metric | 6 Samples | 12 Samples | 24 Samples | 36 Samples | 48 Samples | 65 Samples |
|---|---|---|---|---|---|---|
| Total transcripts (after filtering) | 39,126 | 41,905 | 49,141 | 53,218 | 60,949 | 67,352 |
| Total bases | 47,863,325 | 68,538,120 | 81,795,345 | 90,822,269 | 108,230,624 | 115,661,208 |
| Min length (bp) | 226 | 226 | 226 | 226 | 226 | 101 |
| Avg length (bp) | 1,223.3 | 1,635.6 | 1,664.5 | 1,706.6 | 1,775.8 | 1,717.3 |
| Max length (bp) | 17,246 | 20,866 | 24,010 | 22,376 | 24,102 | 26,093 |
| Q1 length (bp) | 410 | 477 | 452 | 474 | 530 | 469 |
| Median length (bp) | 775 | 1,179 | 1,171 | 1,219 | 1,283 | 1,180 |
| Q3 length (bp) | 1,604 | 2,226 | 2,280 | 2,344 | 2,413 | 2,326 |
| N50 (bp) | 1,908 | 2,550 | 2,680 | 2,725 | 2,776 | 2,779 |

---

## BUSCO Completeness Assessment

### Bacillariophyta (`bacillariophyta_odb12`, n=2,944) — Trinity

| Metric | 6 Samples | 12 Samples | 24 Samples | 36 Samples | 48 Samples | 65 Samples |
|---|---|---|---|---|---|---|
| Complete (C) | 85.2% | 81.7% | 81.8% | 80.9% | 81.5% | 81.5% |
| Complete & Single-copy (S) | 62.6% | 52.2% | 45.2% | 39.2% | 36.9% | 34.6% |
| Complete & Duplicated (D) | 22.6% | 29.4% | 36.6% | 41.7% | 44.6% | 46.9% |
| Fragmented (F) | 10.3% | 12.6% | 13.2% | 13.4% | 13.3% | 13.2% |
| Missing (M) | 4.5% | 5.7% | 5.0% | 5.7% | 5.2% | 5.3% |

### Bacillariophyta (`bacillariophyta_odb12`, n=2,944) — rnaSPAdes

| Metric | 6 Samples | 12 Samples | 24 Samples | 36 Samples | 48 Samples | 65 Samples |
|---|---|---|---|---|---|---|
| Complete (C) | 80.9% | 89.5% | 89.8% | 89.2% | 89.0% | 89.1% |
| Complete & Single-copy (S) | 68.0% | 45.6% | 38.1% | 35.0% | 30.4% | 29.7% |
| Complete & Duplicated (D) | 12.9% | 43.9% | 51.7% | 54.2% | 58.5% | 59.4% |
| Fragmented (F) | 12.2% | 8.0% | 8.8% | 9.3% | 9.5% | 9.2% |
| Missing (M) | 6.9% | 2.5% | 1.5% | 1.5% | 1.5% | 1.7% |

### Stramenopiles (`stramenopiles_odb10`, n=100) — Trinity

| Metric | 6 Samples | 12 Samples | 24 Samples | 36 Samples | 48 Samples | 65 Samples |
|---|---|---|---|---|---|---|
| Complete (C) | 98.0% | 92.3% | 92.4% | 92.5% | 96.4% | 96.6% |
| Complete & Single-copy (S) | 60.0% | 50.4% | 40.5% | 30.8% | 22.4% | 20.9% |
| Complete & Duplicated (D) | 38.0% | 41.9% | 51.9% | 61.7% | 74.0% | 75.6% |
| Fragmented (F) | 2.0% | 6.7% | 6.3% | 6.7% | 3.4% | 3.2% |
| Missing (M) | 0.0% | 1.0% | 1.3% | 0.7% | 0.1% | 0.3% |

### Stramenopiles (`stramenopiles_odb10`, n=100) — rnaSPAdes

| Metric | 6 Samples | 12 Samples | 24 Samples | 36 Samples | 48 Samples | 65 Samples |
|---|---|---|---|---|---|---|
| Complete (C) | 95.0% | 95.7% | 99.0% | 96.7% | 98.4% | 98.4% |
| Complete & Single-copy (S) | 74.0% | 43.9% | 33.0% | 26.4% | 18.9% | 19.7% |
| Complete & Duplicated (D) | 21.0% | 51.8% | 66.0% | 70.3% | 79.5% | 78.8% |
| Fragmented (F) | 4.0% | 3.9% | 1.0% | 2.9% | 1.1% | 1.4% |
| Missing (M) | 1.0% | 0.4% | 0.0% | 0.4% | 0.4% | 0.1% |

---

## Salmon Mapping Rates

All 65 samples were mapped back to each tiered assembly using `Salmon` in quasi-mapping mode with `--validateMappings`. Values shown are the mean mapping rate across the 65 samples.

| Assembler | 12 Samples | 24 Samples | 36 Samples | 48 Samples | 65 Samples |
|---|---|---|---|---|---|
| Trinity | 91.0% | 97.7% | 92.8% | 96.0% | 96.9% |
| rnaSPAdes | 97.2% | 97.2% | 97.0% | 96.5% | 96.9% |

> **Note:** The 6-sample pilot mapping rates are not shown here because they were evaluated against the 6 pilot samples rather than the full 65-sample set. See the previous Results page for those values.

---

## Functional Annotation (EnTAP)

EnTAP was run on the MMseqs2-filtered assemblies using `uniprot_sprot` for similarity searching and EggNOG for ortholog / gene family assignment. For the tiered runs, percentages are reported relative to the total input sequences to EnTAP; the 6-sample columns retain the percentages from the pilot run for reference but use a different denominator and are not directly comparable.

### Annotation Summary — Trinity

| Metric | 6 Samples | 12 Samples | 24 Samples | 36 Samples | 48 Samples | 65 Samples |
|---|---|---|---|---|---|---|
| Total input sequences | 67,058 | 88,078 | 118,431 | 93,919 | 105,721 | 118,086 |
| Total annotated sequences | 19,595 | 22,752 | 26,372 | 27,555 | 32,856 | 36,399 |
| Total ORFs predicted | 33,132 | 39,960 | 47,565 | 48,720 | 57,303 | 62,105 |
| Complete ORFs | 13,743 (41.5%) | 14,451 (36.2%) | 15,939 (33.5%) | 17,324 (35.6%) | 20,614 (36.0%) | 23,332 (37.6%) |
| Internal ORFs | 8,625 (26.0%) | 11,863 (29.7%) | 15,527 (32.6%) | 14,868 (30.5%) | 16,226 (28.3%) | 17,419 (28.0%) |
| Partial ORFs | 10,764 (32.5%) | 13,712 (34.3%) | 16,208 (34.1%) | 16,620 (34.1%) | 20,505 (35.8%) | 21,410 (34.5%) |
| Sequence search hits (UniProt) | 33.2% | 8.4% | 7.1% | 9.8% | 10.9% | 10.6% |
| EggNOG family assignments | 100% | 25.8% | 22.3% | 29.3% | 31.1% | 30.8% |
| GO terms (UniProt) | 32.4% | 4.5% | 4.2% | 5.6% | 6.7% | 7.2% |
| KEGG assignments (EggNOG) | 43.7% | 7.5% | 6.7% | 8.7% | 9.4% | 9.4% |
| Contaminants flagged | 0% | 0% | 0% | 0% | 0% | 0% |

### Annotation Summary — rnaSPAdes

| Metric | 6 Samples | 12 Samples | 24 Samples | 36 Samples | 48 Samples | 65 Samples |
|---|---|---|---|---|---|---|
| Total input sequences | 39,126 | 41,905 | 49,141 | 53,218 | 60,949 | 67,352 |
| Total annotated sequences | 17,497 | 22,016 | 25,604 | 28,132 | 33,639 | 36,612 |
| Total ORFs predicted | 35,567 | 32,739 | 38,130 | 41,538 | 49,753 | 53,701 |
| Complete ORFs | 15,343 (43.1%) | 16,481 (50.3%) | 17,247 (45.2%) | 17,435 (42.0%) | 20,971 (42.1%) | 21,998 (41.0%) |
| Internal ORFs | 8,796 (24.7%) | 6,119 (18.7%) | 7,767 (20.4%) | 8,610 (20.7%) | 9,724 (19.5%) | 11,413 (21.3%) |
| Partial ORFs | 11,428 (32.1%) | 10,295 (31.4%) | 13,361 (35.0%) | 15,816 (38.1%) | 19,175 (38.5%) | 20,404 (38.0%) |
| Sequence search hits (UniProt) | 31.2% | 17.7% | 17.1% | 17.6% | 19.1% | 18.3% |
| EggNOG family assignments | 100% | 52.5% | 52.1% | 52.9% | 55.2% | 54.4% |
| GO terms (UniProt) | 30.4% | 8.4% | 8.9% | 9.5% | 11.3% | 12.3% |
| KEGG assignments (EggNOG) | 43.7% | 15.0% | 15.1% | 15.4% | 16.5% | 16.4% |
| Contaminants flagged | 0% | 0% | 0% | 0% | 0% | 0% |

### EggNOG Taxonomic Scope — Trinity

Proportion of family-assigned sequences mapping to each taxonomic level. The Bacillariophyta share is a rough indicator of how diatom-specific the assembled transcript pool is.

| Taxonomic Scope | 6 Samples | 12 Samples | 24 Samples | 36 Samples | 48 Samples | 65 Samples |
|---|---|---|---|---|---|---|
| Bacillariophyta | 49.0% | 50.0% | 46.0% | 45.3% | 40.1% | 37.2% |
| Eukaryota | 19.8% | 19.0% | 19.8% | 20.6% | 21.9% | 20.7% |
| Fungi | 6.6% | 6.0% | 6.0% | 6.4% | 7.7% | 12.7% |
| Streptophyta | 4.3% | 4.4% | 5.0% | 4.5% | 4.8% | 4.5% |
| Metazoa | 4.4% | 4.4% | 4.7% | 5.0% | 5.8% | 5.4% |

### EggNOG Taxonomic Scope — rnaSPAdes

| Taxonomic Scope | 6 Samples | 12 Samples | 24 Samples | 36 Samples | 48 Samples | 65 Samples |
|---|---|---|---|---|---|---|
| Bacillariophyta | 49.7% | 52.9% | 50.0% | 47.9% | 42.2% | 39.2% |
| Eukaryota | 20.0% | 19.4% | 19.9% | 20.2% | 21.4% | 20.2% |
| Fungi | 6.2% | 5.1% | 5.0% | 6.0% | 7.3% | 12.2% |
| Streptophyta | 4.0% | 3.4% | 3.7% | 4.1% | 4.6% | 4.4% |
| Metazoa | 4.3% | 4.1% | 4.6% | 4.7% | 5.5% | 5.2% |
