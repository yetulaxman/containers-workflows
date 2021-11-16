---
topic: singularity1
title: Tutorial1 -  sharing images using registries
---

Sometimes, we have to build custom images to meet our needs either by building a new image from Dockerfile or adding missing software tools on pre-built images. And we usually want to re-use the customised image later or share it with other collaborators. Image registries are better choice for sharing our image with others! In this session, you will learn to modify existing Docker image and share it with others via registries

## Modifying content inside the container.

1. Deploy FastQC Container

Let's launch fastqc container interactively as it is useful for testing or development. Here you ran the docker run ... command with special flags -it so that it attaches you to an interactive tty in the container. You can run any command inside a container if it is provisioned by author of image! Take some time to run a useful command (e.g., get some documentation help from a software). 


```bash
docker container run -it biocontainers/fastqc:v0.11.9_cv7 /bin/sh
```

Note: the flags -it are short for -i -t which are the short forms of --interactive (Keep STDIN open) and --tty (allocate a terminal).

2. Modify the Container

Now that you are in the container, you can modify the image. Just for illustartion, as fastQC lacks vim editor inside, we can try to install vim inaside the container.

```
apt-get install vim
```

3. Commit Changes to Image

Sharing a docker image involves making your local image available for other people to use it. Let's look at our *fastqc* container into which you have installed e.g., vim editor and you wish to share the new image now with your friend.

Find container id corresponding to *fastqc* container (tip: use `docker ps -a`command) and use it with the following *docker commit* command:

```bash
docker commit <container id> fastqc-vim:test   
```
> note: It is a good idea to stop the container if running before performing *docker commit*  

and then save the resulting image using `docker save` command as below:

```
docker save <image id> -o  fastqc-vim.test.tar   # better yet would be to use `reponame:tag` instead of <image id> (e.g., docker save fastqc-vim:test -o  fastqc-vim-name.test.tar)
```
You can check the existance of tar file in your directory after *docker save* command has been executed successfully. You can send this tar file to your colllaborator/friend.

Finally, your friend can load the image from tar file uisng *docker load * command. 

```bash
docker load < fastqc-vim.test.tar
```
Once image is loaded, you can now use it as if it is downloaded from DockerHub. The newly loaded image can be viewed using `docker images` command on host machine

### Push your image to your DockerHub repository

Sharing an image *via* docker registry such as  DockerHub (the most popular image registry, hosting hundreds of thousands of images) is an efficient way of sharing and managing your images. Once image is in a docker (public) registry, anyone can pull it from there. 

However, this involves setting up an account in Docker registry. Here are few steps you can do to set-up your account:

- One can create an account on the DockerHub using instructions [here](https://hub.docker.com/account/signup/). After verifying your email you are ready to go and upload your first docker image.
- Click on Create Repository.
- Choose a name  and a description for your repository and click Create.

You can now come back to your virtual machine and login to dockerhub by running `docker login` command.

```
docker login --username=your-dockerhub-user-name # this will prompt for your dockerhub password
```
Before pushing your docker image to DockerHub,  you just need to rename docker image to your namespace/account first using `docker tag` command as below:

```bash
docker tag <image id> your-dockerhub-user-name/repo-name[:tag]   # find <image id> corresponding to repository, fastqc-vim  by typing `docker images` command on host machine
```

All images should be tagged with an appropriate prefix to repository name before pushing an image.

 Let's push your image finally as below:

```bash
 docker push your-dockerhub-user-name/repo-name[:tag]
```
Once the push  to repository is successful, your image is now available for everyone to use. Go to your profile page on the DockerHub  to view  your new docker image on DockerHub. Anybody can pull your image from Dockerhub now.

**Summary**: In this section, we have explored two ways of sharing a docker image with other in the following two ways: 1) tarball approach and 2)dockerhub approach. Dockerhub approach is the preferred approach and usually involves providing good documentation including dockerfile. 
