---
topic: snakemake
title: Tutorial7 - Sankemake toy example
---

Snakemake is one of the popular workflow managers in bioinformatics community and is available as a module in Puhti environment. And also, Snakemake can be easily
installed in user's Projappl directory.


## Installing snakemake locall using Tykky container wrapper:

```
module load tykky
mkdir /projappl/project_xxx/snakemake
conda-containerize new --mamba --prefix /projappl/project_xxx/snakemake env.yaml

```
where env.yaml file :

```
channels:
  - conda-forge
  - bioconda
  - defaults

dependencies:
  - python
  - mamba
  - igvtools
  - samtools
#  - snakemake>=5.4,!=6.4
  - snakemake=7.15.2-0

```

Running Snakemake pipeline:


```
export PATH="/projappl/project_xxx/snakemake/bin:$PATH"

/projappl/project_2001659/snakemake/bin/python3.10 /projappl/project_2001659/snakemake/bin/snakemake -s hello-world.smk -j 1 \ 
--latency-wait 60 --cluster "sbatch -t 300 --account=project_2001659 --job-name=hello_world --tasks-per-node=1 --cpus-per-task=1 \
-p medium " --scheduler greedy

```

## Running snakemake with module installed on Puhti

```
ml snakemake/7.17.1
snakemake -s test.smk      -j 1     --latency-wait 60     --use-singularity     --singularity-args "-B /scratch/project_2001659/yetukuri/snakemake_daniel3:/scratch/project_2001659/yetukuri/snakemake_daniel3"   \
--cluster "sbatch -t 10 --account=project_2001659 --job-name=fastqc-help  --tasks-per-node=1 --cpus-per-task=1 --mem-per-cpu=4000 -p test"
```


## Installing other python packages needed for snakemake workflow

Change workiing directory to /scratch area  and create a folder for installing python packages

```
cd /scratch/project_2003682/$USER/snakemake_workflow/ and mkdir venv
```

Install needed packages:

```
export PYTHONUSERBASE="/scratch/project_2003682/$USER/snakemake_workflow/venv"
pip3.9 install --user matplotlib 
export PATH="/scratch/project_2003682/yetukuri/snakemake_workflow/venv/bin:$PATH"
export PYTHONPATH="/scratch/project_2003682/yetukuri/snakemake_workflow/venv/lib/python3.9/site-packages/"  
#(this needs to be the same version of python package)
```
test.smk


import matplotlib
import pandas as pd
import numpy
from snakemake.utils import validate, min_version
from multiprocessing import cpu_count
import glob
import re
import os
import sys
print(sys.version)
print(sys.executable)
import pkg_resources;installed_packages = pkg_resources.working_set;installed_packages_list = sorted(["%s==%s" % (i.key, i.version) for i in installed_packages]);print(installed_packages_list)

rule all:
        input: "HELP-CAPITALISE.txt"

rule say_hello:
        output: "fastqc-help.txt"
        singularity: "docker://amancevice/pandas:latest"
        shell:
                """
                /usr/bin/which python
                python -c "import pandas"
                echo "test" > fastqc-help.txt
                """

rule capitalise:
        input: "fastqc-help.txt"
        output: "HELP-CAPITALISE.txt"
        shell:
                """
                tr '[:lower:]' '[:upper:]' < {input} > {output}
                """
```
