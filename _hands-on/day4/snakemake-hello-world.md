---
topic: snakemake
title: Tutorial1 - Sankemake toy example
---
Snakemake workflow is one of the popular scientific workflows in the bioinformatics community. The workflow manager enables scalable and reproducible scientific pipelines as a series of chained rules. These reproducible workflows come with a fully-specified software environment. 

Snakemake is available as a module in Puhti supercomputing environment. And also, you can easily install it in the your own disk space (e.g., Projappl directory) if a specific version of snakemake is desired. The following session introduces a toy example using a pre-installed *snakemake* module on Puhti.

## Running Snakemake workflow with a pre-installed snakemake module on Puhti

Please make sure that you have a [user account at CSC](https://docs.csc.fi/accounts/how-to-create-new-user-account/) and are a member of a project which [has access to the Puhti service](https://docs.csc.fi/accounts/how-to-add-service-access-for-project/) before start running workflows on Puhti.  More instructions on logging into Puhti supercomputer can be found [here](https://csc-training.github.io/csc-env-eff/hands-on/connecting/ssh-puhti.html). Please note that one should avoid launching snakemake workflows on login nodes and can use interactive or batch jobs. More information on using interactive jobs can be found [here](https://docs.csc.fi/computing/running/interactive-usage/).

The toy snakemake example file, Snakefile (with a capital S and no file extension), has the following content:

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
Snakemake is installed as a module on Puhti. So one can load it withouting needing to install it by yourself. One can write below bash script to execute the above toy example. 

```bash
module load snakemake/7.17.1
snakemake --help   #  to get information on more options.

# The following command shows how to run a snakemake workflow on a cluster using slurm executor
snakemake -s Snakefile \ # the Snakefile is the default file name; no need specify with -s flag
 -j 1  \     # this will execute up to 3 tasks in parallel)       
--latency-wait 60 \  # snakemake to wait up to 60 seconds after a job completes for the output files to become available.
--cluster "sbatch -t 10  --account=project_xxx --job-name=hello-world --tasks-per-node=1 --cpus-per-task=1 
--mem-per-cpu=4000 -p test"
# cluster option to execute snakemake workflow on cluster given other options for slurm
```

Finally, run the workflow. The above script can be put in a file (run_snakemak.sh) can be submitted in the interactive node as below:

```bash
sinteractive -c 2 -m 10000 # type this command on login node
bash run_snakemake.sh   # run the workflow
```

### Using cluster configuration for snakemake workflow

Please pay attention to --cluster-config  and --cluster options of snakemake workflow manager. One can define the values of cluster in cluster.yaml (--cluster-config) file and can be retrieved in the command line options of the flag --cluster e.g., {cluster.partition}. 

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
 snakemake --rulegraph | dot -Tpng > rulegraph.png # Generate the rule based graph 
 snakemake --dag | dot -Tpng > dag.png # Generate directed acyclic graph
```

   
