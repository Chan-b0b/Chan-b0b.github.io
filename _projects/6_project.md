---
layout: page
title: Jev Manipulation (WIP)
description: A LoRA-tuned Qwen3.5-4B as a "System One" robot policy — actions read as multiple-choice probabilities, from camera images only
img: assets/img/jev/thumbnail.jpg
importance: 6
category: robotics
---

**Simulation:** Meta-World (MuJoCo), Sawyer arm  
**Model:** Qwen3.5-4B + LoRA (r=16), served with vLLM  
**Status:** In progress

<br>

### Overview

Testing whether a general-purpose VLM can act as a fast, low-level robot policy in the style of a [Jev](https://typesafe.ai/blog/introducing-system-one-models-and-jev) "System One" model. The model never generates an action as text. Each step it answers one multiple-choice question per axis — decrease / hold / increase — plus one for the gripper, and the answer is read directly from the probabilities of the option letters (one token, no text parsing, so an answer can never go off-schema). Each axis moves by (P(increase) − P(decrease)) × 5 mm.

<br>

**Key points:**

- **LoRA fine-tuning of Qwen3.5-4B:** cross-entropy on the single answer-letter token, with labels computed exactly from a rule at every simulator state, so training data can be generated at any scale. Only 0.52% of the parameters are trained.
- **Instruction following, not just imitation:** training mixes in instructions that do not match the scene, labelled by the instruction rather than by the expert, so the model can only get them right by reading the instruction. Co-training with language rows (scene descriptions and motions in words) keeps the base model's own language and scene knowledge, so the policy draws on what Qwen already knows instead of only replaying learned motions.
- **0.2–0.3 s per control step (~3–5 Hz).**
- **Image-only input:** three camera views and the task in words, no coordinates.
- **Generalization tests:** 38 training tasks from Meta-World MT50, evaluated on held-out tasks never trained on — known scenes with new motions, and unseen scenes — and on randomized object and table colors.

<br>

### Results

**Base vs. LoRA** — closed-loop success rate, 50 episodes per task:

| Input                | Model | reach | push | pick-place | drawer-open |
| -------------------- | ----- | ----- | ---- | ---------- | ----------- |
| Text (coordinates)   | Base  | 0.70  | 0.00 | 0.00       | 0.00        |
| Text (coordinates)   | LoRA  | 1.00  | 1.00 | 1.00       | 1.00        |
| Images (two cameras) | Base  | 0.00  | 0.00 | 0.00       | 0.00        |
| Images (two cameras) | LoRA  | 1.00  | 0.98 | 1.00       | 1.00        |

The base model was given the same rules in the prompt; it already fails at comparing coordinates and applying the rules. Base with images was run for one episode per task.

<br>

**Held-out tasks** — 10 MT50 tasks never trained on, image input:

- With step-by-step instructions: success rate 1.00 on every task except stick-pull (0.90) and handle-pull-side (0.78)
- Without instructions: 0.35 on average

<br>

### Demo

<div class="row mt-3 justify-content-center">
    <div class="col-12 mt-3 mt-md-0">
        {% include video.liquid path="assets/video/jev/drawer-close-v3.mp4" class="img-fluid rounded z-depth-1" controls=true %}
    </div>
</div>
<div class="caption">
    drawer-close — a held-out task, never seen in training.
</div>

<div class="row mt-3 justify-content-center">
    <div class="col-12 mt-3 mt-md-0">
        {% include video.liquid path="assets/video/jev/pick-place-v3.mp4" class="img-fluid rounded z-depth-1" controls=true %}
    </div>
</div>
<div class="caption">
    pick-place.
</div>

<div class="row mt-3 justify-content-center">
    <div class="col-12 mt-3 mt-md-0">
        {% include video.liquid path="assets/video/jev/soccer-v3.mp4" class="img-fluid rounded z-depth-1" controls=true %}
    </div>
</div>
<div class="caption">
    soccer, with randomized colors. Each video shows the three camera inputs and, on the right, the model's probability for every option; the boxed option is the correct label.
</div>
