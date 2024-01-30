---
topic: snakemake
title: Tutorial7 - Sankemake hello-world example
---
Snakemake workflow, which is described in terms of rules that define how to create output files from input files, is one of the popular workflow managers in the bioinformatics community. The workflow manager enables scalable and reproducible scientific pipelines as a series of chained rules. One can share these reproducible workflows with a fully-specified software environment. 

Snakemake is available as a module in Puhti supercomputing environment. And also, it can be easily installed in the user's own disk space (e.g., Projappl directory) if you need to have a specific version for your scientific needs.This hands-on exercise introduces hello-world example using a pre-installed module on Puhti

## Running Snakemake workflow with a pre-installed module on Puhti

Please sure that you have a [user account at CSC](https://docs.csc.fi/accounts/how-to-create-new-user-account/) that is a member of a project which [has access to the Puhti service](https://docs.csc.fi/accounts/how-to-add-service-access-for-project/) before start running workflows on Puhti.  More instructions on logging into Puhti environment can be found [here](https://csc-training.github.io/csc-env-eff/hands-on/connecting/ssh-puhti.html)

Hello-world snakemake example file, Snakefile (with a capital S and no file extension), has the following content:

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

Snakemake is installed as a module on Puhti and can be loaded as below: 

```bash
module load snakemake/7.17.1
snakemake --help   #  to get information on more options.

# The following command shows how to run a snakemake workflow on a cluster using slurm executor
snakemake -s Snakemake \ # the Snakemake filename is the default name; no need specify with -s flag
 -j 1  \     # this will execute up to 3 tasks in parallel)       
--latency-wait 60 \  # snakemake to wait up to 60 seconds after a job completes for the output files to become available.
--cluster "sbatch -t 10  --account=project_xxx --job-name=hello-world --tasks-per-node=1 --cpus-per-task=1 
--mem-per-cpu=4000 -p test"
# cluster option to execute snakemake workflow on cluster given other options for slurm
```

One  should not launch snakemake workflows on login nodes and can use interactive or batch jobs. More information on using interactive jobs can be found [here](https://docs.csc.fi/computing/running/interactive-usage/).

Finally, run the workflow. The above script can be put in a file (run_snakemak.sh) can be submitted as below:

```bash
bash run_snakemake.sh
```

### Using cluster configuration for snakemake workflow

Please pay attention to --cluster-config and --cluster options. One can define (--cluster-config) the values of cluster in cluster.yaml file and can retrieved in the command line e.g., {cluster.partition}. 

```bash
# cluster.yaml 
__default__:
        partition: test
        account: project_xxxx
        nodes: 1
        time: 00:10:00
        job-name: hello-world
```

The snakemake command can be written as :
```
module load snakemake/7.17.1
snakemake --cluster-config cluster.yaml \
	--jobs 1 \
	--cluster "--cluster "sbatch -t {cluster.account}  --account={cluster.account} --job-name={cluster.job-name} --tasks-per-node=1 --cpus-per-task=1 --mem-per-cpu=4000 -p {cluster.partition}"
	--latency-wait 60
```

### Useful commandline options

```
 snakemake -n     # Dry-runs are a great way to check your commands before running them
 snakemake -p     #  Prints the shell commands that are being run to the terminal
```
