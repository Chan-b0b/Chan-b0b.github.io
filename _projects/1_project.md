---
layout: page
title: Humanoid Locomotion
description: RL-based locomotion on Unitree G1 — from Isaac Lab simulation to real-robot deployment
img:
importance: 1
category: robotics
---

## Humanoid Locomotion Control

**Platform:** Unitree G1 / H1-2 Humanoid  
**Simulation:** Isaac Lab (NVIDIA), MuJoCo  
**Algorithm:** PPO (Proximal Policy Optimization)

### Overview

Trained locomotion control policies for the Unitree G1/H1-2 humanoid robot using reinforcement learning in Isaac Lab simulation, then deployed to the physical robot. The key challenge was closing the sim-to-real gap.

**Key achievements:**
- Stable flat-ground walking with simultaneous upper-body operation
- Sim-to-Real robustness: compensated for physical asymmetries and hardware noise
- Policy generalization to unseen environmental disturbances

### Demo

<div class="row mt-3 justify-content-center">
    <div class="col-5 mt-3 mt-md-0">
        {% include video.liquid path="assets/video/locomotion/G1_Locomotion_IsaacLab.webm" class="img-fluid rounded z-depth-1" controls=true %}
    </div>
    <div class="col-5 mt-3 mt-md-0">
        {% include video.liquid path="assets/video/locomotion/G1_Locomotion_Mujoco.webm" class="img-fluid rounded z-depth-1" controls=true %}
    </div>
</div>
<div class="row mt-2 justify-content-center">
    <div class="col-3 mt-3 mt-md-0">
        {% include video.liquid path="assets/video/locomotion/G1_Locomotion.mp4" class="img-fluid rounded z-depth-1" controls=true %}
    </div>
</div>
<div class="caption">
    Training in Isaac Lab (top-left), sim-to-sim transfer to MuJoCo (top-right), and deployment on real hardware (bottom).
</div>

<div class="row mt-3 justify-content-center">
    <div class="col-5 mt-3 mt-md-0">
        {% include video.liquid path="assets/video/locomotion/H1_Squat_Arm_Move_IsaacLab.webm" class="img-fluid rounded z-depth-1" controls=true %}
    </div>
</div>
<div class="caption">
    H1-2 squat with simultaneous arm movement, trained in Isaac Lab.
</div>
