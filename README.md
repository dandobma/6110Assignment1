# 6110Assignment1
This repository contains the entirety of Assignment 1 for BINF 6110, Genomic Methods for Bioinformatics

Introduction


	Genome assembly is conducted using many fragments of DNA sequences, and determining their position in relation to each other to create a representation of the genome from which they originated (Pevsner, 2009). First, sequence fragments, called reads, are read by a sequencing machine, with sequences constructed from overlaps (Pevsner, 2009). Once this process is completed, these sequences can be aligned with scaffolds, non-contiguous sequences separated by gaps of known length (Waterson, 2002). This is done so that complete genomes can be annotated and compared against for variant detection (Li et al, 2010). 
	
	Several challenges complicate genome assembly. Repetitive genomic regions can cause ambiguity when multiple reads align equally well to different locations in the genome (Treangen & Salzberg, 2012). If these regions span the length of entire reads, assemblers can have difficulty resolving their correct placement (Pevzner et al, 2001). Sequencing errors also complicate assembly, as incorrect bases obscure true overlaps and create spurious connections in assembly (Nagarajan & Pop, 2013). Uneven sequencing coverage, a result of underrepresented genomic loci in the sequencing data, can cause missing or poorly assembled regions (Miller et al, 2010).
	
	Bacterial genomes, such as Salmonella enterica, are comparatively well suited for de novo assembly (Didelot et al, 2012). They are typically small (approximately 5Mb), generally lack introns, and contain fewer repetitive elements than eukaryotic genomes (Land et al, 2015). These characteristics reduce assembly complexity and increase the likelihood of producing contiguous assemblies (Koren & Phillippy, 2015). This is not to say that the previously mentioned issues cannot still arise in genomic areas with repetitive elements, mobile genetic regions, or biased nucleotide composition (Treangen & Salzburg, 2012).
	
	Long read sequencing (LRS) has several disadvantages when compared to short read sequencing (SRS), such as elevated substitution and indel error rates (Amarasinghe et al, 2020). Oxford Nanopore using R10 chemistry is an improved version of LRS which has improved on the previous limitations of LRS when compared to SRS, and allows for reads of up to tens of kilobases in length (Wick et al, 2010). This will decrease the likelihood of repetitive regions and structural variants causing issues with scaffold assembly (Koren & Phillippy, 2015). Despite this, it would be prudent to choose assemblers specifically designed to tolerate higher error rates. 
	
Following de novo assembly, comparison to a reference genome allows for accuracy evaluation (Nagarajan & Pop, 2013). It will also detect SNPs, additions, deletions, and larger structural variants (Alkan et al, 2011). In this way, it serves as both a quality control step as well as an analysis (Miller et al, 2010). This is not without its limitations, as it can introduce reference bias, favouring sequences present in the reference genome while underrepresenting novel or divergent regions (Paten et al, 2014).

Our workflow has several advantages and disadvantages. It leverages the strengths of long-read Oxford Nanopore sequencing for rapid de novo genome assembly of Salmonella enterica (Koren & Phillippy, 2015). The use of long reads is expected to improve assembly contiguity by spanning repetitive regions and resolving structural complexity that would challenge short-read sequencing, benefiting from the reduced error rates associated with R10 chemistry (Wick et al., 2021). Despite these advantages, limitations remain, including residual sequencing errors—particularly insertions and deletions—which may affect variant calling accuracy (Amarasinghe et al., 2020). Additionally, reliance on a reference genome may obscure novel genomic information unique to the assembled strain due to reference bias (Paten et al., 2017).



Proposed Methods


Consensus Genome Assembly

	Raw Nanopore FASTQ reads generated using R10 chemistry will be assembled de novo using Flye (v2.9), a genome assembler optimized for long, error-prone reads (Kolmogorov et al, 2019). The expected genome size will be set to 5Mb, consistent with known Salmonella enterica genomes (Land et al, 2015), and in all other parameters the defaults will be used.
	
Reference Genome Acquisition

The reference genome for Salmonella enterica will be downloaded from the NCBI RefSeq database as a FASTA file and associated annotation files (if available). These genomes are curated and non-redundant, making them suitable for comparative analysis (O’Leary et al, 2016). 

Genome Alignment and Variant Calling

	The assembled consensus genome will be aligned to the reference genome using Minimap2 (v2.26), using the --nano-raw command due to the raw reads supplied from which to create our consensus genome. Minimap2 is widely used for long-read data and provides efficient and accurate whole-genome alignments (Li, 2018).
	
Following alignment, variants relative to the reference genome will be identified. Alignment files will be converted and sorted using SAMtools (v1.20) (Li et al, 2009). Variant calling will be performed using bcftools (v1.20) to identify single nucleotide polymorphisms (SNPs) and small insertions and deletions (indels) (Danacek et al, 2021). Variant filtering will be applied to remove low-confidence calls, accounting for Nanopore-specific error profiles.

Visualization and Validation

