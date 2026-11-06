---
layout: page
title: Real2Sim
description: Rebuilding a real manufacturing cell in simulation to collect scalable training data for VLA policies and world models
img: assets/img/real2sim/thumbnail.jpg
importance: 7
category: robotics
---

**Platform:** Dexmate Vega (mobile dual-arm humanoid)  
**Simulation:** Isaac Lab (NVIDIA)  
**Purpose:** Scalable data collection for VLA policy and world model training  

<br>

### Overview

Recreated the real manufacturing cell from the [battery cell sorting PoC](/projects/3_project/) in simulation. On the real robot, every demonstration costs operator time and machine time, and the resulting dataset is small, narrow, and hard to extend. By moving the same task into a simulated copy of the environment, data for the next generation of models can be generated continuously and at scale.

<br>

**Key points:**

- **Real-to-sim reconstruction:** the physical cell — robot, source / target trays, parts, and the task flow used in deployment — is rebuilt as a simulation scene, so simulated episodes follow the same workflow as the real system
- **Scalable data collection:** many environments run in parallel with randomized layouts and assignments, producing far more diverse trajectories than real-robot teleoperation could in the same time
- **Training data for VLA policies:** collected episodes pair camera observations and language instructions with robot actions, providing the data needed to fine-tune vision-language-action models for this task
- **Toward world models:** the same rollouts also serve as observation-action sequences for training world models that predict how the scene evolves under the robot's actions, with the longer-term goal of planning and evaluating policies inside the learned model

<br>

### Demo

<div class="row mt-3 justify-content-center">
    <div class="col-12 mt-3 mt-md-0">
        {% include video.liquid path="assets/video/real2sim/Real2Sim.mp4" class="img-fluid rounded z-depth-1" controls=true %}
    </div>
</div>
<div class="caption">
    16 simulated cells running the battery-cell pick-and-place sequence in parallel. The overlay at the bottom shows each environment's current sub-task and the running layer / success counters.
</div>
