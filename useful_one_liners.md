### Some useful commands and one-liners for future reference

Calculate a number of mapped reads for paired-end alignments excluding secondary and chimeric alignments
Obtained from Biostar post: https://www.biostars.org/p/138116/

```

samtools view -F 0x4 <bam> | cut -f 1 | sort | uniq | wc -l

```

Calculate a number of mapped reads for single-end alignments

```

samtools view -F 0x904 -c <bam>

```

Calculate genome coverage with bedtools, output as bedgraph, ignore bases with 0 coverage

```

bedtools genomecov -ibam <sorted.bam> -bg > <bedgraph>

```

Dowload data from SRA archive

```

# Search for a certain project PRJNA* and get run info file
esearch -db sra -query PRJNA560453 | efetch -format runinfo > info.csv
# get SRR ids from run info file and save them in ids.txt file
# Prefetch read files, otherwise the download will be really slow
cat ids.txt | xargs -n 1 prefetch $1
# Download SRA files, convert to fastq and split into paired-end files
cat ids.txt | xargs -n 1 fastq-dump $1 --split-files

```

### Find the column index of the text file in bash

```

head -n 1 <file_name> | tr '\t' '\n' | cat -n | grep <matching_pattern>

```
### Print all lines till the end of the file after the matching pattern

```
sed -n '/<pattern>/,$p' <filename>
```
### Unzip multiple *.gz* files in parallel:

```
parallel -j 46 "gunzip {}" ::: *.fastq.gz
```
### Get sequence length distribution profile with awk
```
awk 'NR%4 == 2 {lengths[length($0)]++} END {for (l in lengths) {print l,
lengths[l]}}' adaptor-trimmed_inputfile.fastq
```

### Convert uppercase fasta to lowercase while ignoring ids

```
cat mirna_mm10.fasta | sed -r '/^[ACTG]/s/[A-Z]+/\^C0/g'
```
### Use trim_galore and parallel to trim multiple fastq pairs

```
find  path_to_fastq  -name "*_R1_001.fastq.gz" | parallel -j 1 trim_galore --paired --fastqc -o trim_galore/ {} {= s/_R1_/_R2_/ =}
```
### Generate length distribution of reads based on a fastq file
#### From: https://www.biostars.org/p/72433/#72439

```
awk 'NR%4 == 2 {lengths[length($0)]++} END {for (l in lengths) {print l, lengths[l]}}' file.fastq
```

