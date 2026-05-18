---
layout: page
title: IK + Imitation Learning
description: Independently developed IK+IL framework achieving 95% pick-and-place success on Unitree G1
img:
importance: 2
category: robotics
---

## IK + Imitation Learning Framework

**Platform:** Unitree G1 Humanoid, Dexmate Vega  
**Method:** Inverse Kinematics + Imitation Learning (DAgger)  
**Key Result:** 95% pick-and-place success rate

<br>

### Overview

Independently developed an **IK+IL combined learning framework** that significantly reduces dependency on teleoperation data. The core idea: use Inverse Kinematics to automatically generate diverse trajectory data, then apply DAgger-style training to blend IK-generated demonstrations with model inference progressively.

<br>

**Key achievements:**
- **95% pick-and-place success** on Unitree G1 without heavy teleoperation
- Drastically reduced human operator fatigue and data collection cost
- Reusable framework extended to LG Energy Solution battery cell sorting task

<br>

### Method

1. **IK trajectory generation** — automatically create diverse manipulation trajectories from object poses using Inverse Kinematics
2. **DAgger training** — progressively blend IK trajectories with model inference during training for stable policy improvement
3. **SAM2 auto-labeling** — text/query-point input → automatic YOLO training data generation, eliminating manual annotation

<br>

### Demo

<div class="row mt-3 justify-content-center">
    <div class="col-4 mt-3 mt-md-0">
        {% include video.liquid path="assets/video/ik_il/ik.webm" class="img-fluid rounded z-depth-1" controls=true %}
    </div>
    <div class="col-4 mt-3 mt-md-0">
        {% include video.liquid path="assets/video/ik_il/IK_IL.webm" class="img-fluid rounded z-depth-1" controls=true %}
    </div>
</div>
<div class="caption">
    IK+IL framework in action: automatic trajectory generation (left) and imitation learning policy execution (right).
</div>


