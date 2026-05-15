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

### Technical Stack

| Component | Detail |
|-----------|--------|
| Robot | Unitree G1, H1-2 |
| Simulator | Isaac Lab, MuJoCo |
| Algorithm | PPO |
| Transfer method | Domain randomization + physical asymmetry compensation |
| Organization | LG CNS Future Robotics Lab |

<div class="row">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/1.jpg" title="example image" class="img-fluid rounded z-depth-1" %}
    </div>
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/3.jpg" title="example image" class="img-fluid rounded z-depth-1" %}
    </div>
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/5.jpg" title="example image" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    Caption photos easily. On the left, a road goes through a tunnel. Middle, leaves artistically fall in a hipster photoshoot. Right, in another hipster photoshoot, a lumberjack grasps a handful of pine needles.
</div>
<div class="row">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/5.jpg" title="example image" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    This image can also have a caption. It's like magic.
</div>

You can also put regular text between your rows of images, even citations {% cite einstein1950meaning %}.
Say you wanted to write a bit about your project before you posted the rest of the images.
You describe how you toiled, sweated, _bled_ for your project, and then... you reveal its glory in the next row of images.

<div class="row justify-content-sm-center">
    <div class="col-sm-8 mt-3 mt-md-0">
        {% include figure.liquid path="assets/img/6.jpg" title="example image" class="img-fluid rounded z-depth-1" %}
    </div>
    <div class="col-sm-4 mt-3 mt-md-0">
        {% include figure.liquid path="assets/img/11.jpg" title="example image" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    You can also have artistically styled 2/3 + 1/3 images, like these.
</div>

The code is simple.
Just wrap your images with `<div class="col-sm">` and place them inside `<div class="row">` (read more about the <a href="https://getbootstrap.com/docs/4.4/layout/grid/">Bootstrap Grid</a> system).
To make images responsive, add `img-fluid` class to each; for rounded corners and shadows use `rounded` and `z-depth-1` classes.
Here's the code for the last row of images above:

{% raw %}

```html
<div class="row justify-content-sm-center">
  <div class="col-sm-8 mt-3 mt-md-0">
    {% include figure.liquid path="assets/img/6.jpg" title="example image" class="img-fluid rounded z-depth-1" %}
  </div>
  <div class="col-sm-4 mt-3 mt-md-0">
    {% include figure.liquid path="assets/img/11.jpg" title="example image" class="img-fluid rounded z-depth-1" %}
  </div>
</div>
```

{% endraw %}
