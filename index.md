---
title: Workflows and Containers in Bioinformatics
author: CSC Training
---

# Material for _Workflows and Containers in Bioinformatics_ -course

### Tutorials and exercises
{% for hands-on in items %}
{% if hands-on.topic == 'connecting' %}
1. [{{ hands-on.title }}]({{ hands-on.url | relative_url }})
{% endif %}
{% if hands-on.topic == 'Linux Prerequisites' %}
1. [{{ hands-on.title }}]({{ hands-on.url | relative_url }})
{% endif %}
{% endfor %}
1. [Advanced tutorial - Login Puhti with NoMachine](https://docs.csc.fi/support/tutorials/nomachine-usage/)
2. [Advanced tutorial - Run R studio/Jupyter Notebook on Puhti via ssh-tunnel and browser](https://docs.csc.fi/support/tutorials/rstudio-or-jupyter-notebooks/) This requires ssh-keys (see above) but is the recommended way to use these interactive tools.

## 2. Introduction to HPC environment
### 2.1 [Slides](https://a3s.fi/CSC_training/02_environment.html)
### 2.2 [Video: CSC Datacenter in Kajaani](https://www.youtube.com/watch?v=HeqN0h391wg)

## 3. Disk areas
### 3.1 [Slides](https://a3s.fi/CSC_training/03_disk_areas.html)
### 3.2 Tutorials and exercises
{% for hands-on in items %}
{% if hands-on.topic == 'disk-areas' %}
1. [{{ hands-on.title }}]({{ hands-on.url | relative_url }})
{% endif %}
{% endfor %}

## 4. Module system
### 4.1 [Slides](https://a3s.fi/CSC_training/04_modules.html)
### 4.2 Tutorials and exercises
{% for hands-on in items %}
{% if hands-on.topic == 'modules' %}
1. [{{ hands-on.title }}]({{ hands-on.url | relative_url }})
{% endif %}
{% endfor %}

## 5. Batch queue system and interactive use
### 5.1 [Slides](https://a3s.fi/CSC_training/05_batch_jobs.html)
### 5.2 Tutorials and exercises
{% for hands-on in items %}
{% if hands-on.topic == 'Batch jobs' %}
1. [{{ hands-on.title }}]({{ hands-on.url | relative_url }})
{% endif %}
{% endfor %}
1. [Exercise - Serial, array and parallel jobs with R + contours calculation from DEM with raster package (GIS) ](https://github.com/csc-training/geocomputing/tree/master/R/puhti)
1. [Exercise - Serial, array and parallel jobs with Python + NDVI calculation rasterio package (GIS) ](https://github.com/csc-training/geocomputing/tree/master/python/puhti)

## 6. Batch job resource usage
### 6.1 [Slides](https://a3s.fi/CSC_training/06_understanding_usage.html)
### 6.2 Tutorials and exercises
{% for hands-on in items %}
{% if hands-on.topic == 'batch resources' %}
1. [{{ hands-on.title }}]({{ hands-on.url | relative_url }})
{% endif %}
{% endfor %}

## 7. Allas and where to keep your data
### 7.1 [Slides](https://a3s.fi/CSC_training/07_allas.html)
### 7.2 [Video: Using Allas](https://youtu.be/q5PZot62kv8)
### 7.3 Tutorials and exercises
{% for hands-on in items %}
{% if hands-on.topic == 'allas' %}
1. [{{ hands-on.title }}]({{ hands-on.url | relative_url }})
{% endif %}
{% endfor %}

## 8. Installing your own software
### 8.1 [Slides](https://a3s.fi/CSC_training/08_installing.html)
### 8.2 Tutorials and exercises
{% for hands-on in items %}
{% if hands-on.topic == 'installing' %}
1. [{{ hands-on.title }}]({{ hands-on.url | relative_url }})
{% endif %}
{% endfor %}

## 9. Containers and Singularity
### 9.1 [Slides](https://a3s.fi/CSC_training/09_singularity.html)
### 9.2 Tutorials and exercises
{% for hands-on in items %}
{% if hands-on.topic == 'singularity' %}
1. [{{ hands-on.title }}]({{ hands-on.url | relative_url }})
{% endif %}
{% endfor %}

## 10. How to speed up jobs
### 10.1 [Slides](https://a3s.fi/CSC_training/10_speed_up_jobs.html)
### 10.2 Tutorials and exercises
{% for hands-on in items %}
{% if hands-on.topic == 'throughput' %}
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
   
