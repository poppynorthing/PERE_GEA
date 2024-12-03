# Variant filtering and linkage pruning

Raw variants were filtered using VCFtools vX (citation), using the following criteria: only biallelic SNPs with data present for > 20% of individuals, 
a minor allele frequency of > 5%, and a mean site depth of < 75x. 

```
vcftools --vcf pere_raw_variants.vcf \
  --max-missing 0.2 \
  --max-meanDP 75 \
  --maf 0.05 \
  --min-alleles 2 \
  --max-alleles 2 \
  --remove-indels \
  --recode \
  --out pere_raw_variants_filtered 
```

For admixture analysis, SNPs were linkage pruned in 10kb windows (r2 > 0.15) using Plink vX (citation). 

```
plink --vcf pere_raw_variants_filtered.vcf --indep-pairwise 10 5 0.15 --out pere_variants_pruned --double-id --keep-allele-order --allow-extra-chr 0 --make-bed

# Reformat chromosome numbers in bed file for admixture-appropriate formatting
for i in *.bim ; do sed -i 's/pere_chr_//g' $i ; done
```
