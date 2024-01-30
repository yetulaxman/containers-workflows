---
topic: snakemake
title: Tutorial7 - Sankemake hello-world example
---
Snakemake workflow, which is described in terms of rules that define how to create output files from input files, is one of the popular workflow managers in the bioinformatics community. The workflow manager enables scalable and reproducible scientific pipelines as a series of chained rules. One can share these reproducible workflows with a fully-specified software environment. 

Snakemake is available as a module in Puhti supercomputing environment. And also, it can be easily installed in the user's own disk space (e.g., Projappl directory) if you need to have a specific version for your scientific needs.This hands-on exercise introduces hello-world example using a pre-installed module on Puhti

## Running Snakemake workflow with a pre-installed module on Puhti

Hello-world example, Snakefile (with a capital S and no file extension) has the following content:

```bash

rule all:
        input: "CAPITAL_CASE.txt"

rule say_hello:
        output: "smaller_case.txt"
        shell:
                """
                echo "greetigs from csc to snakemake community" > smaller_case.txt
                """
rule capitalise:
        input: "smaller_case.txt"
        output: "CAPITAL_CASE.txt"
        shell:
                """
                tr '[:lower:]' '[:upper:]' < {input} > {output}
                """
```

Snakemake is installed as a module on Puhti and can be used as below: 

```bash
module load snakemake/7.17.1
snakemake --help   #  to get information on more options.
snakemake -s Snakemake \ # the Snakemake filename is the default name; no need specify with -s flag
 -j 1  \     # this will execute up to 3 tasks in parallel)       
--latency-wait 60 \  # snakemake to wait up to 60 seconds after a job completes for the output files to become available.
--cluster "sbatch -t 10  --account=project_xxx --job-name=hello-world --tasks-per-node=1 --cpus-per-task=1 
--mem-per-cpu=4000 -p test"
# cluster option to execute snakemake workflow on cluster given other options for slurm
```
