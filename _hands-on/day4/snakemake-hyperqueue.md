---
topic: snakemake
title: Tutorial10 - Sankemake with hyperqueue executor
---

Snakemake workflow, which is described in terms of rules that define how to create output files from input files, is one of the popular workflow managers in the bioinformatics community. Snakemake is available as a module in Puhti environment. And also, Snakemake can be easily installed in the user's own disk space (e.g., Projappl directory) if you need to have a specific version for your scientific workflows.


## Running Snakemake with HyperQueue
If your workflow manager is using sbatch for each process execution and you have many short processes it's advisable to switch to HyperQueue to improve throughput and decrease load on the system batch scheduler.

Using Snakemake's --cluster flag we can use hq submit instead of sbatch:

```
snakemake --cluster "hq submit --cpus <threads> ..."

```

> Note: If you want to install a specific version of Snakemake along with other python packages, we recommend installing  using pip. This way, one can use
  singularity containers smoothly in Snakemake workflow.
