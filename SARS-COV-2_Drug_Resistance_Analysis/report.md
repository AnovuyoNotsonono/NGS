## **Analysis of Mutations in SARS-CoV-2 PLpro and Potential Impact on Inhibitor Interaction**

**Introduction**

This study was motivated by the work of Ferreira et al. (2023) “Identification of novel allosteric sites of SARS-CoV-2 papain-like protease (PLpro) for the development of COVID-19 antivirals” which highlighted several PLpro residues — T10, D12, T54, Y72, Y83, S212, Y213, Y251, K254, and Y305 — as critical for the protein’s enzymatic activity. The study demonstrated that inhibitors such as GRL0617 show promising efficacy by targeting these residues.

Our study aimed to identify naturally occurring mutations in the PLpro region that could potentially interfere with inhibitor binding.

**Methods**

Reference Sequences

The reference for coordinates of the orf1ab polyprotein, which contains PLpro, was obtained from NCBI Reference Sequence: YP_009724389.1. The residue span of PLpro was determined by aligning the PLpro fasta sequence from the reference using BLAST against the PDB structure 8UOB, corresponding to the known PLpro structure.

*Sequencing Data Processing*

The raw sequencing dataset used in this study was downloaded from NCBI SRA, including Tiled_ClickSeq experiments for SARS-CoV-2:

SRR25743479_1.fastq.gz / SRR25743479_2.fastq.gz

SRR25743480_1.fastq.gz / SRR25743480_2.fastq.gz

SRR25743481_1.fastq.gz / SRR25743481_2.fastq.gz

SRR25743503_1.fastq.gz / SRR25743503_2.fastq.gz

These paired-end reads were processed using Nextflow with the nf-core/viralrecon pipeline:

```bash
nextflow run nf-core/viralrecon \
  -profile conda \
  --max_memory '10GB' \
  --max_cpus 8 \
  --input samplesheet.csv \
  --outdir results/viralrecon \
  --protocol amplicon \
  --fasta 'nCoV-2019.reference.fasta' \
  --gff 'GCA_009858895.3_ASM985889v3_genomic.200409.gff' \
  --primer_bed 'nCoV-2019.primer.bed' \
  --primer_set artic \
  --skip_kraken2 \
  --skip_assembly \
  --skip_pangolin \
  --skip_nextclade \
  --skip_asciigenome \
  --platform illumina \
  -resume
 ```
This workflow enabled high-throughput processing of Illumina sequencing reads, including primer trimming, variant calling, and annotation. Specific modules such as Kraken2, assembly, Pangolin, Nextclade, and AsciiGenome were skipped to focus the analysis on variant identification. The -resume flag allowed resuming from previously completed steps, ensuring efficiency and reproducibility.

**Variant Analysis**

Variant calling results were filtered and compiled into filtered_variants.csv and variants_long_table.csv. Only missense mutations were considered for downstream analysis relevant to PLpro activity.

**Results**

A total of 96 missense mutations were identified, with 68 located within the orf1ab polyprotein. Among these, two mutations were located within PLpro at functionally significant residues:

*Y72C* — within the BL2 loop, previously identified as critical for inhibitor binding.

*E252G* — another residue known to affect PLpro conformation and activity.

These findings suggest that these regions are prone to mutations that could disrupt the binding of inhibitors like GRL0617.

**Discussion**

The observed mutations at Y72 and E252 may influence PLpro’s structural stability and its interaction with inhibitors. Further studies are warranted, including:

*Molecular docking* to compare binding affinities of wild-type versus variant PLpro with inhibitors.

*2D interaction analysis* to assess the effect on hydrogen bonds and hydrophobic contacts.

*Molecular dynamics simulations* to evaluate the impact of these mutations on protein flexibility and stability.

These analyses could provide a mechanistic understanding of how mutations compromise inhibitor efficacy and inform future antiviral development.

*The table below summarizes the key PLpro mutations:*

| Residue (Reference) | Mutation | Location/Functional Region  | Potential Impact on Inhibitor Binding                              |
| ------------------- | -------- | --------------------------- | ------------------------------------------------------------------ |
| Y72                 | Y72C     | BL2 loop                    | May disrupt GRL0617 binding; alters local hydrogen bonding network |
| E252                | E252G    | Catalytic/Allosteric region | Could affect loop flexibility and inhibitor interaction            |

**Conclusion**

This study highlights critical mutations in PLpro that may influence inhibitor binding, supporting further structure-based drug design and mutational analysis.

**References**

Ferreira JC, Villanueva AJ, Al Adem K, Fadl S, Alzyoud L, Ghattas MA, Rabeh WM. Identification of novel allosteric sites of SARS-CoV-2 papain-like protease (PLpro) for the development of COVID-19 antivirals. [Journal/Preprint details].

Di Tommaso P, et al. Nextflow enables reproducible computational workflows. Nat Biotechnol. 2017;35:316–319.

nf-core/viralrecon: A community curated bioinformatics pipeline for SARS-CoV-2 variant analysis. Available: https://nf-co.re/viralrecon

NCBI Reference Sequence: YP_009724389.1.

Protein Data Bank: 8UOB.

BLAST: Basic Local Alignment Search Tool. Altschul SF, et al. J Mol Biol. 1990;215:403–410. Available: https://blast.ncbi.nlm.nih.gov/

NCBI SRA dataset: SRR25743479, SRR25743480, SRR25743481, SRR25743503 (Tiled_ClickSeq of SARS-CoV-2)
