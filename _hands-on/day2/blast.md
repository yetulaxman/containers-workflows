---
topic: bioapplications
title: Tutorial1 - BLAST example
---

There are many bioinformatics applications available as docker images in  different registries such as [Docker Hub](https://hub.docker.com), [Red Hat Quay](https://quay.io) and [Biocontainers](https://biocontainers.pro). Our aim here is to acquire some basic skills to use any containerised application from the registries for your bioinformatics analysis in HPC environment.  These skills greatly empowers bioinformatician as there are several thousands of containerised applictaions which can be easily deoployed in a reproducible manner. This tutorial provides hands-on experience with one of the popular bioinformatics tool called, BLAST which compares nucleotide or protein sequences to sequence in a database and calculates the statistical significance of the matches.

### Expected learning from tutorial:
After this tutorial you will be able to 
- Search a bioinformatics application (in this case, BLAST) as a docker image from a registry (from Red Hat Quay)
- Deploy the application as a singularity container in Puhti environment interactively


### Run BLAST analysis using a singularity container

1. Navigate to scratch folder and launch interactive terminal on Puhti

   ```bash 
   cd /scratch/project_xxxx/$USER
   sinteractive -c 2 -m 4G -d 250   # enter project number  upon prompting
   ``` 
2. We'll use a BLAST (Basic Local Alignment Search Tool) a image as available from [Quay Registry](https://quay.io). Visit the Quay registry webpage and search for 
   the BLAST image (using key word: BLAST) on the right hand top corner. You can find the blast images from different repositories/accounts. Pick the one under 
   biocontainer repository (i.e., biocontainers/blast). And asl search for different tags available for the image (on left side menu, click on *tags* icon). Once
   we have a fully qualified URI (= docker://hostname/repository/imagename:tag) for docker image, we can convert it to singularity image using **singularity build**
   subcommand as below:
    
   ```bash
   singularity build blast_quay.sif docker://quay.io/biocontainers/blast:2.12.0--pl5262h3289130_0
   ```

3. Run a simple command inside the singularity container to get help information from blastp  
   ```bash
   singularity exec blast_quay.sif blastp -help
   ```

4. Let's download and prepare the data needed to start analysis with BLAST tool

   ```bash
    wget https://www.uniprot.org/uniprot/Q61074.fasta # mouse Protein phosphatase 1G
    ```
   This is a mouse FASTA sequence.  We'll also need a reference database to blast against and can be downloaded here:

   ```bash
    curl -O ftp://ftp.ncbi.nih.gov/refseq/M_musculus/mRNA_Prot/mouse.1.protein.faa.gz
    gunzip mouse.1.protein.faa.gz
    ```
    We need to prepare the mouse database with `makeblastdb` for the search, so we'll run the following:

    ```bash
    mkdir makeblastdb
    msingularity exec -B $PWD:$PWD blast_quay.sif makeblastdb -in mouse.1.protein.faa -dbtype prot
    ```  
    After the container has terminated, you should see several new files in the current directory.
    
5. Finally, as we have all the input data ready for anlaysis, we can now do the final alignment step using `blastp` as below:

   ```bash
   singularity exec -B $PWD:$PWD blast.sif blastp -query Q61074.fasta -db mouse.1.protein.faa -out results.txt
   ```
   The final results are stored in `results.txt`;

   ```blast
   less results.txt   # you should be able to match some isoforms of phosphatases in mouses as best hits in the blast search
   ```

   ```bash
   Sequences producing significant alignments:                          (Bits)  Value

   XP_036016308.1 protein phosphatase 1B isoform X3 [Mus musculus]       129     3e-32
   XP_030105454.1 protein phosphatase 1B isoform X3 [Mus musculus]       129     3e-32
   XP_030105453.1 protein phosphatase 1B isoform X3 [Mus musculus]       129     3e-32
   XP_006523925.1 protein phosphatase 1B isoform X3 [Mus musculus]       129     3e-32
   XP_036016307.1 protein phosphatase 1B isoform X2 [Mus musculus]       129     4e-32
   XP_030105452.1 protein phosphatase 1B isoform X2 [Mus musculus]       129     4e-32
   XP_036016306.1 protein phosphatase 1B isoform X2 [Mus musculus]       129     4e-32
   XP_006523924.1 protein phosphatase 1B isoform X2 [Mus musculus]       129     4e-32
   XP_011244623.1 protein phosphatase 1B isoform X1 [Mus musculus]       130     4e-32
   XP_011244624.1 protein phosphatase 1B isoform X1 [Mus musculus]       130     4e-32
   XP_006523923.1 protein phosphatase 1B isoform X1 [Mus musculus]       130     4e-32
   XP_036013149.1 protein phosphatase 1A isoform X2 [Mus musculus]       128     6e-32
   XP_036013148.1 protein phosphatase 1A isoform X1 [Mus musculus]       127     3e-31
   [..]
  ```
  We can see that several proteins in the mouse genome match those in the mouse phasphatage (as expected).