Genome alignments and called variants will be visualized using IGV (Integrative Genomics Viewer) to enable manual inspection of variant calls and assessment of alignment quality across the genome (Robinson et al, 2011). Visualization will focus on regions containing dense variant clusters, indels, or potential misassemblies. This step will serve as an additional validation of the consensus genome and provide insight into genomic differences between the assembled strain and the reference.



References


Alkan, C., Coe, B. P., & Eichler, E. E. (2011). Genome structural variation discovery and genotyping. Nature Reviews Genetics, 12(5), 363–376. https://doi.org/10.1038/nrg2958
Amarasinghe, S. L., Su, S., Dong, X., Zappia, L., Ritchie, M. E., & Gouil, Q. (2020). Opportunities and challenges in long-read sequencing data analysis. Genome Biology, 21, 30. https://doi.org/10.1186/s13059-020-1935-5
Danecek, P., Bonfield, J. K., Liddle, J., et al. (2021). Twelve years of SAMtools and BCFtools. GigaScience, 10(2), giab008. https://doi.org/10.1093/gigascience/giab008
Didelot, X., Bowden, R., Wilson, D. J., Peto, T. E. A., & Crook, D. W. (2012). Transforming clinical microbiology with bacterial genome sequencing. Nature Reviews Genetics, 13(9), 601–612. https://doi.org/10.1038/nrg3226
Kolmogorov, M., Yuan, J., Lin, Y., & Pevzner, P. A. (2019). Assembly of long, error-prone reads using repeat graphs. Nature Biotechnology, 37, 540–546. https://doi.org/10.1038/s41587-019-0072-8
Koren, S., & Phillippy, A. M. (2015). One chromosome, one contig: Complete microbial genomes from long-read sequencing and assembly. Current Opinion in Microbiology, 23, 110–120. https://doi.org/10.1016/j.mib.2014.11.014
Land, M. L., Hyatt, D., Jun, S.-R., et al. (2015). Insights from 20 years of bacterial genome sequencing. Functional & Integrative Genomics, 15, 141–161. https://doi.org/10.1007/s10142-015-0433-4
Li, H. (2018). Minimap2: Pairwise alignment for nucleotide sequences. Bioinformatics, 34(18), 3094–3100. https://doi.org/10.1093/bioinformatics/bty191
Li, H., Handsaker, B., Wysoker, A., et al. (2009). The Sequence Alignment/Map format and SAMtools. Bioinformatics, 25(16), 2078–2079. https://doi.org/10.1093/bioinformatics/btp352
Li, R., Zhu, H., Ruan, J., Qian, W., Fang, X., Shi, Z., Li, Y., Li, S., Shan, G., Kristiansen, K., Yang, H., Wang, J., & Wang, J. (2010). De novo assembly of human genomes with massively parallel short read sequencing. Genome Research, 20(2), 265–272. https://doi.org/10.1101/gr.097261.109
Miller, J. R., Koren, S., & Sutton, G. (2010). Assembly algorithms for next-generation sequencing data. Genomics, 95(6), 315–327.
Nagarajan, N., & Pop, M. (2013). Sequence assembly demystified. Nature Reviews Genetics, 14(3), 157–167. https://doi.org/10.1038/nrg3367
O’Leary, N. A., Wright, M. W., Brister, J. R., et al. (2016). Reference sequence (RefSeq) database at NCBI: Current status, taxonomic expansion, and functional annotation. Nucleic Acids Research, 44(D1), D733–D745. https://doi.org/10.1093/nar/gkv1189
Paten, B., Novak, A. M., Eizenga, J. M., & Garrison, E. (2017). Genome graphs and the evolution of genome inference. Genome Research, 27(5), 665–676. https://doi.org/10.1101/gr.214155.116
Pevsner, J. (2009). Bioinformatics and functional genomics (2nd ed.). Wiley-Blackwell.
Pevzner, P. A., Tang, H., & Waterman, M. S. (2001). An Eulerian path approach to DNA fragment assembly. Proceedings of the National Academy of Sciences of the United States of America, 98(17), 9748–9753. https://doi.org/10.1073/pnas.171285098
Robinson, J. T., Thorvaldsdóttir, H., Winckler, W., et al. (2011). Integrative Genomics Viewer. Nature Biotechnology, 29, 24–26. https://doi.org/10.1038/nbt.1754
Treangen, T. J., & Salzberg, S. L. (2012). Repetitive DNA and next-generation sequencing: Computational challenges and solutions. Nature Reviews Genetics, 13(1), 36–46. https://doi.org/10.1038/nrg3117
Waterston, R. H. (2002). On the sequencing of the human genome. Proceedings of the National Academy of Sciences of the United States of America, 99(6), 3712–3716. https://doi.org/10.1073/pnas.042692499
Wick, R. R., Judd, L. M., & Holt, K. E. (2021). Performance of neural network basecalling tools for Oxford Nanopore sequencing. Genome Biology, 22, 129. https://doi.org/10.1186/s13059-021-02355-5

