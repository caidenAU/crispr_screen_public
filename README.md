# crispr_screen
### For preparing and analyzing crispr screening sgRNA library, and downstream analysis

# Library analysis with MAGeCK
For MAGeCK Walkthrough and explanation 
```
https://hpc.nih.gov/apps/MAGeCK.html 
```

MAGeck requires an index of sgRNAs. We are using the brunello library which can be downloaded in BioHPC with the following command
```
wget https://sourceforge.net/projects/mageck/files/libraries/broadgpp-brunello-library-corrected.txt.zip/
```
This will save the file to the current working directory
Next, unzip with the following command
```
gunzip broadgpp-brunello-library-corrected.txt
```
Next, save raw fastq.gz files from NGS core using
```
wget --no-parent --recursive --user=USERNAME --password='PASSWORD' ftp://ngsdata.swmed.org/FILE_NAME
```
The raw sequencing data comes as a fastq.gz file, unzip it with
```
gunzip file1.fastq.gz
```
Next, load in MAGeCK
```
module load mageck/0.5.8
```
Next run the following command, replacing the file name with the unzipped file you downloaded
```
mageck count -l broadgpp-brunello-library-corrected.txt -n escneg --trim-5 23 --sample-label "custom_library_name" --fastq file1.fastq
```
This will execute MAGeCK and the terminal will begin populating with the progress, allow it to run as long as it needs. Will take a couple minutes depending on the number of reads generated from your sequencing run.

The output should look something like this
```
INFO  @ Tue, 10 Oct 2023 16:00:33: Summary of file 09072023raw.fastq: 
INFO  @ Tue, 10 Oct 2023 16:00:33: giniindex	0.0692491044352 
INFO  @ Tue, 10 Oct 2023 16:00:33: totalsgrnas	77441 
INFO  @ Tue, 10 Oct 2023 16:00:33: mappedreads	10516885 
INFO  @ Tue, 10 Oct 2023 16:00:33: reads	84987914 
INFO  @ Tue, 10 Oct 2023 16:00:33: label	09 
INFO  @ Tue, 10 Oct 2023 16:00:33: zerosgrnas	78 
```
This is saying that the library index contains 77441 sgRNAs, and that from our input file, 10516885 reads mapped. From this, only 78 sgRNAs were not present in our preparation. This represents (77441-78) / 77441 = 99.899% coverage, which is more than sufficient. 

The script will out a variety of log files in the curret working directory
```
escneg_countsummary.R
escneg.countsummary.txt
escneg.log
escneg.count_normalized.txt
escneg_countsummary.Rnw
escneg.count.txt
```
**escneg.log** is a text file containing the terminal output

**escneg.count_normalized.txt** is a tab delimited file with rows for each sgRNA and columns explaining the gene name and file. May be useful for seeing which genes come back with zero sgRNAs targeting.


