---
layout: page
title: Pytorch on HPC
description: Presentation on using Pytorch on the HPC cluster
img: assets/img/superpod-crop.png
importance: 6
category: school
---
<div class="row">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid path="assets/img/superpod.jpg" title="superpod" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    NVIDIA DGX SuperPOD
</div>

This is a brief project I did for a class on High Performance Computing. Our goal was basically to do any task using SMU's [HPC resources](https://www.smu.edu/OIT/Research/HPC). We were to improve performance in some way (the main focus of the course) and make a short presentation summarizing the results. SMU is fortunate enough to have a large CPU cluster and a brand new GPU cluster. The GPU cluster has 20 NVIDIA DGX nodes - each with eight A100's.

Because I am interested in deep learning, I wanted to use this project as an opportunity to set up and debug a suitable environment on the cluster. For my project, I chose to train a large enough neural network to tax the CPU-based machines and then compare the performance with an A100. Some of the highlights are included in the slideshow below. In short, the GPU performance was several orders of magnitude faster. I had fun with this project and learned a great deal about working with these resources. It was a great opportunity to brush up on ssh, linux, and the cli, and getting things to run well on the cluster ended up being more difficult than it sounds.

The slideshow below is made with [Quarto](https://quarto.org/). It is a great tool for making reproducible documents and presentations, and I am beginning to prefer it for most of my work. Hit `f` to view fullscreen.

<div class="container">
  <div class="row">
    <div class="col-md-12">
      <iframe src="/assets/html/presentation.html" style="width:100%; height:800px; border:none;"></iframe>
    </div>
  </div>
</div>
