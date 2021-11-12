---
topic: bioapplications
title: Tutorial2 - WGS analysis with DeepVariant container 
---

## Analysis of whole genome sequencing (WGS) data using DeepVariant singularity container
Run [deepvariant method](https://github.com/google/deepvariant) to perform variant calling on WGS and WES data sets in Puhti supercomputing environment using singularity container. One needs to prepare deepvariant singualrity image, models and test data in order to run the pipeline. Additionally, other prerequisites for running deepvariant method includes 1) obtaining A reference genome in [FASTA](https://en.wikipedia.org/wiki/FASTA_format) format and its corresponding index file (.fai). 2) An aligned reads file in [BAM](http://genome.sph.umich.edu/wiki/BAM) format and its corresponding index file (.bai). For the sake of this tutorial, testdata is provided as a downloadable link in the following sections. 

### Expected learning from tutorial:
Upon completion of this tutorial you will learn to: 
- Prepare a singularity image (in this case, deepvariant) interactively from [DockerHub](https://hub.docker.com/)
- Deploy a singularity container as a batch job on Puhti cluster

### Run WGS analysis with DeepVarinat singularity container on Puhti

1. First login to Puhti supecomputer using *SSH*:
   ```bash
   ssh yourcscusername@puhti.csc.fi
   ```
2. Navigate to project scratch directory and prepare a folder for analysis:
   ```bash
    cd /scratch/project_xxxx/$USER   # replace xxxx with your project number
    mkdir deepvariant
    cd deepvariant
   ```
4. Start interactive session on Puhti:
   ```
    sinteractive -c 2 -m 4G -d 250
   ```
    One has to choose course project on the command prompt to start an interactive session.

5. Prepare singularity image from docker image for DeepVariant analysis:

    We want to use LOCAL_SCRATCH for Singularity tmp and cache. Unsetting XDG_RUNTIME_DIR will silence some unnecessary warnings. We will learn more about these 
    settings later in the course.

   ```bash
    export SINGULARITY_TMPDIR=$LOCAL_SCRATCH
    export SINGULARITY_CACHEDIR=$LOCAL_SCRATCH
    unset XDG_RUNTIME_DIR
    singularity build deepvariant_cpu_1.2.0.sif docker://google/deepvariant:1.2.0
   ```

6. Download and unpack the testdata for deepvariant analysis
   ```bash
    wget https://a3s.fi/containers-workflows/deepvariant_testdata.tar.gz
    tar -xavf deepvariant_testdata.tar.gz
   ```

7. Prepare slurm scripts to run on Puhti (e.g., deepvariant_puhti.sh):

   ```bash
   #!/bin/bash
   #SBATCH --time=00:10:00
   #SBATCH --partition=small     # You can also choose partition : "test" for this toy example
   #SBATCH --account=project_xxxx
   #SBATCH --ntasks=1
   #SBATCH --cpus-per-task=1
   #SBATCH --mem=4000

   singularity_wrapper exec deepvariant_cpu_1.2.0.sif \
   /opt/deepvariant/bin/run_deepvariant \
   --model_type=WGS   --ref=./testdata/ucsc.hg19.chr20.unittest.fasta \
   --reads=./testdata/NA12878_S1.chr20.10_10p1mb.bam \
   --regions "chr20:10,000,000-10,010,000" \
   --output_vcf=$PWD/output.vcf.gz \
   --output_gvcf=$PWD/output.g.vcf.gz
   ```
8. Submit your job to Puhti cluster

   ```bash
   sbatch deepvariant_puhti.sh
   ```
