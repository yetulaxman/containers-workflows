---
topic: singularity
title: Tutorial1 -  Conversion of Docker images to singularity
---

We have seen in previous episodes where we have converted Docker images to singularity images using *singularity build* subcommand. In most cases that conversion would work just fine. Please also note that we can use *singularity pull* subcommand to convert docker images to singularity images.  We will use a bionda package called, [trimmomatic](https://bioconda.github.io/recipes/trimmomatic/README.html) which is a flexible read trimming tool for Illumina NGS data as an example here. 

###  Expected outcome of this tutorial:
- Build a singularity container from the a bioconda package (trimmomatic)
- Understand singularity tmp and cache directories
- Efficient ways of image converion in HPC environment

### how can you convert a docker image (e.g.,trimmomatic software) on bioconda environment to singularity image?

Start interactive node on Puhti:

Visit the trimmomatic software tool available on the [bioconda](https://bioconda.github.io/recipes/trimmomatic/README.html) page and copy the path of the trimmomatic image (something docker:quay.io/biocontainers/trimmomatic:<tag>). One can use the singularity pull subcommnad as below:

however, we are still missing a valid tag for the image. Look for the [latest tag](https://quay.io/repository/biocontainers/trimmomatic?tab=tags) available for trimmomatic software in docker registry. You can now use the following command to convert a docker image to singularity one.

```bash
singularity pull docker://quay.io/biocontainers/trimmomatic:0.32--hdfd78af_4
```
Upon successful completion of above command, you will be able to see trimmomatic singulairty image (trimmomatic_0.32--hdfd78af_4.sif) in the current directory.

You can check the disk space taken up by the image in cache folder as below:
  
```bash  
  singularity cache list
```
when you convert multiple images or bigger image, the cache can take up lot of space. In HPC environment, cache directory is located in HOME directory whose quota is limited by default. You can clean the cache as below:
 ```bash
  singularity cache clean
 ```

To avoid such cache issues which will result in disk quota errors, we can reset singularity tmp and cache directories to scratch as below:
```  
export SINGULARITY_TMPDIR=/scratch/project_xxxx/$USER
export SINGULARITY_CACHEDIR=/scratch/project_xxxx/$USER
unset XDG_RUNTIME_DIR
singularity pull docker://quay.io/biocontainers/trimmomatic:0.32--hdfd78af_4
```
  
or better yet would be is to use LOCAL_SCRATCH directories on Puhti. This is quite useful especially when the image sizes are quite big (in the order of GBs). Docker images are composed of layers and unpacking the layers and flaten into one single image file can I/O intensive. For the the sake simple example we can use same trimmomatic software. 
```  
export SINGULARITY_TMPDIR=$LOCAL_SCRATCH
export SINGULARITY_CACHEDIR=$LOCAL_SCRATCH
unset XDG_RUNTIME_DIR
singularity pull docker://quay.io/biocontainers/trimmomatic:0.32--hdfd78af_4
```
