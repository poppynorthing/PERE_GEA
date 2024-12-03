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
cp *pere_chrs* ./reference

#Align reads to reference chromosomes. 
bwa mem reference/pere_chrs sample_R1 sample_R2 > sample.sam
```
Then, several steps were taken to format the alignment files and generate statistics on mapping rates of each sample from the alignment file. These steps were taken using samtools vX (citation), and the last line of code was adopted from [Jessica Rick](https://jessicarick.github.io/bioinformatics-for-conservation/docs/folder/3-read-alignment/).
```
#Convert sam to bam

for sam in *.sam; do samtools view -b -o $sam.bam $sam ; done

#Rename bam's

for i in *.bam; do mv "$i" "`echo $i | sed "s/.sam././"`"; done

#Sort bam
for bam in *.bam; do samtools sort -o $bam.sorted.bam $bam ; done

#Rename sorted bams

for i in *.sorted.bam; do mv "$i" "`echo $i | sed "s/.bam././"`"; done

#index bam files for variant calling
for sortbam in *.sorted.bam; do samtools index $sortbam ; done

# Generate mapping statistics from every bam

for bam in *.sorted.bam; do
  echo $bam
  base=`basename $bam .sorted.bam` # remove the file ending to get just the individual ID
  reads=`samtools flagstat $bam | grep total | cut -f 1 -d' '` # extract just the total raw reads
  mapped=`samtools flagstat $bam | grep mapped | head -n 1 | cut -f 1 -d' '` # extract just the number of mapped reads
  echo "${bam},${reads},${mapped}" >> filtered_bam_stats.csv # output these numbers into a text file
done
```
