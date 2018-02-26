## Pipeline for running checkm on UW-Madison's CHTC or an HTCondor system


This pipeline includes scripts for running checkm on a group of genomes and getting back 
a group of files for each genome containing the completeness and redundancy results.


### Input
- `chtc-checkm.sh` - the script for generating the output files below
- `chtc-checkm.sub` - the submission script for generating the output files below
- `bins_to_classify.txt` - list of files to run checkm on, example below:
```
2236347068.fna
2236347069.fna
2236661000.fna
2236661001.fna
2236661002.fna
2236661003.fna
2236661004.fna
```
- `refs/` - directory to store all the `.fna` files below


### Output

Resulting files where `genomename` stand for whatever was before `.fna` in the fasta file checkm was run on
- `genomename.checkm.lineage.txt` - The results of the lineage specific run of checkm.
- `genomename.taxon.arch.txt` - The results running the taxon specific option with the Archaeal marker gene set.
- `genomename.taxon.bact.txt`- The results running the taxon specific option with the Bacterial marker gene set.

Header for all of these files is `genome taxon genome_length completeness redundancy` and can be found in the `output.header.txt` file.









