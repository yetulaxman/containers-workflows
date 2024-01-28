---
topic: snakemake
title: Tutorial8 - Sankemake tykky example
---

Snakemake workflow, which is described in terms of rules that define how to create output files from input files, is one of the popular workflow managers in the bioinformatics community. Snakemake is available as a module in Puhti environment. And also, Snakemake can be easily installed in the user's own disk space (e.g., Projappl directory) if you need to have a specific version for your scientific workflows.

### Installing other python packages needed for Snakemake workflow

The current Snakemake module on Puhti is based on pip-installation and has a limited set of other python packages. For any real-world examples, one needs to have other python packages which can be installed using *pip* in a user defined folder (e.g., a directory in scratch or Projappl)

Change to working directory to e.g., scratch area  and create a folder for installing python packages (Here, let's install *matplotlib* package that is not available in Snakemake module) 

```bash
cd /scratch/project_xxx/$USER/snakemake_workflow/ && mkdir venv
```

Install the needed packages:

```bash
export PYTHONUSERBASE="/scratch/project_xxx/$USER/snakemake_workflow/venv"
pip3.9 install --user matplotlib 
export PATH="/scratch/project_xxx/yetukuri/snakemake_workflow/venv/bin:$PATH"
export PYTHONPATH="/scratch/project_xxx/yetukuri/snakemake_workflow/venv/lib/python3.9/site-packages/"  
#(this needs to be the same version of python package)
```

Content of test2.smk file is as shown below:

```bash
import matplotlib   # You can use it if needed as part of preporcessing of data.

rule all:
        input: "CAPITALISE.txt"

rule say_hello:
        output: "smaller_case.txt"
        python -c "import matplotlib"
        shell:
                """
                echo "testing toy example for snakemake workflow where matplotlib package is installed" > smaller_case.txt
                """
rule capitalise:
        input: "smaller_case.txt"
        output: "CAPITALISE.txt"
        shell:
                """
                tr '[:lower:]' '[:upper:]' < {input} > {output}
                """
```

Run Snakemake workflow:

```bash
module load snakemake/7.17.1
snakemake -s test2.smk      -j 1     --latency-wait 60   --cluster "sbatch -t 10 \
--account=project_2001659 --job-name=fastqc-help  --tasks-per-node=1 --cpus-per-task=1 \
--mem-per-cpu=4000 -p test"
```


### Installing python packages with tykky needed for Snakemake workflow

Conda installations should not be done directly on Puhti. [Tykky wrapper tool](https://docs.csc.fi/computing/containers/tykky/) instead be used to install python packages in setting up your compute environment. The wrapper tool installs applications inside of a singularity container and thus  facilitates better performance in terms of faster startup times, reduced IO load, and reduced number of files on parallel filesystems. 

Here is an example of tykky-based custom installation for cytodata 2023 hackathon (make sure to edit with correct CSC project name and user name as needed):

```bash
# start an interactive session once you are in login node
sinteractive -c 8 -m 30000 -d 100  # this command requests a compute node with 8 cores, 30 GB memory and 100 GB local disk space; change settings as needed
# load needed packages
module load git   # git command is not available by default on interactive nodes
module load purge  # clean environment 
module load tykky # load tykky wrapper 
# install python libraries using tykky wrapper tool; make sure to use proper project/username
mkdir -p /projappl/<project>/$USER && mkdir -p /projappl/<project>/$USER/snakemake_tykky
conda-containerize new --prefix  /projappl/<project>/$USER/snakemake_tykky env.yaml    
```

In the above example, Tykky installs a basic setup (as listed in the file, env.yml) to the directory '/projappl/project_xxxx/snakemake_tykky'. Please note that you have to add the bin directory of installation to the $PATH variable before start using the installed environment.

Once the environment is successfully installed, one has to use add the installed path as below:
```
export PATH="/projappl/project_2001659/$USER/snakemake_tykky/bin:$PATH"
```

