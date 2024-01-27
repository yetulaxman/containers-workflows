---
topic: snakemake
title: Tutorial7 - Sankemake hello-wrold example
---

Snakemake workflow, which is described in terms of rules that define how to create output files from input files, is one of the popular workflow managers in the bioinformatics community. Snakemake is available as a module in Puhti environment. And also, Snakemake can be easily installed in the user's own disk space (e.g., Projappl directory) if you need to have a specific version for your scientific workflows.


## Running Snakemake with a pre-installed module on Puhti

Snakemake is installed as a module on Puhti and can be used as below: 


```bash
module load snakemake/7.17.1
snakemake -s test.smk      -j 1     --latency-wait 60   --cluster "sbatch -t 10 \
--account=project_2001659 --job-name=fastqc-help  --tasks-per-node=1 --cpus-per-task=1 \
--mem-per-cpu=4000 -p test"
```

In the above script, contents of nextflow script, *test.smk* are as below:

```bash

rule all:
        input: "CAPITALISE.txt"

rule say_hello:
        output: "smaller_case.txt"
        shell:
                """
                echo "testing toy example for snakemake workflow" > smaller_case.txt
                """
rule capitalise:
        input: "smaller_case.txt"
        output: "CAPITALISE.txt"
        shell:
                """
                tr '[:lower:]' '[:upper:]' < {input} > {output}
                """
```
