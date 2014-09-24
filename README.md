RNA-SEQ MAPPING PIPELINE



0. Tasks
========

This pipeline performs the following tasks:
- create a directory structure
- perform quality control on FastQ files (using FastQC)
- align reads of each sample in a run against reference genome (using STAR)
- perform quality control on generated BAM files (using Qualimap)
- count reads in features (using HTSeq-count)
- normalize read counts (using DESeq)


1. Running the pipeline
=======================

Run by typing: 
perl /hpc/cog_bioinf/common_scripts/RNA_seq_analysis/scripts/RNAseqAnalyse.pl -input [/path/to/Unaligned/projectdir] - output [/path/to/store]

CAUTION: Paths to input and output directories must be absolute paths!

To see additional parameters, type:
perl /hpc/cog_bioinf/common_scripts/RNA_seq_analysis/scripts/RNAseqAnalyse.pl -h


2. Additional tools
===================

Please contact Annelies (F.A.S.Smouter@umcutrecht.nl) if you want to add additional tools/scripts/options.


3. Detecting fusion genes
=========================

The pipeline searches for fusion genes / chimeric junctions by default. The default minimum segment length is 1 and can be edited with option -chimSegmentMin.


4. Read counts
==============

The raw read counts table can be found in the directory: <rundir>/read_counts/<run>_merged_counts.txt. This table contains ENSEMBL gene IDs (rows) and the raw read 
counts per library (columns). The counting is done by htseq-count.


5. Normalized read counts
=========================

The raw read counts are normalized using the DESeq method included in the DESeq Bioconductor package and is based on the hypothesis that most genes are not DE. A DESeq scaling factor for
a given lane is computed as the median of the ratio, for each gene, of its read count over its geometric mean across all lanes. The underlying idea is that non-DE
genes should have similar read counts across samples, leading to a ratio of 1. Assuming most genes are not DE, the median of this ratio for the lane provides an
estimate of the correction factor that should be applied to all read counts of this lane to fulfill the hypothesis. By calling the estimateSizeFactors() and sizeFactors()
functions in the DESeq Bioconductor package, this factor is computed for each lane, and raw read counts are divided by the factor associated with their sequencing lane. 

Differential expression analysis can be performed using this normalized table.