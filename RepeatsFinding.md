Users can use the RepeatFinding.sh scripts to identify repeats.
To use the shell, the two programs PerM and Sim\_Solexa must be in your path.

## Input ##
  * Transcriptom in fasta format
  * Reference file in fasta, index or a txt which is a list of fa file
  * read lengths
  * number of mismatches
  * Y/N Y if the reference is transcriptom. N if the reference is genome

## Meta file names ##
> To save disk space, the scripts keep generate meta file correspond to a region and delete them after the region is done.

  * Index file
  * Simulated reads
  * Simulated reads list
  * mapping log file

Note the repeats file output itself could be still too big, if the reference is too repetitive or too big.

## Output ##
> ### transcriptom to transcriptom ###
> ### transcriptom to genome ###

## Limitations ##
> The meta file's path is currently hard coded. Which means running both command "Y" and "N" will overwrite each others meta file. To avoid that, the two run should be in two different directly with a link to the parameter 1 and 2 in the current dirrectly.

## Running Time ##
We experience a 2 days running time for RefSEQ to hg19 with 16 CPU and 15GB memory.
The scripts take advantage of multiple CPU in both simulate and mapping reads.