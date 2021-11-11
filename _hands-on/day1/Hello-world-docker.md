---
topic: containers
title: Tutorial1 - Hello-world Docker example
---

This short tutorial gives a very brief idea on how docker containers are launched. Although we don't work with docker containers in this course, we will be referring to docker images occasionally as we proceed. Docker is one of the most popular container platforms and it is worth having a basic idea.

## Learning Objectives
In this tutorial, you will learn to: 
- Launch an instance for Docker environment in [Play with Docker](https://labs.play-with-docker.com/)
- Spawn a hello-world docker container 
- Understand the mechanism behind running a docker container

## Launch an instance using play-with-docker
[![Try in PWD](https://cdn.rawgit.com/play-with-docker/stacks/cff22438/assets/images/button.png)](http://labs.play-with-docker.com/)

Within [play-with-docker](http://labs.play-with-docker.com/) webpage click on **create session**. Then, another page
will open with a big counter on the upper left corner. Click on **+ add new instance** and a terminal like instance should be generated on the right. 

## Spin up a hello-world Container 

Let’s start with a simple *hello-world* container from [Dockerhub](https://hub.docker.com) which is a docker registry for sharing images. Try running the following command on your linux terminal:

```bash
docker run hello-world
```
When a `docker run` command is issued *via* commandline interface (CLI), docker client contacts Docker daemon to check whether an image named, "hello-world" exists locally. If the image is not found locally, docker daemon pulls "hello-world" official image from DockerHub (*Note*: default registry for docker is DockerHub and for other docker registries one has to explicitly mention registry name as well). Once docker image is available locally, `docker run` command creates a new container from that image. Docker daemon then streams the default output of container to Docker client (= something we see as an output on terminal). Yes...lot of stuff has happened behind the scenes !!!

#### What is the terminal output from the above `docker run` command? #####

  ```bash
  Unable to find image 'hello-world:latest' locally
  latest: Pulling from library/hello-world
  78445dd45222: Pull complete
  Digest: sha256:c5515758d4c5e1e838e9cd307f6c6a0d620b5e07e6f927b07d05f6d12a1ac8d7
  Status: Downloaded newer image for hello-world:latest

 Hello from Docker!
  This message shows that your installation appears to be working correctly.
 ```
 
Note that `docker run` creates a container, executes the command in it and stops the container when it is done.

#### What will happen if you run the above command again? Did you observe any change in terminal output now?

Docker image has already been downloaded locally and therefore docker can execute the container straight away.

#### What is the default image tag used in `hello-world` example?

By default, an image is pulled with `latest` tag. It is possible to pull an image with a specified tag and is actually a good practice to use specific tag name for reproducible research. Tag provides a version control-like mechanism for docker images.

Congratulations, you have run a “Hello-World” Docker successfully !!!

## Exercises

1. Run a container from an image named "alpine" from DockerHub and execute a command inside that container so that output from the container is "Welcome to CSC !"

2. How would you run the same *hello-world example* from docker registry with fully qualified reference path for an image? Name few third-party docker registries besides DockerHub.

