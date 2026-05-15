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

### Overview

Independently developed an **IK+IL combined learning framework** that significantly reduces dependency on teleoperation data. The core idea: use Inverse Kinematics to automatically generate diverse trajectory data, then apply DAgger-style training to blend IK-generated demonstrations with model inference progressively.

**Key achievements:**
- **95% pick-and-place success** on Unitree G1 without heavy teleoperation
- Drastically reduced human operator fatigue and data collection cost
- Reusable framework extended to LG Energy Solution battery cell sorting task

### Method

1. **IK trajectory generation** — automatically create diverse manipulation trajectories from object poses using Inverse Kinematics
2. **DAgger training** — progressively blend IK trajectories with model inference during training for stable policy improvement
3. **SAM2 auto-labeling** — text/query-point input → automatic YOLO training data generation, eliminating manual annotation

### Demo

<div class="row mt-3 justify-content-center">
    <div class="col-4 mt-3 mt-md-0">
        {% include video.liquid path="assets/video/ik_il/IK_IL.webm" class="img-fluid rounded z-depth-1" controls=true %}
    </div>
    <div class="col-4 mt-3 mt-md-0">
        {% include video.liquid path="assets/video/ik_il/ik.webm" class="img-fluid rounded z-depth-1" controls=true %}
    </div>
</div>
<div class="caption">
    IK+IL framework in action: automatic trajectory generation (left) and imitation learning policy execution (right).
</div>

### Technical Stack

| Component | Detail |
|-----------|--------|
| Robot | Unitree G1, Dexmate Vega |
| Control | Inverse Kinematics (IK) |
| Learning | Imitation Learning, DAgger |
| Auto-labeling | SAM2 + YOLO |
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
