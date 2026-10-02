---
layout: page
title: Robotic Juggling (WIP)
description: RL-based dynamic juggling on Dexmate — training in Isaac Lab
img: assets/img/juggling/thumbnail.jpg
importance: 4
category: robotics
---

**Platform:** Dexmate  
**Simulation:** Isaac Lab (NVIDIA)  
**Algorithm:** PPO (Proximal Policy Optimization)  
**Status:** In progress — training ongoing, no real-robot deployment yet

<br>

### Overview

Training a Dexmate arm to juggle using reinforcement learning in Isaac Lab. Unlike the quasi-static pick-and-place tasks in the [manipulation project](/projects/3_project/), juggling requires continuous, highly dynamic control — catching and re-throwing an object under tight timing constraints — making it a good testbed for agile, high-frequency manipulation policies.

<br>

### Training

<div class="row mt-3 justify-content-center">
    <div class="col-8 mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/juggling/dexmate_juggling_isaaclab.png" class="img-fluid rounded z-depth-1" zoomable=true %}
    </div>
</div>
<div class="caption">
    Massively parallel training in Isaac Lab — thousands of Dexmate instances learning to juggle simultaneously.
</div>

<br>

### Demo

<div class="row mt-3 justify-content-center">
    <div class="col-8 mt-3 mt-md-0">
        {% include video.liquid path="assets/video/juggling/Juggling.mp4" class="img-fluid rounded z-depth-1" controls=true %}
    </div>
</div>
<div class="caption">
    Juggling policy rollout.
</div>
