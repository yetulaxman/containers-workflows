---
topic: snakemake
title: Tutorial4 - Sankemake with hyperqueue executor
---

Snakemake workflow, which is described in terms of rules that define how to create output files from input files, is one of the popular workflows in the bioinformatics community. Snakemake is available as a module in Puhti environment. And also, Snakemake can be easily installed in the user's own disk space (e.g., Projappl directory) if you need to have a specific version for your scientific workflows.


## Running Snakemake with HyperQueue
If your workflow manager is using sbatch for each process execution and you have many short processes it's advisable to switch to HyperQueue to improve throughput and decrease load on the system batch scheduler.

Using Snakemake's --cluster flag we can use hq submit instead of sbatch:

```
snakemake --cluster "hq submit --cpus <threads> ..."

```

For the completion of this tutorial, you can download the needed scripts and data from Allas object storage as below:

```
wget https://a3s.fi/snakemake/snakemake_tutorial.tar.gz
tar -xavf snakemake_tutorial.tar.gz
```

### example 1: Hyperqueue executor for snakemake workflow where python packages are installed with tykky

Batch script as sbatch-hq-tykky.sh :

```
#!/bin/bash
#SBATCH --job-name=myTest
#SBATCH --account=project_xxxx
#SBATCH --time=00:10:00
#SBATCH --mem-per-cpu=2G
#SBATCH --nodes=1
#SBATCH --cpus-per-task=40
#SBATCH --ntasks-per-node=1
#SBATCH --partition=small

module load hyperqueue

export PATH="/projappl/project_xxxx/$USER/snakemake_tykky/bin:$PATH"

# Create a per job directory

export HQ_SERVER_DIR=$PWD/.hq-server-$SLURM_JOB_ID
mkdir -p $HQ_SERVER_DIR

hq server start &
srun --cpu-bind=none --hint=nomultithread --mpi=none -N $SLURM_NNODES -n $SLURM_NNODES -c 40 hq worker start --cpus=40 &

num_up=$(hq worker list | grep RUNNING | wc -l)
while true; do

    echo "Checking if workers have started"
    if [[ $num_up -eq $SLURM_NNODES ]];then
        echo "Workers started"
        break
    fi
    echo "$num_up/$SLURM_NNODES workers have started"
    sleep 1
    num_up=$(hq worker list | grep RUNNING | wc -l)

done
snakemake -s Snakefile -j 1  --cluster "hq submit --cpus 2 "
hq worker stop all
hq server stop
```

and run the script as below:

```bash
sbatch sbatch-hq-tykky.sh
```

### example 2: Hyperqueue executor for snakemake workflow where python packages are installed in singularity container

sbatch script (sbatch-hq-sing.sh):

```bash
#!/bin/bash
#SBATCH --job-name=myTest
#SBATCH --account=project_xxxx
#SBATCH --time=00:10:00
#SBATCH --mem-per-cpu=2G
#SBATCH --nodes=1
#SBATCH --cpus-per-task=40
#SBATCH --ntasks-per-node=1
#SBATCH --partition=small

module load hyperqueue
module load snakemake/7.17.1

# Create a per job directory

export HQ_SERVER_DIR=$PWD/.hq-server-$SLURM_JOB_ID
mkdir -p $HQ_SERVER_DIR

hq server start &
srun --cpu-bind=none --hint=nomultithread --mpi=none -N $SLURM_NNODES -n $SLURM_NNODES -c 40 hq worker start --cpus=40 &

num_up=$(hq worker list | grep RUNNING | wc -l)
while true; do

    echo "Checking if workers have started"
    if [[ $num_up -eq $SLURM_NNODES ]];then
        echo "Workers started"
        break
    fi
    echo "$num_up/$SLURM_NNODES workers have started"
    sleep 1
    num_up=$(hq worker list | grep RUNNING | wc -l)

done

snakemake -s Snakefile -j 1  --use-singularity  --cluster "hq submit --cpus 2 "

hq worker stop all
hq server stop
```
