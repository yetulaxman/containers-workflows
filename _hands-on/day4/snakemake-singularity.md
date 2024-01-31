---
topic: snakemake
title: Tutorial3 - Sankemake with singularity containers 
---

Snakemake workflow, which is described in terms of rules that define how to create output files from input files, is one of the popular workflows in the bioinformatics community. Snakemake is available as a module in Puhti environment. And also, Snakemake can be easily installed in the user's own disk space (e.g., Projappl directory) if you need to have a specific version for your scientific workflows.

## A singularity refresher at CSC
Please check our CSC documentation on how to use [singularity containers at CSC](https://docs.csc.fi/computing/containers/run-existing/). You can consult official [Singularity documentation](https://docs.sylabs.io/guides/latest/user-guide/) for more details.

### How to get containers

#### 1. Run or pull an existing Singularity/apptainer image from a repository
   
   It is possible to run containers directly from a repository:

     ```bash
     apptainer run shub://vsoch/hello-world:latest
     ```
     Usually it is better to pull the container first and then use the image file:

     ```bash
     apptainer pull shub://vsoch/hello-world:latest
     apptainer run hello-world_latest.sif
     ```

#### 2. Convert an existing Docker container to an Apptainer container

 Docker images are downloaded as layers. These layers are stored in a cache directory. Default location of the cache   nis `$HOME/.singularity/cache`. Since the home directory has limited capacity and some images can be large, it's 
 best to set `$SINGULARITY_CACHE` to point to some other location with more space.
 
 You can now run `singularity build`:
 ```bash
  apptainer build alpine.sif docker://library/alpine:latest
  ```
💡 You can find more detailed instructions on converting Docker containers in [Docs CSC](https://docs.csc.fi/computing/containers/creating/#converting-a-docker-container).

### Minimial script for building singularity image on Puhti

One can build singualrity image on Puhti and use it to run the snakemake workflow instead of installing the required tools  using tykky wrapper tool. One can build singularity image on Puhti using *--fakeroot* option. An example singularity definition file, with conda environment defined in a file(e.g, tutorial.yaml), is shown below:

```
Bootstrap : docker
From :  continuumio/miniconda3
IncludeCmd : yes

%labels
AUTHOR email@email.com

%files
tutorial.yaml

%post
apt-get update && apt-get install -y procps && apt-get clean -y
/opt/conda/bin/conda env create -n snakemake_env -f /tutorial.yaml
/opt/conda/bin/conda clean -a

%environment
export PATH=/opt/conda/bin:$PATH
. /opt/conda/etc/profile.d/conda.sh
conda activate snakemake_env

%runscript
echo "This is a tutorial for building singularity/appatainer image"
```

Finally, build the container image as below:

```bash
singularity build --fakeroot tutorial.sif tutorial.def 
```
## Run snakemake rule inside of a singularity container

Singularity/Apptainer is istalled on login and compute nodes on CSC computers. so no need to install it separately. Use * --use-singularity* flag to activate singularity environement and bind mount necessary disk space when usig snakemake workflows. This example relies on the user having some method for obtaining docker images that can be converted to singularity images. This example uses a publicly available dockerhub Fastqc image. A major caveat of using dockerhub is that if you run this example on a widely use cluster it is possible you will get an error.

```bash
module load snakemake/7.17.1
snakemake -s snakemake-singularity.smk      -j 1     --latency-wait 60     --use-singularity --singularity-args "-B /scratch/project_2001659/yetukuri/snakemake_workflow:/scratch/project_2001659/yetukuri/snakemake_workflow"   \
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

‼️  If you want to install a specific version of Snakemake along with other python packages, we recommend installing  using pip. This way, one can use
  singularity containers smoothly in Snakemake workflow.

‼️ Please note that scaling up of jobs with slurm executor should be done carefully. In case you need to submit lot of small jobs to cluster, please use hyper executor which can allocate a large resource block and then submit your jobs to the same resource allocation.  Slurm executor submits each of your computational tasks as separate Slurm jobs or job step.

##  Run snakemake workflow with singularity container

One can run build a singularity container (--with fakeroot) option CSC supercomputer and define the image on the top level of Snakefile as shown below:

```
##### setup singularity #####
# this container defines the underlying OS for each job when using the workflow
# e.g., singularity: "docker://ginolhac/snake-rna-seq:0.2"
singularity: "image/turotial.sif"

```

Finally, one can submit the snakemake workflow as slurm job as shown below:
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
For the completion of this tutorial, you can download the needed scripts and data from Allas object storage as below:

```
wget https://a3s.fi/snakemake/snakemake_tutorial.tar.gz
tar -xavf snakemake_tutorial.tar.gz
```
You can finally submit snakemake workflow as below:

```
bash sbatch-sing.sh
```


Tips:
- snakemake --containerize > Dockerfile  (to create docker file from the snakemake)
