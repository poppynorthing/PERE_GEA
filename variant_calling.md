# variant calling
[fill in]

# Generating statistics on raw variant file:

Adopted from [jrick course guide](https://jessicarick.github.io/bioinformatics-for-conservation/docs/folder/5-variant-filtering/) & using VCFtools v0.1.16 (citation). 

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
