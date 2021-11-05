---
title: Containers and Workflows in Bioinformatics
author: CSC Training
---

# Material for _Containers and Workflows in Bioinformatics_ -course

{% assign items = site.hands-on |  sort: "title" | reverse %}

## 0. Course Preliminaries
###  [Slides](link)


## 1.  Introduction to CSC HPC environment and containers
### 1.1 [Slides:Introduction to CSC HPC environment](link)
### 1.2 [Slides:Fundamentals of containers](link)
### 1.3 Tutorials and exercises
{% for hands-on in items %}
{% if hands-on.topic == 'csc-hpc-containers' %}
1. [{{ hands-on.title }}]({{ hands-on.url | relative_url }})
{% endif %}
{% endfor %}

## 2. Using Pre-existing Images for Bioapplications
### 2.1 [Slides: Using container images in HPC environment](link)
### 2.2 [Slides:Containerised bio applications](link)
### 2.3 Tutorials and exercises
{% for hands-on in items %}
{% if hands-on.topic == 'bioapplications' %}
1. [{{ hands-on.title }}]({{ hands-on.url | relative_url }})
{% endif %}
{% endfor %}

## 3. Running Singularity on HPC Environment 
### 3.1 [Slides: Converting docker images to singularity images](link)
### 3.2 [Slides: Building singularity container images](link)
### 3.3 Tutorials and exercises
{% for hands-on in items %}
{% if hands-on.topic == 'singularity' %}
1. [{{ hands-on.title }}]({{ hands-on.url | relative_url }})
{% endif %}
{% endfor %}

## 4. Workflows (Nextflow) on HPC 
### 4.1 [Slides: Introduction to workflows](link)
### 4.2 [Slides: Workflows with singularity containers](link)
### 4.3 Tutorials and exercises
{% for hands-on in items %}
{% if hands-on.topic == 'nextflow' %}
1. [{{ hands-on.title }}]({{ hands-on.url | relative_url }})
{% endif %}
{% endfor %}

## Information
<p></p>

<p>
  <div style="float: left; width: 50%;">
   <img src="./slides/img/EuroCC_Logo_invert.png" width=110 align=middle/>
   <p><small>
     This project has received funding from the European High-Performance Computing Joint Undertaking (JU) under grant agreement No 951732.
      </small>
    </p>
  </div>
  <div style="float: right; width: 50%;">
    <img src="https://mirrors.creativecommons.org/presskit/buttons/88x31/png/by-sa.png" width=180>
    <p><small>
  All material (C) 2020-2021 by CSC -IT Center for Science Ltd.  <br />
  This work is licensed under a <strong>Creative Commons Attribution-ShareAlike</strong> 3.0 <br />
  Unported License, <a href="http://creativecommons.org/licenses/by-sa/4.0/">http://creativecommons.org/licenses/by-sa/4.0/</a>
      </small>
    </p>
  </div>
</p>
<p>&nbsp;</p>
   
