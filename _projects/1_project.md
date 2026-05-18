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

Trained locomotion control policies for the Unitree G1 humanoid robot using reinforcement learning in Isaac Lab simulation, then deployed to the physical robot. The key challenge was closing the sim-to-real gap under real manufacturing floor conditions.

**Key achievements:**
- Stable flat-ground walking with simultaneous upper-body operation
- Sim-to-Real robustness: compensated for physical asymmetries and hardware noise
- Policy generalization to unseen environmental disturbances

### Demo

<div class="row mt-3 justify-content-center">
    <div class="col-4 mt-3 mt-md-0">
        {% include video.liquid path="assets/video/locomotion/G1_Locomotion.mp4" class="img-fluid rounded z-depth-1" controls=true %}
    </div>
</div>
<div class="caption">
    Unitree G1 humanoid locomotion policy deployed on real hardware, trained via reinforcement learning in Isaac Lab.
</div>
