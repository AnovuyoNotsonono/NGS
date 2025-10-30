# SARS-CoV-2 PLpro Mutation Analysis

## Overview
This repository contains the results and workflow for the study:

**Identification of mutations in SARS-CoV-2 papain-like protease (PLpro) that could affect inhibitor interactions.**  

The study was motivated by Ferreira JC, Villanueva AJ, Al Adem K, Fadl S, Alzyoud L, Ghattas MA, Rabeh WM. *Identification of novel allosteric sites of SARS-CoV-2 papain-like protease (PLpro) for the development of COVID-19 antivirals.* Critical PLpro residues such as **T10, D12, T54, Y72, Y83, S212, Y213, Y251, K254, and Y305** were previously identified as essential for activity. Inhibitors like **GRL0617** have demonstrated promising efficacy.

The aim of this project was to identify mutations on PLpro that could interfere with inhibitor binding.

---

## Repository Structure

SARS-COV-2_Drug_Resistance_Analysis/
├── results/
│ └── viralrecon/
│ ├── variants/
│ │ ├── ivar/
│ │ │ ├── filtered_variants.csv
│ │ │ └── variants_long_table.csv
│ └── ...
├── report.md
├── samplesheet.csv
├── nCoV-2019.reference.fasta
├── GCA_009858895.3_ASM985889v3_genomic.200409.gff
├── nCoV-2019.primer.bed
└── README.md

```sql
- **filtered_variants.csv**: Contains all filtered mutations and their annotated effects (missense, nonsense, etc.).
- **variants_long_table.csv**: Full list of variants across the analyzed samples.
- **samplesheet.csv**: Input metadata for nf-core/viralrecon.
- **reference files**: FASTA, GFF, and primer BED files used for alignment and variant calling.

---
```

## Methods

### Variant Calling Workflow
Variants were identified using the **nf-core/viralrecon** Nextflow workflow:

```bash
nextflow run nf-core/viralrecon -profile conda \
    --max_memory '10GB' --max_cpus 8 \
    --input samplesheet.csv \
    --outdir results/viralrecon \
    --protocol amplicon \
    --fasta 'nCoV-2019.reference.fasta' \
    --gff 'GCA_009858895.3_ASM985889v3_genomic.200409.gff' \
    --primer_bed 'nCoV-2019.primer.bed' \
    --primer_set artic \
    --skip_kraken2 --skip_assembly --skip_pangolin --skip_nextclade --skip_asciigenome \
    --platform illumina -resume
```
