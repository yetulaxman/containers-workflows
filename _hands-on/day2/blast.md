---
topic: bioapplications
title: Tutorial1 - BLAST example
---

There are many bioinformatics applications available as docker images in  different registries such as [Docker Hub](https://hub.docker.com), [Red Hat Quay](https://quay.io) and [Biocontainers](https://biocontainers.pro). Our aim here is to acquire some basic skills to use any containerised application from the registries for your bioinformatics analysis in HPC environment.  These skills greatly empowers bioinformatician as there are several thousands of containerised applictaions which can be easily deoployed in a reproducible manner. This tutorial provides hands-on experience with one of the popular bioinformatics tool called, BLAST which compares nucleotide or protein sequences to sequence in a database and calculates the statistical significance of the matches.

### Expected learning from tutorial:
After this tutorial you will be able to 
- Search a bioinformatics application (in this case, BLAST) from a registry (from Red Hat Quay)
- Able to deploy in CSC Puhti environment interactively


### Running BLAST from a container with Docker ###

We'll be running a BLAST (Basic Local Alignment Search Tool) example with a container from [BioContainers](https://biocontainers.pro).  BLAST is a tool bioinformaticians use to compare a sample genetic sequence to a database of known seqeuences; it's one of the most widely used bioinformatics tools.

To begin, we'll pull the BLAST container (this will take a little bit):

Login to interactive node on Puhti

```
sinteractive -c 2 -m 4G -d 250
```

Visit quay.io registry and find the latest version of blast 

```
singularity build blast_quay.sif docker://quay.io/biocontainers/blast:2.12.0--pl5262h3289130_0
```


Run a simple command to get help information from blast  

```
singularity exec blast_quay.sif blastp -help
```

Let's download some data to start blasting:

```
# wget https://www.uniprot.org/uniprot/Q61074.fasta # mouse Protein phosphatase 1G
```

This is a human prion FASTA sequence.  We'll also need a reference database to blast against:

```
# curl -O ftp://ftp.ncbi.nih.gov/refseq/M_musculus/mRNA_Prot/mouse.1.protein.faa.gz
$ gunzip mouse.1.protein.faa.gz
```
{: .bash}

We need to prepare the zebrafish database with `makeblastdb` for the search, so we'll run the following (see a previous episode for details on `-v`):

```
mkdir makeblastdb
singularity exec -B $PWD:$PWD blast_quay.sif makeblastdb -in mouse.1.protein.faa -dbtype prot
```

After the container has terminated, you should see several new files in the current directory.  We can now do the final alignment step using `blastp`:

```
 singularity exec -B $PWD:$PWD blast.sif blastp -query Q61074.fasta -db mouse.1.protein.faa -out results.txt
```


The final results are stored in `results.txt`;

```
$ less results.txt   # you should be able to match some isoforms of phosphatases in mouses as best hits in the blast search
```

```
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

We can see that several proteins in the zebrafish genome match those in the human prion (interesting?).


