---
topic: singularity
title: Tutorial2 -  Converting of local Docker images to singularity 
---

We have converted Docker images from registries to singularity in previous tutorial. Sometimes, the images may not be readily available in image registeries for our purpose. In that case either we have to modify some existing docker image or build a new one. This can be done on your local machines or any host machine where you have prelilized root access. This tutorial explains how to build a singularity image from local docker image. 

###  Expected outcome of this tutorial:
After this tutorial, you will be able to:
- Save a docker image locally 
- Launch a singularity container from local docker image archive

### Converting a local docker image to singularity 

1. Let's use the same trimmomatic software example we have used in previous tutorial. On [PWD terminal](https://cdn.rawgit.com/play-with-docker/stacks/cff22438
   /assets/images/button.png)](http://labs.play-with-docker.com/),run the following command to pull an image:

   ```bash
    docker pull quay.io/biocontainers/trimmomatic:0.32--hdfd78af_4
   ```
  In a real world use case, this image can be a modified image registries or a new image built on this local machines.
  
2. Once image is pulled or built on you local machine, they are in  local registry (usually at /var/lib/docker) on host machines. In order to find those stored
   docker images, use **docker images** commannd. 
  
   ```bash  
   docker images
   ```
  Find the Docker image ID corresponding to trimmomatic image.
  
3. Create a tarball of the Docker image (with image id as cc8b303fee58)  using the **docker save** command:
  
   ```bash
   docker save cc8b303fee58 -o trimmomatic_image.tar  # in my case image_id is : cc8b303fee58
   ```

4. Copy the image tarball on PWD to scratch folder on Puhti 

   ```  
   scp trimmomatic_image.tar CSCUSERNAME@puhti.csc.fi:/users/project_xxx/CSCUSERNAME
   ```

5. Convert the tarball to a Singularity image. 
 
  ```bash
  cd /users/project_xxx/CSCUSERNAME
  singularity build local_trimmomatic_image.sif docker-archive://trimmomatic_tar
  ```
  
6. Check if the trimmomatic software is working properly

  ```bash
   singularity exec local_trimmomatic_image.sif trimmomatic --help
  ```
