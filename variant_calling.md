# Variant Calling
Variants were called from the alignments (and indices) generated in read_processing_alignment.md using <i>mpileup</i> from BCFtools vX (citation). 

```
bcftools mpileup -Ou -a AD,DP -d 1000 -f pere_chrs.fa ../alignment/final_alignments_subset/*.sorted.bam | bcftools call -m -v -Ov -o pere_raw_variants.vcf
```

# Generating statistics on raw variant file:

Variant statistics were generated using multiple commands from vcftools vX (citation). Code below adopted from [Jessica Rick](https://jessicarick.github.io/bioinformatics-for-conservation/docs/folder/5-variant-filtering/). 

```
# mean depth per individual
vcftools --vcf pere_raw_variants.vcf --depth --out vcf_stats/pere_raw_variants

# mean depth per site
vcftools --vcf pere_raw_variants.vcf --site-mean-depth --out vcf_stats/pere_raw_variants

# missingness per individual
vcftools --vcf pere_raw_variants.vcf --missing-indv --out vcf_stats/pere_raw_variants

# missingness per site 
vcftools --vcf pere_raw_variants.vcf --missing-site --out vcf_stats/pere_raw_variants

# heterozyosity and Fis per individual 
vcftools --vcf pere_raw_variants.vcf --het --out vcf_stats/pere_raw_variants

```
