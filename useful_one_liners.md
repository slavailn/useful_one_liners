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

