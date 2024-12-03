# Admixture analysis

Fractional ancestry assignments were calculated using ADMIXTURE vX (citation). 

```
mkdir admix_out

for k in {1..14}; do echo "running for K=$k" ; admixture --cv pere_variants_pruned.bed $k ; done
mv *pere_variants_pruned* ./admix_out
```
