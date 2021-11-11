---
topic: singularity
title: Tutorial1 -  Conversion of local Docker images to singularity image
---

We have seen in previous episodes where we have converted Docker images from registries. Sometimes, you may want to convert local docker images to singularity images

###  Expected outcome of this tutorial:
we will learn:
- how to save a docker image locally (on PWD)
- how to start a singularity container from local archive

### how can you convert a local docker image to singularity image

Start interactive node on Puhti:

On PWD terminal, run the following command:

```bash
docker pull quay.io/biocontainers/trimmomatic:0.32--hdfd78af_4
```
Check if the image is saved using the following command:
  
```bash  
 docker images
```
once you identified the image on PWD, copy the image_id corresponding to that image and save the image as below:
```bash
docker save cc8b303fee58 -o trimmomatic_image.tar  # in my case image_id is : cc8b303fee58
```

copy the image on PWD to Puhti :
```  
scp trimmomatic_image.tar CSCUSERNAME@puhti.csc.fi:/users/project_xxx/CSCUSERNAME
```
Load local archive onn Puhti terminal:
```bash
cd /users/project_xxx/CSCUSERNAME
singularity build local_trimmomatic_image.sif docker-archive://trimmomatic_tar
```
Check if the software is working properly:

```bash
singularity exec local_trimmomatic_image.sif trimmomatic --help
```
