# Read Processing and Alignment

Reads were demultiplexed and filtered using <i>process_radtags</i> v2.60 from Stacks (citation) and subsequently trimmed using Trim Galore vX (citation).

```
#Demultiplex and filter reads
process_radtags -1 rawsequences_R1.fastq \
-2	rawsequences_R2.fastq \
-b 	barcodes.tsv \
-r -c -q \
-e pstI --renz-2 mseI --disable-rad-check \
--inline-inline \
-o demultiplexed_reads

#Trim reads
trim_galore --fastqc --paired -q 20 -o trimmed_reads/ sample_R1.fq sample_R2.fq
```
The quality of the demultiplexed reads before and after trimming was assessed using FastQC (citation) and visualized with MultiQC (citation).

```
# Run FastQC on demultiplexed reads 
fastqc ./demultiplexed reads/*
multiqc ./demultiplexed reads

# Run Fast QC on trimmed reads
fastqc ./trimmed_reads/*
multiqc ./trimmed_reads
```

Reads were then aligned to the <i>P. recurvata</i> reference chromosomes (Northing et al. <i>in review</i>) using the <i>mem</i> function in BWA (citation).
```
#First, make an index of the reference chromosomes that we align reads to
bwa index -p pere_chrs pere_chrs.fasta

mkdir reference
cp *pere_chrs* ~/reference

#Align reads to reference chromosomes. 
bwa mem reference/pere_chrs sample_R1 sample_R2 > sample.sam

#Sort reads and convert to bam
samtools sort ...
```
