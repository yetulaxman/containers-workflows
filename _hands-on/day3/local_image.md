---
topic: singularity
title: Tutorial2 -  Converting of local Docker images to singularity 
---

We have earlier converted Docker image from registries to singularity in the previous tutorials. Sometimes, the images may not be readily available in image registries for our purpose. In that case, we have to either modify some existing docker image or build a new one. This can only be done on our local machines or any host machine where we have privileged root access. This tutorial explains how to build a singularity image from a local docker image. 

###  Expected outcome of this tutorial:
After this tutorial, you will learn to:
- Save a docker image locally 
- Launch a singularity container from a local docker image 

### Converting a local docker image to singularity 

1. Let's use the same trimmomatic software example we have used in previous tutorial. On PWD terminal [![PWD terminal](https://cdn.rawgit.com/play-with-docker
   /stacks/cff22438/assets/images/button.png)](http://labs.play-with-docker.com/), run the following command to pull an image:

   ```bash
    docker pull quay.io/biocontainers/trimmomatic:0.32--hdfd78af_4
   ```
   In a real-world use case, this image can be a modified image pulled from registries or completely a new image built on your local machine to meet your needs. 
  
2. Once an image is pulled or built on your local machine, the image is stored in local registry (usually at /var/lib/docker) on host machines. In order to find
   the stored docker images, use **docker images** command. 
  
   ```bash  
   docker images
   ```
   From the above command, you need to find an image ID of trimmomatic image to save it locally. 
  
3. Create a tarball of the Docker image (with image id as cc8b303fee58)  using the **docker save** command as below:
  
   ```bash
   docker save cc8b303fee58 -o trimmomatic_image.tar  # in this case image_id is : cc8b303fee58
   ls -l  trimmomatic_image.tar  # to see the local copy of image tarball
   ```

4. Copy the image tarball from PWD to scratch folder on Puhti 

   ```  
   scp trimmomatic_image.tar YOURCSCUSERNAME@puhti.csc.fi:/scratch/project_xxx/YOURCSCUSERNAME
   ```

5. Build Singularity image from the tarball. 
 
    ```bash
    cd /scratch/project_xxxx/YOURCSCUSERNAME  # replace `project_xxxx` with a valid project number 
    singularity build local_trimmomatic_image.sif docker-archive://trimmomatic_tar
    ```
  
6. Launch singularity container and check if you can get a commandline help for trimmomatic software

    ```bash
   singularity exec local_trimmomatic_image.sif trimmomatic --help
   ```
