---
topic: bioapplications
title: Tutorial2 - deepvariant example
---

### Deepvariant pipeline as developed by GoogleAI ###
Run [deepvariant method](https://github.com/google/deepvariant) to perform variant calling on WGS and WES data sets in Puhti supercomputing environment using deepvariant singularity container


## How to get started
One needs the DeepVariant programs (https://github.com/google/deepvariant) and some test data. One can download data from Pouta object storage  (or from google bucket)  Storage.

### Prepare deepvariant singulairty image from docker image

One needs to get deepvariant docker image, models and test data in order to run the pipeline. Additionally, other prerequisites for running deepvariant method includes 1) obtaining A reference genome in [FASTA](https://en.wikipedia.org/wiki/FASTA_format) format and its corresponding index file (.fai). 2) An aligned reads file in [BAM](http://genome.sph.umich.edu/wiki/BAM) format and its corresponding index file (.bai).

#### _Convert docker image to singularity_ ####

Log in to Puhti using your CSC credentials

Go to your subdirectory in the project scratch directory (created in last exercise):
```
cd /scratch/project_xxxx/$USER
```
Singularity is only available in compute nodes. We can use **sinteractive** to build the 
Singularity image in an interactive session.
```
sinteractive -i
```
Choose the course project. Set memory to 4 GB. You can use defaults for other parameters.

We want to use LOCAL_SCRATCH for Singularity tmp and cache. Unsetting XDG_RUNTIME_DIR will 
silence some unnecessary warnings.
```
export SINGULARITY_TMPDIR=$LOCAL_SCRATCH
export SINGULARITY_CACHEDIR=$LOCAL_SCRATCH
unset XDG_RUNTIME_DIR
singularity build deepvariant_cpu_1.2.0.sif docker://google/deepvariant:1.2.0
exit
```
Download test data 
```
wget https://a3s.fi/containers-workflows/deepvariant_testdata.tar.gz
tar -xavf deepvariant_testdata.tar.gz

```

### Prepare slurm scripts to run on Puhti (e.g., deepvariant_puhti.sh)

```
#!/bin/bash
#SBATCH --time=00:05:00
#SBATCH --partition=small
#SBATCH --account=project_xxxx
#SBATCH --ntasks=1
#SBATCH --cpus-per-task=1
#SBATCH --mem=4000

singularity_wrapper exec deepvariant_cpu_1.2.0.sif \
/opt/deepvariant/bin/run_deepvariant \
--model_type=WGS   --ref=./testdata/ucsc.hg19.chr20.unittest.fasta \
--reads=./testdata/NA12878_S1.chr20.10_10p1mb.bam \
--regions "chr20:10,000,000-10,010,000" \
--output_vcf=$PWD/output.vcf.gz \
--output_gvcf=$PWD/output.g.vcf.gz
```
