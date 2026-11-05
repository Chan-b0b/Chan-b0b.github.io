---
layout: page
title: Real2Sim
description: Rebuilding the battery-cell handling PoC cell in simulation — 16 parallel environments of a mobile dual-arm humanoid running the full pick-and-place sequence
img: assets/img/real2sim/thumbnail.jpg
importance: 7
category: robotics
---

**Platform:** Dexmate Vega (mobile dual-arm humanoid)  
**Simulation:** Isaac Lab (NVIDIA)  
**Task:** Battery-cell pick-and-place from source trays to target trays with suction grippers

<br>

### Overview

Recreated the real manufacturing cell from the [battery cell sorting PoC](/projects/3_project/) as a simulation scene, so that the same task can be run, randomized, and evaluated at scale without occupying the physical robot. Each environment contains a mobile dual-arm humanoid, a set of source trays and target trays, and the same task logic used on the real system.

<br>

**Key points:**

- **16 parallel cells** running independent episodes of the full sequence: drive to the source station, pick a battery cell with suction, drive to the target station, place, and restage for the next layer
- **Contact-aware placement:** the part is lowered at a fixed speed until a contact force threshold is reached, then the pump is released and the arm retracts — mirroring the real deployment logic
- **Randomized source / target assignment** per episode, so trajectories cover the whole cell rather than one fixed route
- **Live per-environment status overlay:** current episode, source / target tray, sub-step, and cumulative layers completed and success count

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
