PerM is recently used in publication in [Cell](http://ac.els-cdn.com/S0092867411015157/1-s2.0-S0092867411015157-main.pdf?_tid=8f85357a-1dcf-11e3-b37c-00000aab0f6c&acdnat=1379226485_43b98693c6c25ca40531f6a2aaadb473)!  Please check [Google Schalar](http://scholar.google.com/scholar?hl=en&lr=&cites=6422116322208841799&um=1&ie=UTF-8&ei=QPtYT8K7NJToiALStcnSCw&sa=X&oi=science_links&ct=sl-citedby&resnum=9&sqi=2&ved=0CGUQzgIwCA) to see more citations.

Version 0.4.0 solved the issue that stop parsing reads when encounter reads that shorter than expected.
[More recent updates](http://code.google.com/p/perm/wiki/RecentUpdated)!!
## PerM ##
PerM is a software package which was designed to perform highly efficient genome scale alignments for hundreds of millions of short reads produced by the _ABI SOLiD_ and _Illumina_ sequencing platforms. Today PerM is capable of providing **full** sensitivity for alignments within **4** mismatches for 50bp SOLID reads and **9** mismatches for 100bp Illumina reads.

## Usage ##
The reference sequence(s) can be whole genomes with multiple chromosomes, the transcriptome or even the millions reads in the fasta format, separated by '>'. The reads can be in the fasta, fastq, csfasta + QUAL formats or fastq for _SOLiD_ reads. PerM can output alignments in our mapping format or the SAM format and that output can be further processed by [ComB](http://code.google.com/p/comb),  [SAMtools](http://samtools.sourceforge.net/), [RseqFlow](http://genomics.isi.edu/rnaseq) pipeline and the [Galaxy's \*test\* server](http://test.g2.bx.psu.edu/).
Check the [manual](http://code.google.com/p/perm/wiki/Manual) for more detail.

## Algorithm and Performance ##
With its special periodic spaced seeds, PerM can be fully sensitive to four mismatches, and highly sensitive to higher numbers of mismatches. This seed matching method has speed advantages in longer read (although limited to 64bp currently), non-mappable reads (for fixed number of shift and checking) and in the genome scale mapping due to the high seed weight. PerM is about **37** million reads per CPU hour, full sensitive to **3** mismatches and highly sensitive to more than 3 mismatches for 50bp SOLiD reads. PerM can build the reference index in parallel; it takes half hour to build the human genome index with 16 CPUs and 14 GB memory.

## SNP Calling ##
http://dw408k-1.cmb.usc.edu:8080/new.PNG
Use our SNP calling tool [ComB](http://code.google.com/p/comb),
which is much more accurate than SAMtools for SOLiD reads.

## Splice Junctions Detection ##
http://dw408k-1.cmb.usc.edu:8080/new.PNG
You can also use PerM to detect known splice junctions. Check [here](http://code.google.com/p/perm/wiki/DetectSplicedJuncitons) for details.
To detect novel alternative splice junctions, try our new tool [clippers](http://code.google.com/p/clippers/), which targets long, novel and non-cardinal splice junctions or deletions, with 100bp or longer Illumina reads.

## System Requirements ##
PerM uses 4.5 bytes memory per base to index the reference genome. The memory usage does not dependent on the number of reads. Thus PerM requires 2GB to map reads to the human transcriptom (400 M bases)and 14 GB of memory to map reads to the human genome (3 G bases). Multiple read sets can be mapped simultaneously with multiple CPUs (Cores)using [OpenMP](http://openmp.org/wp/) to look up the shared memory index. Users can use [iPerM](http://code.google.com/p/perm/wiki/iPerM), our wrapper, on a smaller memory computer or use [qPerM](http://code.google.com/p/perm/wiki/qPerM) to map one read set in parallel.

## Citation ##
Please cite our publication in the [Bioinformatics](http://bioinformatics.oxfordjournals.org/cgi/content/full/25/19/2514) journal, _Chen Y, Souaiaia T, Chen T. PerM: Efficient mapping of short sequencing reads with periodic full sensitive spaced seeds. Bioinformatics, 2009, 25 (19): 2514-2521_.

## Development Team ##
This tool was developed by [Ting Chen](http://www.cmb.usc.edu/people/tingchen/)'s [group](http://tingchenlab.cmb.usc.edu/), [Center of Excellence in Genomic Sciences](http://cegs.usc.edu/) at the [University of Southern California](http://college.usc.edu/).
**Please email Yangho Chen (_yanghoch at usc.edu_,), so I can put you in the PerM mailing list for any new updates.** All suggestions are welcomed.


![https://lh3.googleusercontent.com/-ittJAjt0Jvg/ToD4r9NL-RI/AAAAAAAACpk/I6z-o7bK-m8/s512/mudruncrop.jpg](https://lh3.googleusercontent.com/-ittJAjt0Jvg/ToD4r9NL-RI/AAAAAAAACpk/I6z-o7bK-m8/s512/mudruncrop.jpg)



![http://www.usc.edu/assets/usc/img/01/usc-name-gold-cardinal.gif](http://www.usc.edu/assets/usc/img/01/usc-name-gold-cardinal.gif)


![http://www.cmb.usc.edu/images/image.jpg](http://www.cmb.usc.edu/images/image.jpg)