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
- **Remaining error:** the last 1–3 cm and, in one case, about 10° of cup yaw are not resolved by a single open-loop plan, which motivates re-planning at every step

<br>

### Closed-Loop Tracking (MPC)

The open-loop test asks for one plan and never looks again. In closed loop the planner runs at every control step: the newest three camera frames and a goal picture go to the world model, CEM picks a short plan (200 candidates, 10 rounds, 3 steps ahead), only the **first** action is executed for 0.2 s, and then planning starts over from what the camera actually shows.

Here the goal is not a single end picture but a moving target. A scripted expert first performs a motion while its head-camera frames are recorded; at each step the planner is given the expert's frame from 3 steps (0.6 s) in the future as the goal. The world model therefore has to **follow a target pose trajectory using pictures only** — it never receives the expert's actions or the target pose as numbers.

<div class="row mt-3 justify-content-center">
    <div class="col-12 mt-3 mt-md-0">
        {% include video.liquid path="assets/video/wm/mpc_track_toward.mp4" class="img-fluid rounded z-depth-1" controls=true loop=true muted=true %}
    </div>
</div>
<div class="caption">
    Run 1 — a 25 cm straight line toward the robot. Top left: the simulator's head camera after each action. Top right: the frame the world model predicted for that step. Bottom left: the goal picture currently being tracked. Bottom right: overview of the cell.
</div>

<div class="row mt-3 justify-content-center">
    <div class="col-12 mt-3 mt-md-0">
        {% include video.liquid path="assets/video/wm/mpc_track_away.mp4" class="img-fluid rounded z-depth-1" controls=true loop=true muted=true %}
    </div>
</div>
<div class="caption">
    Run 2 — a 16 cm straight line away from the robot.
</div>

<div class="row mt-3 justify-content-center">
    <div class="col-12 mt-3 mt-md-0">
        {% include video.liquid path="assets/video/wm/mpc_track_diagonal.mp4" class="img-fluid rounded z-depth-1" controls=true loop=true muted=true %}
    </div>
</div>
<div class="caption">
    Run 3 — a 25 cm diagonal line from the centre tray toward the side tray. The videos show simulation time only; the simulator is paused while the planner computes.
</div>

| Run | Distance to final target (start → end) | Tracking error along the way (mean / max) | Yaw error at the end |
| :--- | :---: | :---: | :---: |
| 1 — toward the robot | 25.1 cm → 2.1 cm | 2.2 cm / 4.0 cm | 3.2° |
| 2 — away from the robot | 16.1 cm → 2.1 cm | 2.1 cm / 3.9 cm | 3.1° |
| 3 — diagonal | 25.4 cm → 3.8 cm | 3.6 cm / 5.1 cm | 1.2° |

Tracking error is the distance between the cup and the expert's cup position at the same time step.

<div class="row mt-3 justify-content-center">
    <div class="col-12 mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/wm/mpc_track_toward_compare.png" class="img-fluid rounded z-depth-1" zoomable=true %}
    </div>
</div>
<div class="caption">
    Run 1 in numbers. Left three plots: the action chosen by the world model planner (orange) against the expert's action (blue) at each step, per axis. Right: the cup's path seen from above — the planner's path stays on the expert's line and stops next to the expert's end point.
</div>

**What this shows:**

- **The cup follows the target pose:** over 16–25 cm of travel the cup stays within about 2–4 cm of the moving target on average and never more than 5.1 cm away, and it ends 2–4 cm from the final pose with 1–3° of yaw error
- **The planner recovers the expert's motion from pictures:** along the main direction of travel the chosen step size matches the expert's (about 2.4 cm per step in Run 1), and the planner slows down and stops when the goal picture stops moving — without ever seeing the expert's actions
- **Predictions stay consistent with the simulator:** the predicted next frame (top right in the videos) places the arm and cup where the simulator's camera then shows them; the prediction gets blurrier when the arm moves toward the edge of the image, as at the end of Run 3
- **What is still rough:** the off-axis components of the action are noisy (about ±1 cm per step sideways and vertically), which is where most of the tracking error comes from, and Run 3 ends 3.8 cm short. Planning takes about 11 s per 0.2 s control step, so this is not yet real-time

<br>

### Next

- **Smoother, tighter tracking:** reduce the action noise and the remaining 2–4 cm error, and bring planning time down toward real-time
- **Beyond reaching:** goals that involve contact — picking a cell with suction and placing it in a tray pocket
- **Real robot:** the current model is trained on simulation only; adapting it to real camera frames is a later step
