# The general transcription regulator SPT5 stabilizes RNA polymerase II, orchestrates transcription cycles, and maintains the enhancer landscape

Primary sequencing data and BigWig files are deposited at the GEO depository: GSE180845.

**ChIP-Rx analysis**

The paired-end ChIP-Rx reads were processed with Trim Galore v0.6.6 (https://www.bioinformatics.babraham.ac.uk/projects/trim_galore/) to remove adaptors and low-quality reads with the parameter “-q 25” and then aligned to the human hg19 and mouse mm10 assemblies using Bowtie v2.3.5.1 with default parameters (Langmead and Salzberg, 2012). All unmapped reads, low mapping quality reads (MAPQ < 30) and PCR duplicates were removed using SAMtools v1.9 (Li et al., 2009) and Picard v2.23.3 (https://broadinstitute.github.io/picard/). The number of spike-in mm10 reads was counted with SAMtools v1.9 and normalization factor alpha = 1e6/mm10_count was calculated (Li et al., 2009). Normalized bigwig was generated with deeptools v3.5.0 and reads mapped to the ENCODE blacklist regions were removed using bedtools v2.29.2 (Amemiya et al., 2019; Quinlan and Hall, 2010; Ramirez et al., 2016). Peaks were called using macs2 v2.2.6 with a q-value threshold of 0.05 and peak annotation was performed with ChIPseeker R package v1.24.0 (Yu et al., 2015; Zhang et al., 2008).

**PRO-seq analysis**

Raw reads were processed as described for ChIP-Rx above with read length > 15 bp and ribosomal RNA reads were removed using Bowtie v2.3.5.1 with “--un-conc-gz” (Langmead and Salzberg, 2012). The remaining reads were aligned to hg19 or mm10 genome using Bowtie v2.3.5.1 with “--local --sensitive-local” (Langmead and Salzberg, 2012). Removal of low mapping quality reads and duplicated reads and calculation of normalization factor were performed in the same way as described in ChIP-Rx. Strand-specific PRO-seq signal normalized by spike-in reads was generated by deeptools v3.5.0 (Ramirez et al., 2016). 

**TT-seq analysis**

The TT-seq reads were first trimmed as described in ChIP-Rx, followed by mapping to human hg19 and mouse mm10 assemblies using STAR v2.7.5c with parameters “-outFilterMultimapNmax 1 -alignSJoverhangMin 8 -alignSJDBoverhangMin 1 -outFilterMismatchNmax 999 -outFilterMismatchNoverLmax 0.02 -alignIntronMin 20 -alignIntronMax 1000000 -alignMatesGapMax 1000000 -outSAMtype BAM SortedByCoordinate” (Dobin et al., 2013). SAMtools v1.9 (Li et al., 2009) was used to quality filter SAM files with “-q 7 -f 2” and duplicated reads were removed using Picard v2.23.3 (https://broadinstitute.github.io/picard/). The counts of spike-in mm10 reads were used to generate normalization factors for coverage profiles.

**RNA-seq analysis**

Raw reads were trimmed as described in ChIP-Rx and then aligned to human hg19 and mouse mm10 assemblies using STAR v2.7.5c with parameter “--outSAMtype BAM SortedByCoordinate --twopassMode Basic --outFilterMismatchNmax 2 --outSJfilterReads Unique” (Dobin et al., 2013). Read normalization was based on 1e6/spike-in read number calculated by SAMtools v1.9 and normalized bigwig files were built with deeptools v3.5.0 (Li et al., 2009; Ramirez et al., 2016). Gene expression quantification was performed with featureCounts tool from the Rsubread R package v2.0.1 (Liao et al., 2019). For each gene, we computed the number of fragments per kilobase of exon per million mapped reads (FPKM) by normalizing the spike-in mm10 reads. Differential expression analysis was performed using DESeq2 R package v1.26.0 (Love et al., 2014).

**ATAC-seq analysis**

After trimming the adaptors and low-quality reads as described for ChIP-Rx above, the remaining reads were aligned to human (hg19) genome assembly using Bowtie 2 v2.3.5.1 with parameters “-N 1 -L 25 -X 2000 --no-mixed --no-discordant” (Langmead and Salzberg, 2012). Only uniquely mapping reads with a mapping quality MAPQ > 10 were used for further analysis. Reads counts adjusted to reads per kilobase per million mapped reads (RPKM) using deeptools v3.5.0 and peaks were called by macs2 v2.2.6 (Ramirez et al., 2016; Zhang et al., 2008). Differential accessibility analysis between dTAG- and DMSO-treated cells on the ATAC-seq peaks was performed with the DiffBind R package v2.16.2 (Ross-Innes et al., 2012). 