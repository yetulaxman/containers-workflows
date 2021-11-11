
## (WIP) This section is in progress


## how can you convert a docker image (e.g.,fastx software) on bioconda environment to singularity image?

Expected outcome of this tutorial:
- Searching a biocontainer from container registry
- Able to convert a docker image to container image
- Able to run a container

Visit the fastqc software tool available on the bioconda page (https://bioconda.github.io/recipes/fastx_toolkit/README.html) and take path of the image from  docker pull command from above page as below:

```
docker pull quay.io/biocontainers/fastx_toolkit:<tag>
```

Path for above image is here: docker://quay.io/biocontainers/fastx_toolkit:<tag>

And pick your tag from here: (https://quay.io/repository/biocontainers/fastx_toolkit?tab=tags)

And now, you can use any linux machine that has singularity installed 

Use the following command to convert a docker image to singularity using tag (here i picked 0.0.14--he1b5a44_8) :

```

singularity build fastx_toolkit.sif docker://quay.io/biocontainers/fastx_toolkit:0.0.14--he1b5a44_8

```

Alternatively,  on Puhti interactive terminal, you can use the the following commands:

```

export SINGULARITY_TMPDIR=$LOCAL_SCRATCH
export SINGULARITY_CACHEDIR=$LOCAL_SCRATCH
unset XDG_RUNTIME_DIR
singularity build fastx_toolkit.sif docker://quay.io/biocontainers/fastx_toolkit:0.0.14--he1b5a44_8

```
