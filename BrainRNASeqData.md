We are building a pipeline which uses PerM to process RNA-seq data and estimate the expression level of each gene/isoform.
  1. The putative transcripts are enumerated from the mapped hg18 regions with the splicing junctions supported by short reads.
  1. These putative transcripts are filtered by a similar scoring scheme adopted from the [AceView](http://www.ncbi.nlm.nih.gov/IEB/Research/Acembly/) data base.
  1. The RNA-seq reads are mapped again to the enumerated transcripts.
  1. The mapping results are sent to **TranSEQ** program, Dr. Xiting Yan's (unpublished) program for expression levels estimation.

The preliminary [results](http://dw408k-1.cmb.usc.edu:8080/Gene_List.htm) can be browsed on the UCSC [genome browser](http://genome.ucsc.edu/), with the following steps.

  * Follow the hyperlink to check a gene in a chosen chromosome.
  * Click the `Expression` link of each gene to see the mapped-reads coverage on the UCSC genome browser.
  * Switch the reference to hg18 by clicking the `Convert` button on the top tool bar and choosing the `New Assembly` to `Mar.2006`
  * Go back to gene list page and click the selected `Isoforms` of the gene to see the transcripts we enumerated.
  * Copy the isoform sequence and [BLAT](http://genome.ucsc.edu/cgi-bin/hgBlat?hgsid=148468955) it on the genome browser to see it with the mapped reads signal.
  * The expressed proportion and the expressed log likelihood of a transcript is shown on the isoforms Id.
  * The log likelihood shows the confidence that the transcript is expressed. Inf log likelihood indicates there are reads uniquely mapped to the isoform.
  * The result can be compared with the [AceView](http://www.ncbi.nlm.nih.gov/IEB/Research/Acembly/)'s transcripts by [BLAT](http://genome.ucsc.edu/cgi-bin/hgBlat?hgsid=148468955)ing the result together.
  * The result of nucleotide sequence and the amino sequence should be [BLAT](http://genome.ucsc.edu/cgi-bin/hgBlat?hgsid=148468955) separately.