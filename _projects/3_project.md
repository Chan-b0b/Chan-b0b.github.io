---
layout: page
title: Humanoid Manipulation
description: Vision-based pick-and-place manipulation with Unitree G1 in manufacturing environments
img:
importance: 3
category: robotics
---

## Humanoid Manipulation

**Platform:** Unitree G1, Dexmate Vega
**Method:** VLA fine-tuning (GR00T, SmolVLA, ACT) + IK-based control  
**Application:** Manufacturing PoC (LG Chem, LG Energy Solution), Logistics PoC (Market Kurly)

<br>

### Overview

Developed and deployed manipulation policies for the Unitree G1 humanoid robot targeting real manufacturing tasks. Combined VLA model fine-tuning with IK-based control for robust, deployable solutions.

<br>

**Key achievements:**
- Pick-and-place in unstructured manufacturing environments
- VLA models (GR00T, SmolVLA, ACT) fine-tuned for manufacturing-specific tasks
- Successfully deployed across multiple client PoCs

<br>

**PoC Applications:**
- **LG Chem** — Unitree A2 quadruped + arm for sample bottle collection automation
- **LG Energy Solution** — Dexmate Vega for battery cell sorting with IK-based control
- **Market Kurly** — Dexmate Vega for tote box stacking in logistics automation

<br>

### Demo

<div class="row mt-3 justify-content-center">
    <div class="col-4 mt-3 mt-md-0">
        {% include video.liquid path="assets/video/manipulation/G1_Manipulation.mp4" class="img-fluid rounded z-depth-1" controls=true %}
    </div>
</div>
<div class="caption">
    Unitree G1 humanoid manipulation policy for manufacturing pick-and-place tasks.
</div>