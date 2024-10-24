# Read Processing and Alignment

After de-multiplexing and removing barcodes with SnoWhite v2.0.3 (Dlugosch et al. 2013), reads were filtered and trimmed using <i>process_radtags</i> v2.60 from Stacks.

```
process_radtags -1 sample_R1.fastq -2 sample_R2.fastq -o ./stacks_out -c -q -r -e pstI --renz-2 mseI
```

Reads were then aligned to the P. recurvata reference chromosomes (Northing et al. <i>in review</i>) using the <i>mem</i> function in BWA (citation).
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
