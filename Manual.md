If you have any usage questions, please email "yanghoch at usc dot edu".

## CONTENTS ##
### [Compiling](http://code.google.com/p/perm/wiki/Compiling) ###
### [Input Files](http://code.google.com/p/perm/wiki/Input) ###
### [Output Files](http://code.google.com/p/perm/wiki/Output) ###
### [Limitations and Future work](http://code.google.com/p/perm/wiki/Limitations) ###
### [Algorithms ](http://code.google.com/p/perm/wiki/Algorithms) ###
### [FAQ ](http://code.google.com/p/perm/wiki/FAQ) ###
## SYNOPSIS ##
To use the command line, type PerM with the args in the order.

For single-end reads: `PerM <Ref> <Reads> [options]`

For [example](http://code.google.com/p/perm/wiki/single_end_example):
  * `./PerM Ref.fasta Reads.fasta -v 5 -o out.mapping -u ummappedReads.fa`
  * `./PerM RefFilesList.txt ReadsSetFilesList.txt -v 5 -u unmappedReads.fa -E `
  * `./PerM Ref.fasta Reads.csfasta -v 5 -m -s my.index --delimiter ',' --seed F3`
  * `./PerM my.index SingleEndReads.csfasta -v 5 -o out.sam -k 10 -a ambiguous10.csfasta`

For paired-end reads: ` PerM <Ref> -1 <F3_Reads> -2 <R3_Reads> [options] `

For [example](http://code.google.com/p/perm/wiki/paired_end_example):
  * `./PerM ref.fa -1 F3.fa -2 R3.fa -U 3000 -L 100 -v 5 -A -m -s -o out.sam`
  * `./PerM ref.txt -1 F3.fq -2 R3.fq -v 5 -m -s my.index -o out.mapping --seed F3`
  * `./PerM my.index -1 F3.fq -2 R3.fq -U 3000 -L 100 -v 5 -A -o out.sam`

To build an index only: ` PerM <Ref> <Read Length> --readFormat {.csfasta | .fasta} -m -s <index path> --seed F3`

For example:
  * `./PerM hg18.txt 50 --readFormat .csfasta -m -s hg18_50_SOLiD.index`

## Command Line Options ##
#### Required Arguments ####
  1. The reference file should be in FASTA format with the either the **.fasta**, **.fna**, or **.fa** file extension. For a transcriptome with multiple genes or isoforms as reference, concatenate all FASTA sequences in a single FASTA file. Alternatively, if there are many files, for example one per chromosome, ex: chr1.fa to chrY.fa, list the FASTA filenames one per line in a file which has the **.txt** extension. The **.txt** is important because PerM examines the file extension to know if the input file is a list of filenames. The filenames need to include the file path (relative or absolute) unless the FASTA files are all in the same directory that PerM is run from.
  1. The read file(s) should be in the .fasta, .fastq, .csfasta or .csfastq format. PerM parses a file according to its extension, or the format explicitly specified by the **`--readFormat <format>`** flag. If there are multiple read files, list each filename, one per line, in a .txt file. PerM takes it as the input and can map multiple read sets in parallel by [OpenMP](http://en.wikipedia.org/wiki/OpenMP).

#### Short Options (grouped by related functionality) ####
| **-A** | | Output **all** alignments within the mismatch threshold (see **-v** option), end-to-end.|
|:-------|:|:----------------------------------------------------------------------------------------|
| **-B** |  | Output **best** alignments in terms of mismatches in the threshold (see **-v** option). For example, if a read has no perfect match alignments, two single base mismatch alignments, and additional alignments with more mismatches, only the two single base mismatch alignments will be output. **-B** is the default mode if neither **-A** or **-B** is specified. |
| **-E** |  | Output only uniquely mapped reads _remaining after the **best** down selection has been applied if applicable_. When combined with the **-A** option, only reads with a single alignment within the mismatch threshold (see **-v** option) will be output. |

| **-v** | # | Maximum number of mismatches allowed (or allowed in each end for pair-end reads). The default value is the number of mismatches that the seed used is fully sensitive to.|
|:-------|:--|:-------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **-k** | # | Specifies maximum number of alignments to output. The default value is 200 if the -k flag is not given. Alignments for reads mapping to more than the maximum number positions will not be output. Use the -a option to collect reads which exceeded the maximum.|

| **-t** | # | Number of bases at the 5' end of each read to ignore. For example, if the first 5 bases are used as a barcode or to index multiple samples together, use -t 5. If not specified, no initial bases will be ignored. |
|:-------|:--|:-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **-T** | # | Number of bases in each read to use, starting after any bases ignored by -t option. Later bases at the 3' of the read are ignored. For example, -T 30 means use only first 30 bases (signals) after the any bases ignored due to the -t option. |

| **-m** | | Create the reference index without reusing the saved index even if available. |
|:-------|:|:------------------------------------------------------------------------------|
| **-s** | _path_ | Save the reference index to accelerate the mapping in the future. If _path_ is not specified, the index will be created in the current working directory (i.e. where PerM is run from) using the default index name. If _path_ is a directory, the index will be created in the specified directory using the default index name (directory must exist; it will not automatically be created). If _path_ is a file path, the index will be created with the specified name. |

| **-o** | _filepath_ | Name of mapping output file when mapping a single read set. The output file format will be either the .mapping tab delimited text format or the SAM format as determined by the extension of the output filename. For example `-o out.sam` will output in SAM format; `-o /path/to/out.mapping` will output in .mapping format. Use **--outputFormat** to override this behavior. The **-o** option does not apply when multiple reads sets are being mapped at once to take advantage of multiple CPUs (cores); see the **-d** option for that case. |
|:-------|:-----------|:------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **-d** | _dirpath_ | Output directory for mapping output files when mapping multiple read sets (output files will be named automatically). If the directory specified does not exist, the output directory will be created provided the parent directory exists. If the **-d** switch is not specified, files will be written to the directory PerM is run from. Note: if -d _filepath_ is specified when mapping a single read set, _dirpath_ will be prepended to _filepath_; however, this usage is not recommended. |
| **-a** | _filepath_ | Create a FASTA (FASTQ) file for reads mapped to more positions than the threshold specified by -k or the default of 200. |
| **-b** | _filepath_ | Create a FASTA (FASTQ) file for reads that is shorter than expected length or with strange characters. |
| **-u** | _filepath_ | Create a FASTA (FASTAQ) file of unmapped reads. When a single read set is mapped, _filename_ specifies the name of the output file. When multiple read sets are mapped, _filename_ is irrelevant and should be omitted; the files of unmapped sequences will automatically be named and created in the directory PerM is run from. |

#### Long Options ####
| **--ambiguosReadOnly**| | Output only ambiguous mapping to find repeats (similar regions within substitution threshold). When this option is specified, reads that mapped to over mapping number threshold that specified by -k will still be printed. |
|:----------------------|:|:-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **--ambiguosReadInOneLine**|  | Output reads mapped to more than k places in one line. When this option is specified, reads that mapped to over mapping number threshold specified by -k will still be printed but printed in one line.|
| **--noSamHeader**|  | Do not include a SAM header. This makes it easier to concatenate multiple SAM output files. |
| **--includeReadsWN**| # | Map reads with equal or fewer N or '.' bases than the specified threshold by encoding N or '.' as A or 3. Reads with more 'N' will be discarded. The default setting discards read with any 'N'. |
| **--statsOnly** |  | Output the mapping statistics to stdout only, without saving alignments to files. |
| **--ignoreQS** |  | Ignore the quality scores in FASTQ or QUAL files. |
| **--printNM** |  | When quality scores are available, use this flag to print number of mismatches, instead of mismatch scores in mapping format. |
| **--seed** | {F<sub>0</sub> | F<sub>1</sub> | F<sub>2</sub> | F<sub>3</sub> | F<sub>4</sub> | S<sub>11</sub> | S<sub>20</sub> | S<sub>12</sub>} | Specify the seed pattern. The F<sub>0</sub>, F<sub>1</sub>, F<sub>2</sub>, F<sub>3</sub>, and F<sub>4</sub> seeds are fully sensitive to 0-4 mismatches respectively. The S<sub>11</sub> S<sub>20</sub> S<sub>12</sub> seeds are designed for the SOLiD sequencer.  An S<sub>kj</sub> seed is full sensitive to k adjacent mismatch pairs (SNP signature is color space) and j isolated mismatches.  See [the algorithm page](http://code.google.com/p/perm/wiki/Algorithms) for more information about the seed patterns.|
| **--refFormat** | _{fasta | list | index }_| Assume references sequence(s) are in the specified format, instead of guessing according to file's extension. |
| **--readFormat**|_{fasta | fastq | csfasta | csfastq}_| Assume reads are in the specified format, instead of guessing according to the file(s)' extension. |
| **--outputFormat**|_{ sam | mapping }_| Override the default output mapping format option or specify it explicitly when the output file extension is not .sam or .mapping. |
| **--delimiter**|_char_| _char_ is a character used as the delimiter to separate the the read id, and the additional info in the line with '>' when reading a FASTA or CSFASTA file. |
| **--log**|_filepath_| _filepath_ specifies the name of the log file which contains the mapping statistics that will also be printed on the screen. |
| **--forwardOnly**|  | Map reads to the forward strand only: (This is for SOLiD Strand specific sequencing). |
| **--reverseOnly**|  | Map reads to the reverse strand only: (This is for SOLiD Strand specific sequencing) |



[Examples](http://code.google.com/p/perm/wiki/Examples)

#### Options for Paired-end Reads ####
PerM deals with mate-paired reads by mapping each end separately. All combinations of mated pairs mapping to the same reference sequence will be output if their separation are in the allowed ranged as specified by the **-L** and **-U** flags.

| **-e**| | Exclude ambiguous paired. |
|:------|:|:--------------------------|
| **-L** / **--lowerBound** |_Int_| lower bound for mate-paired separation distance |
| **-U** / **--upperBound** |_Int_| upper bound for mate-paired separation distance |

The upper bound and lower bound can be negative, which may catch the re-arrangement variations. Use the ` -A ` argument to avoid missing the correct pairs. However, this may greatly increase the running time if both ends are in repetitive regions.
| **--fr** | | Map paired-end reads to different strand only |
|:---------|:|:----------------------------------------------|
| **--ff** |  | Map paired-end reads to the same strand only |
| **--printRefSeq** |  | Print the mapped reference paired sequence as the two last columns in .mapping format. |
The default option output mapping in both the same or different strand.

[Examples](http://code.google.com/p/perm/wiki/Examples)

#### Default settings ####
The following are the default settings when the corresponding command line option is not specified. Please specify the option to change the default settings.
  * Allow only two mismatches only in each end and use seed F<sub>2</sub> S<sub>11</sub> or F<sub>3</sub> ,selected according to the read lengths and types.
  * Print the best alignments for each read in terms of the number of mismatches.
  * Output files in `*`.mapping format.
  * Searches for a saved index with the default file name before building the new index.
  * Won't save the index in file, unless `-s` is specified.
  * For paired end reads, the default allowed separation distance is 0-3000 bp. Change with the `-L` and `-U` options.

## Parallel Mapping ##
PerM simultaneously maps multiple reads sets in a list by querying the same index. It will detect how many CPUs (cores) are available and assign each of them a read set. If a read set is done, the next read set in the list will be processed automatically. Each read set will have its own mapping output file. To better utilize all CPUs on a node, large reads set should be split into many small read sets and put in a list.
When multiple nodes are used in the same file system, the index should be pre-built first by one node; the other nodes will read the pre-built index without building index again. Without pre-built index, each machine will try to build its own index, wasting CPU time and storage space.

## Exit codes ##
PerM sets the exit code to 0 upon successful completion, the normal Unix behavior. If the program is terminated via Ctrl-C (SIGINT), the exit code will be 2, the number for SIGINT (see `man kill`). If you invoke PerM from another language, you can check the return code and do something intelligent. Here is a Perl pseudo-code example:

```
while (... some sort of loop ...) {
  my $cmd = "PerM ... arguments and switches";
  my $ec = system($cmd);
  if ($ec == 2) {
    print STDERR "PerM terminated via Ctrl-C. Stopping run.\n\n";
    # Maybe do some cleanup such as deleting the small files the read file was
    # split into for parallel processing.
    exit($ec);
  }
}
```

## Use PerM on Galaxy ##
Thanks to Prof Anton Nekrutenko and Kelly Vincent at PSU, you can now use PerM on [Galaxy's \*test\* server](http://test.g2.bx.psu.edu/). Follow the hyperlink to Galaxy's page, and click NGS:Mapping in the tool menu. Please choose _Map with PerM for SOLiD and Illumina_. You can upload your own reference or use the pre-built hg19 index in the system. Please email me if you encounter any difficulties. Once the system proves its stability, it will be moved to Galaxy's main server with more pre-built reference index.

## Unit Test ##
When PerM was developed, a unit cppUnit test module was also prepared. If you are interested in the test code for PerM, please email me.