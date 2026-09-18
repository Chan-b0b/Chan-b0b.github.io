---
layout: page
title: Gemini-ER
description: Gesture-instruction manipulation on Dexmate Vega 1P — Gemini Embodied Reasoning for scene understanding & planning, IK-based execution
img:
importance: 5
category: robotics
---

**Platform:** Dexmate Vega 1P  
**Method:** Gemini Embodied Reasoning (Gemini-ER) for instruction analysis & planning + IK-based control

<br>

### Overview

Used Gemini-ER to interpret gestural instructions given to the robot, plan the corresponding manipulation sequence, and execute it via IK-based control. The robot grasps the object pointed at by a finger with an appropriate grasp pose, then places it onto an open palm presented to it.

<br>

**Key achievements:**

- Gesture-conditioned instruction understanding: identifying the pointed-at object and the target palm from the scene
- Grasp pose planning appropriate to each object, driven by Gemini-ER's reasoning output
- End-to-end pipeline from instruction analysis to planning to IK-based action execution

<br>

### Demo

<div class="row mt-3 justify-content-center">
    <div class="col-6 mt-3 mt-md-0">
        {% include video.liquid path="assets/video/gemini_er/Side.mp4" class="img-fluid rounded z-depth-1" controls=true %}
    </div>
    <div class="col-4 mt-3 mt-md-0">
        {% include video.liquid path="assets/video/gemini_er/head.mp4" class="img-fluid rounded z-depth-1" controls=true %}
    </div>
</div>
<div class="caption">
    Side view (left) and robot head-camera view (right) of Gemini-ER guided pointing-to-grasp and palm-placement on Dexmate Vega 1P.
</div>
