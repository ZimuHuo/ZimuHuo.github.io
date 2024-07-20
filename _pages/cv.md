---
layout: archive
title: "CV"
permalink: /cv/
author_profile: true
redirect_from:
  - /resume
---

{% include base_path %}

Education
======
* XXX, Radiology, University of Cambridge, 2023
* MEng, Biomedical Engineering, Imperial College London, 2019-2023

Research experience
======
* Research Assistant, Department of Bioengineering, Imperial College London (Oct 2022 – Oct 2023)
  * I Implemented reconstruction toolbox for parallel imaging, low rank reconstruction, and compressed sensing.
  * I built an automated segmentation pipeline using UNET based machine learning tools and quantitative analysis methods such as T2 and Apparent diffusion coefficient (ADC) mapping for musculoskeletal MRI and MRI neurography.
  * Supervisor: Dr. Neal Bangerter and Dr. Peter lally 

* Research Intern, Royal Brompton Hospital, National Heart and Lung Institute (June 2022-- Oct 2022)
  * I implemented reconstruction pipelines for echo planar imaging and simultaneous multi-slice imaging potentially allowing acquisition times to be shortened by a factor of 3.
  * I was actively involved in a clinical research study and had experience operating a Siemens scanner. 
  * Supervisor: Dr. Sonia Nielles-Vallespin and Dr. Andrew Scott

Skills
======
* Programming Languages: C/C++, Python, MATLAB, Java, SQL, JavaScript, HTML/CSS
* Software and Developer Tools: MRI pulse sequence development (Siemens IDEA, GE EPIC), MRI reconstruction tool (Gadgetron, Bart), Siemens MATLAB Parallel Transmit Toolbox, FSL, ITK


Publications
======
  <ul>{% for post in site.publications reversed %}
    {% include archive-single-cv.html %}
  {% endfor %}</ul>
  
Talks
======
  <ul>{% for post in site.talks reversed %}
    {% include archive-single-talk-cv.html  %}
  {% endfor %}</ul>
  
Teaching
======
  <ul>{% for post in site.teaching reversed %}
    {% include archive-single-cv.html %}
  {% endfor %}</ul>
  
Interest
======

* Cambridge Rowing club Darwin College 23-24
* Imperial Cross Country and Athletics 22-23
* Black and White photography 19-present

