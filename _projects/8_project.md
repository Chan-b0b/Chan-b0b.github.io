---
layout: page
title: World Model (WIP)
description: An action-conditioned world model for a manufacturing cell — the robot plans by imagining camera frames instead of querying a simulator
img: assets/img/wm/thumbnail.jpg
importance: 8
category: robotics
---

**Platform:** Dexmate Vega (mobile dual-arm humanoid)  
**Model:** DINO-WM style latent world model (frozen DINOv2 encoder + learned ViT predictor)  
**Training data:** 2,600 simulated episodes from the [Real2Sim](/projects/7_project/) cell  
**Purpose:** Predict how the scene changes under the robot's actions, and plan with those predictions  

<br>

### Overview

A world model learns the consequences of actions: given what the camera sees now and what the robot is about to do, it predicts what the camera will see next. Once those predictions are reliable, the robot can try out many action sequences in imagination and keep the one that leads to the desired picture — no task-specific policy and no reward function, only a goal image.

This project trains such a model for the battery-cell pick-and-place task, using episodes collected in the simulated copy of the real cell, and tests whether plans made inside the model hold up when executed in the physics simulator.

<br>

### Model

<div class="row mt-3 justify-content-center">
    <div class="col-12 mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/wm/architecture.png" class="img-fluid rounded z-depth-1" zoomable=true %}
    </div>
</div>
<div class="caption">
    Model and planning loop. Blue: image features from the frozen encoder. Green: the only part trained for dynamics. Amber: the planning loop that searches over actions.
</div>

- **Observation:** the head camera only, resized to 224 × 224. A frozen DINOv2 ViT-S/14 turns each frame into 256 patch tokens of 384 dimensions, so the model predicts in feature space rather than in pixels
- **Action (5-D):** the suction cup's position change (Δx, Δy, Δz), yaw change, and suction on / off, at the camera's 5 Hz. The same definition applies to the simulator and the real robot
- **Proprioception (6-D):** the cup's pose and whether it is holding a part
- **Predictor:** action and proprioception are embedded to 10 dimensions each and attached to every patch token (404-d). A 6-layer ViT reads the last 3 frames and predicts the next frame's tokens; feeding predictions back in gives a multi-step rollout
- **Decoder:** a separate decoder turns predicted tokens back into an image. It is used only to look at what the model imagines, and plays no part in planning
- **Training data:** 2,000 simulated episodes with randomized layouts and aim offsets, plus 600 with randomized floor colours and lighting so that the model does not depend on one visual appearance

<br>

### Planning with the Model

Given a start image and a goal image, a sampling-based optimizer (CEM) draws 300 candidate action sequences, rolls each one out inside the world model for 5 steps (1 second), and scores it by how close the imagined final frame is to the goal in feature space. The best candidates seed the next round; after 15 rounds the best plan is kept.

The plan is then executed **open loop** in the simulator: all five actions run without re-planning or looking at the camera again, and the result is compared with the goal.

<div class="row mt-3 justify-content-center">
    <div class="col-12 mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/wm/openloop_0.png" class="img-fluid rounded z-depth-1" zoomable=true %}
    </div>
</div>
<div class="row mt-3 justify-content-center">
    <div class="col-12 mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/wm/openloop_2.png" class="img-fluid rounded z-depth-1" zoomable=true %}
    </div>
</div>
<div class="row mt-3 justify-content-center">
    <div class="col-12 mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/wm/openloop_5.png" class="img-fluid rounded z-depth-1" zoomable=true %}
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
- **Remaining error:** the last 1–3 cm and, in one case, about 10° of cup yaw are not resolved by a single open-loop plan

<br>

### Next

- **Closed-loop planning (MPC):** re-plan from the newest camera frame every step, so that errors are corrected as they appear
- **Beyond reaching:** goals that involve contact — picking a cell with suction and placing it in a tray pocket
- **Real robot:** the current model is trained on simulation only; adapting it to real camera frames is a later step
