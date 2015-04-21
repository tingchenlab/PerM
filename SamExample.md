You can use [samtools](http://samtools.sourceforge.net/) to obtain a consensus sequence or to tally up (i.e. pileup) the number of reads falling in each sequence interval where the intervals may represent say genes or RNAs. To get the expression level of RNA-seq from SAM file, I wrote a simple bash shell wrapper [wrapper](http://perm.googlecode.com/files/pileUp.sh) to combine multiple steps into one command. After you get the wig file with signal for each base-pair, you can further condense it to bed with my python scripts [wig2Bed.py](http://perm.googlecode.com/files/Wig2Bed.py). The output can be be displayed in a genome browser as shown in the example [web pages](http://dw408k-1.cmb.usc.edu:8080/GeneList.htm) on my computer, after proper headers line are added.

To detect the junctions, you can first


[Back to output](http://code.google.com/p/perm/wiki/Output)

[Back to manual](http://code.google.com/p/perm/wiki/Manual)