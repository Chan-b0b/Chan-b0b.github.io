---
layout: page
title: Humanoid Manipulation
description: Vision-based pick-and-place manipulation with Unitree G1 in manufacturing environments
img:
importance: 3
category: robotics
---

## Humanoid Manipulation

**Platform:** Unitree G1 Humanoid  
**Method:** VLA fine-tuning (GR00T, SmolVLA, ACT) + IK-based control  
**Application:** Manufacturing PoC (LG Chem, LG Energy Solution, Market Kurly)

### Overview

Developed and deployed manipulation policies for the Unitree G1 humanoid robot targeting real manufacturing tasks. Combined VLA model fine-tuning with IK-based control for robust, deployable solutions.

**Key achievements:**
- Pick-and-place in unstructured manufacturing environments
- VLA models (GR00T, SmolVLA, ACT) fine-tuned for manufacturing-specific tasks
- Successfully deployed across multiple client PoCs

**PoC Applications:**
- **LG Chem** — Unitree A2 quadruped + arm for sample bottle collection automation
- **LG Energy Solution** — Battery cell sorting with IK-based control
- **Market Kurly** — Tote box stacking for logistics automation

### Demo

<div class="row mt-3">
    <div class="col-sm mt-3 mt-md-0">
        {% include video.liquid path="assets/video/manipulation/G1_Manipulation.mp4" class="img-fluid rounded z-depth-1" controls=true %}
    </div>
</div>
<div class="caption">
    Unitree G1 humanoid manipulation policy for manufacturing pick-and-place tasks.
</div>

### Technical Stack

| Component | Detail |
|-----------|--------|
| Robot | Unitree G1, Unitree A2 |
| VLA Models | GR00T, SmolVLA, ACT |
| Control | IK-based trajectory + model inference |
| Data pipeline | SAM2-based auto-labeling, LeRobot |
| Organization | LG CNS Future Robotics Lab |


Every project has a beautiful feature showcase page.
It's easy to include images in a flexible 3-column grid format.
Make your photos 1/3, 2/3, or full width.

To give your project a background in the portfolio page, just add the img tag to the front matter like so:

    ---
    layout: page
    title: project
    description: a project with a background image
    img: /assets/img/12.jpg
    ---

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

You can also put regular text between your rows of images.
Say you wanted to write a little bit about your project before you posted the rest of the images.
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
