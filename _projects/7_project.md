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

<br>

### World Model: Planning from Imagined Rollouts

The simulated episodes are used to train an action-conditioned world model (DINO-WM style: a frozen DINOv2 image encoder with a learned latent dynamics predictor). The model sees only the head camera and a 5-D action — the suction cup's position delta, yaw delta, and suction on/off at 5 Hz — and predicts how the scene will look after each action.

To test whether the model is good enough to act with, it is used as the planner's only source of prediction. Given a start image and a goal image, a sampling-based optimizer (CEM, 300 candidate action sequences, 15 refinement rounds) searches for the 5-step plan whose imagined final frame is closest to the goal in feature space. The plan is then executed **open loop** in the simulator — all five actions are run without re-planning or looking at the camera again — and the result is compared with the goal.

<div class="row mt-3 justify-content-center">
    <div class="col-12 mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/real2sim/wm/openloop_0.png" class="img-fluid rounded z-depth-1" zoomable=true %}
    </div>
</div>
<div class="row mt-3 justify-content-center">
    <div class="col-12 mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/real2sim/wm/openloop_2.png" class="img-fluid rounded z-depth-1" zoomable=true %}
    </div>
</div>
<div class="row mt-3 justify-content-center">
    <div class="col-12 mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/real2sim/wm/openloop_5.png" class="img-fluid rounded z-depth-1" zoomable=true %}
    </div>
</div>
<div class="caption">
    Three open-loop reach plans. In each row, from left to right: the start frame, the goal frame, the world model's imagined frame at the end of the plan, and the simulator's actual frame after running the plan. The bar at the bottom shows the cup's distance to the goal before and after.
</div>

| Goal | Cup-to-goal distance (start → after plan) | Yaw error after plan |
| :--- | :---: | :---: |
| Left edge of the tray (top) | 15.3 cm → 1.4 cm | 9.9° |
| Right pocket (middle) | 17.9 cm → 1.6 cm | 0.6° |
| Centre of the tray (bottom) | 13.1 cm → 2.9 cm | 1.7° |

<br>

**What this shows:**

- **The imagined outcome matches the simulated one:** the model's predicted final frame and the simulator's frame agree on where the arm and cup end up, so plans chosen inside the model carry over to the physics simulation
- **Planning from images alone reaches the goal region:** with no access to the simulator state, a single 1-second plan closes a 13–18 cm gap to within 1.4–2.9 cm
- **Remaining error:** the last 1–3 cm and, in one case, about 10° of cup yaw are not resolved by a single open-loop plan; closing that gap with closed-loop re-planning (MPC) is the current work
