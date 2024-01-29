---
topic: snakemake
title: Tutorial9 - Sankemake with singularity containers (WIP)
---

Snakemake workflow, which is described in terms of rules that define how to create output files from input files, is one of the popular workflow managers in the bioinformatics community. Snakemake is available as a module in Puhti environment. And also, Snakemake can be easily installed in the user's own disk space (e.g., Projappl directory) if you need to have a specific version for your scientific workflows.


## Run snakemake rule inside of a singularity container

Singularity/Apptainer is istalled on login and compute nodes on CSC computers. so no need to install it separately. Use * --use-singularity* flag to activate singularity environement and bind mount necessary disk space when usig snakemake workflows. This example relies on the user having some method for obtaining docker images that can be converted to singularity images. This example uses a publicly available dockerhub Fastqc image. A major caveat of using dockerhub is that if you run this example on a widely use cluster it is possible you will get an error.

```bash
module load snakemake/7.17.1
snakemake -s test3.smk      -j 1     --latency-wait 60     --use-singularity --singularity-args "-B /scratch/project_2001659/yetukuri/snakemake_workflow:/scratch/project_2001659/yetukuri/snakemake_workflow"   \
--cluster "sbatch -t 10 --account=project_xxx --job-name=fastqc-help  --tasks-per-node=1 --cpu


Contents of snakemake-singularity.smk are as below:

```bash

rule all:
        input: "fastqc_help.txt"

rule say_hello:
        output: "fastqc-help.txt"
        singularity: "docker://biocontainers/fastqc:v0.11.9_cv8"
        shell:
                """
                fastqc --help > "fastqc-help.txt"
                """
```

> Note: If you want to install a specific version of Snakemake along with other python packages, we recommend installing  using pip. This way, one can use
  singularity containers smoothly in Snakemake workflow.

you can also define your profiles in a config file (config.yaml) and run the snakemake workflow as below:
```bash
cluster:
  mkdir -p logs/{rule} &&
  sbatch
    --cpus-per-task={threads}
    --job-name=smk-{rule}-{wildcards}
    --output=logs/{rule}/{rule}-{wildcards}-%j.out
jobs: 10
printshellcmds: True
jobscript: slurm_jobscript.sh
use-conda: False
use-singularity: True
```
and one can run workflow as below:

```
snakemake --profile
```

##  Run snakemake workflow with local singularity container
One can run build a singularity container (--with fakeroot) option CSC supercomputer and use that image to run snakem ake workflow on the top of snakemake file as below:

```
singularity: "image/turotial.sif"

```

batch script to run the workflow:

```
#!/bin/bash
#SBATCH --job-name=myTest
#SBATCH --account=project_2001659
#SBATCH --time=00:10:00
#SBATCH --mem-per-cpu=2G
#SBATCH --partition=test
#SBATCH --cpus-per-task=4

module load snakemake/7.17.1
snakemake -s Snakefile  --use-singularity  -j 4
```
## Minimial script for building singularity image on Puhti

One can use the following script and conda environment defined in the tutorial.yaml file and build a singualirty image from it. One ca use the built image to run the snakemake workflow instead of installing the required tools either locally or usig tykky wrapper tool.
```
Bootstrap : docker
From :  continuumio/miniconda3
IncludeCmd : yes

%files
tutorial.yml

%post
apt-get update && apt-get install -y procps && apt-get clean -y
/opt/conda/bin/conda env create -n snakemake_env -f /environment.yml
/opt/conda/bin/conda clean -a


%environment
export PATH=/opt/conda/bin:$PATH
. /opt/conda/etc/profile.d/conda.sh
conda activate snakemake_env

%runscript
echo "This is as tutorial singularity/appatainer image"
```


Build the image as below:

```bash
singularity build --fakeroot tutorial.sif tutorial.def 
```
