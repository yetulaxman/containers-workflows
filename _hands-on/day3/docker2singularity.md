---
topic: singularity
title: Tutorial1 -  Conversion of Docker images to singularity
---

# Conversion of Docker image to Singularity image 

We converted a Docker image to singularity one using *singularity build* subcommand in previous episodes. And also, we can use *singularity pull* subcommand to achieve the same outcome. As an example for the image conversion, we will use a biconda package named, trimmomatic software which is available as docker image in quay.io registry. Trimmomatic is a flexible read trimming tool for Illumina NGS data. 

###  Expected outcome of this tutorial:
Upon completion of this tutorial, you will be able to:
- Launch a singularity container from a bioconda package (e.g., trimmomatic)
- Manage Singularity TMPDIR and CACHEDIR directories in HPC environment


### Launching trimmomatic software as a singularity container

1. Launch interactive session on Puhti as below:

   ```bash
   # start interactive node as below and choose your project name on prompt
   sinteractive -c 2 -m 4G -d 100
   ```
2. Visit trimmomatic software webpage as available on [bioconda](https://bioconda.github.io/recipes/trimmomatic/README.html) channel. Look for URI of trimmomatic
   image (Looks something like quay.io/biocontainers/trimmomatic:<tag> ) and incorporate the [latest tag](https://quay.io/repository/biocontainers
   /trimmomatic?tab=tags) information for trimmomatic software as availabe in *quay.io* registry. Once you have information of URI and tag information, you can 
   build a singularity image as below:
  
   ```bash
    singularity pull docker://quay.io/biocontainers/trimmomatic:0.32--hdfd78af_4
   ```
   Upon successful completion of above command, you will be able to see trimmomatic singulairty image (name: trimmomatic_0.32--hdfd78af_4.sif) in the current
   directory.

   You can check the disk space taken up by the image in cache folder as below:
  
   ```bash  
   singularity cache list
   ```
   when you convert multiple smaller images or a bigger image, the singularity cache can take up lot of space. Please note that the singularity cache directory is 
   by default your HOME directory whcih is limted by disk space quota in CSC HPC environment. It is therefore good idea to clean singularity cache every now and
  then as below:
  
   ```bash
    singularity cache clean
   ```
  
3. Launch trimmomatic container and check the comamndline help 
    ```bash
    singularity exec trimmomatic_0.32--hdfd78af_4.sif trimmomatic --help
   ```
  
4. As a best practice tip, do not pull a Singularity image to your Puhti Home Directory. In order to avoid such cache issues which would result in disk space
   errors, we recommend resetting Singularity TMPDIR and CACHEDIR directories either to Lustre scratch or  LOCAL_SCRATCH  before performing image conversion as 
   below:
  
   ```bash  
    export SINGULARITY_TMPDIR=/scratch/project_xxxx/$USER
    export SINGULARITY_CACHEDIR=/scratch/project_xxxx/$USER
    unset XDG_RUNTIME_DIR
    singularity pull docker://quay.io/biocontainers/trimmomatic:0.32--hdfd78af_4
   ```
  
   or better yet would be is to use LOCAL_SCRATCH directories on Puhti especially if the image sizes are quite big (in the order of GBs). Usually larger docker 
  images are composed of several layers and importing and unpacking the layers into ne single image file for singularity can be I/O intensive. For the the sake of
  simple example we can use the same trimmomatic example here. 
  
   ```bash  
   export SINGULARITY_TMPDIR=$LOCAL_SCRATCH
   export SINGULARITY_CACHEDIR=$LOCAL_SCRATCH
   unset XDG_RUNTIME_DIR
   singularity pull docker://quay.io/biocontainers/trimmomatic:0.32--hdfd78af_4
   ```
