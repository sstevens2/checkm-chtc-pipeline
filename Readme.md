## Pipeline for running checkm on UW-Madison's CHTC or an HTCondor system

### Running the pipeline 

This pipeline includes scripts for running checkm on a group of genomes and getting back 
a group of files for each genome containing the completeness and redundancy results.  This pipeline is written for an [HTCondor system](https://research.cs.wisc.edu/htcondor/), specifically [UW-Madison's CHTC's HPC cluster](http://chtc.cs.wisc.edu/) using this [docker image for checkm](https://hub.docker.com/r/sstevens/checkm/).

#### Input
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


#### Output

Resulting files where `genomename` stand for whatever was before `.fna` in the fasta file checkm was run on
- `genomename.checkm.lineage.txt` - The results of the lineage specific run of checkm.
- `genomename.taxon.arch.txt` - The results running the taxon specific option with the Archaeal marker gene set.
- `genomename.taxon.bact.txt`- The results running the taxon specific option with the Bacterial marker gene set.

Header for all of these files is `genome taxon genome_length completeness redundancy` and can be found in the `output.header.txt` file.


#### To run it

1. Clone this repository to your home folder on CHTC.
```
git clone https://github.com/sstevens2/checkm-chtc-pipeline
```
2. Move all of your `.fna` genome/bin/MAG/SAG files into the cloned repo.
3. Make the `bins_to_classify.txt` file by `ls`ing the `.fna` files
	- Checkm requires they end in the extention `.fna`.  Code below for fixing the extensions. 
		```
		for filename in *.fa; do cp $filename ${filename%.fa}.fna; done
		## rm *.fa  # run this if the above looks like it worked
		```
	- If you are submitting more than 10,000 genomes, you will have to split them up into separate submissions because this is the max you can submit to one job on CHTC.
	- Do not include the `refs/` part of the path if the files are in the `refs` directory already.
```
ls *.fna > bins_to_classify.txt
```
or
```
cd refs/
ls *.fna > ../bins_to_classify.txt
cd ..
```
4. Move the `.fna` files to a directory called `refs/`, if they already aren't there.
```
mv *.fna refs/
```
5. Submit the jobs
```
condor_submit chtc-checkm.sub
```


### What if I want to get all the checkm results not just the results parsed above?

If there is additional info you want from the checkm results for each lineage specific run, you can use the `chtc-checkm-all.sub` and `chtc-checkm-all.sh` scripts.


Inputs are the same as above but with the `all` versions of the scripts.  And you can run it with the same directions as above, except the submit command is changed to:
```
condor_submit chtc-checkm-all.sub
```

#### Output

- `genomename_checkm.tgz` -  A zipped folder of all the checkm output for that run.


### Other things

Send me an email or submit an issue if you have questions or issues!


### Acknowledgements

Thanks to Alex Linz for the scripts to start with, testing my checkm docker, and feedback.






