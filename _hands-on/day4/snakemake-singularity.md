---
topic: snakemake
title: Tutorial9 - Sankemake with singularity containers
---

Snakemake workflow, which is described in terms of rules that define how to create output files from input files, is one of the popular workflow managers in the bioinformatics community. Snakemake is available as a module in Puhti environment. And also, Snakemake can be easily installed in the user's own disk space (e.g., Projappl directory) if you need to have a specific version for your scientific workflows.


## Snakemake workflow with singularity containers

Use * --use-singularity* flag to activate singularity environement and bind mount necessary disk space 

```bash
module load snakemake/7.17.1
snakemake -s test3.smk      -j 1     --latency-wait 60     --use-singularity --singularity-args "-B /scratch/project_2001659/yetukuri/snakemake_workflow:/scratch/project_2001659/yetukuri/snakemake_workflow"   \
--cluster "sbatch -t 10 --account=project_xxx --job-name=fastqc-help  --tasks-per-node=1 --cpu


Contents of test3.smk are as below:

```bash
# test availability of different packages 
import matplotlib

import pkg_resources;installed_packages = pkg_resources.working_set;installed_packages_list = sorted(["%s==%s" % (i.key, i.version) for i in installed_packages]);print(installed_packages_list)

rule all:
        input: "HELP-CAPITALISE.txt"

rule say_hello:
        output: "fastqc-help.txt"
        singularity: "docker://biocontainers/fastqc:v0.11.9_cv8"
        shell:
                """
                fastqc --help > "fastqc-help.txt"
                """

rule capitalise:
        input: "fastqc-help.txt"
        output: "HELP-CAPITALISE.txt"
        shell:
                """
                tr '[:lower:]' '[:upper:]' < {input} > {output}
                """
```

```

> Note: If you want to install a specific version of Snakemake along with other python packages, we recommend installing  using pip. This way, one can use
  singularity containers smoothly in Snakemake workflow.
