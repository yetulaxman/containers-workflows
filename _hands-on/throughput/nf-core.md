---
topic: nextflow
title: (Bonus) Tutorial1 - Hello-world example
---

nf-core is a community effort to collect a curated set of analysis pipelines built using Nextflow. Here we use nfcore/atacseq as an example pipeline for ATAC-seq data.

Here is an example batch script to run the pipeline on Puhti:

#!/bin/bash
#SBATCH --time=01:00:00
#SBATCH --partition=small
#SBATCH --account=project_xxxx
#SBATCH --cpus-per-task=4
#SBATCH --mem-per-cpu=4000

export TMPDIR=$PWD
export SINGULARITY_TMPDIR=$PWD
export SINGULARITY_CACHEDIR=$PWD
unset XDG_RUNTIME_DIR

# Activate  Nextflow on Puhti
module load bioconda
source activate nextflow

# Nextflow command here
nextflow run nf-core/atacseq -r 1.2.1 -profile test,singularity -resume

copy and paste the above script to a file named atacseq.sh and replace your project number with project_xxxx in slurm directives.

Finally, submit your job

sbatch atacseq.sh
