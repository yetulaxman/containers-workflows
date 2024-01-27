---
topic: snakemake
title: Tutorial9 - Sankemake with singularity containers
---

Snakemake workflow, which is described in terms of rules that define how to create output files from input files, is one of the popular workflow managers in the bioinformatics community. Snakemake is available as a module in Puhti environment. And also, Snakemake can be easily installed in the user's own disk space (e.g., Projappl directory) if you need to have a specific version for your scientific workflows.


## Run jobs inside a singularity container

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

```

> Note: If you want to install a specific version of Snakemake along with other python packages, we recommend installing  using pip. This way, one can use
  singularity containers smoothly in Snakemake workflow.
