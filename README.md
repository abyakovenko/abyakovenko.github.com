# abyakovenko.github.com
This is a personal blog to record progress on my side-projects that I consider exciting.

---
layout: post
title:  "Autoregressive World Generation: Philosophy and Motivation"
date:   2025-06-28 17:27:00 +0100
categories: 3d-generation research-diary
---

I am starting a new project aimed at 3D interactive world generation. I have a number of requirements and naive expectations for what I want such a system to achieve.

### Core Requirements

My vision for this system is guided by four main principles:

1.  **Expandability:** The system must be reactive to exploration. It should be capable of adding new details and expanding the world as a viewer moves through it.

2.  **Interactivity:** The generated world must be dynamic. This includes physics simulations, animated objects, and fauna. Like modern video generation models, the world should feel alive and potentially respond to the actions of the observer.

3.  **Efficiency:** The generation and simulation processes must be efficient enough to deliver results in real-time, providing a truly interactive experience.

4.  **Controllability:** The tools for controlling the output should feel familiar. The user experience should be similar to how artists and creators already control and manipulate other 3D creations.

### The Medium and Engineering Decisions

A critical aspect is how the generated world is represented. What is its underlying structure, and how does that inform our engineering decisions? The output format must be easy to render, straightforward to modify for animation, and designed for expandability.

Traditional approaches like the Multi-Layer Perceptrons (MLPs) used in Neural Radiance Fields (`NeRFs`) are powerful for representation but are notoriously difficult to expand or edit. This leads me to believe the resulting approach must be:

-   **Primitive-based:** To ensure rendering is computationally efficient and the output is controllable.
-   **Hierarchical:** To allow for seamless expandability and level-of-detail scaling.
-   **Autoregressive:** To naturally combine the hierarchical structure with a primitive-based foundation.

This narrow set of choices points quite directly toward **Gaussian Splatting**. I'm not thinking of Gaussian Splats for mere scene representation, but for a more ambitious concept: *semantically enriched Gaussian Splatting*. It makes intuitive sense. Surfels are one of the simplest 3D primitives, and using Gaussians to represent objects and their constituent parts feels like a natural fit for a hierarchical, autoregressive model.

### On Training Data

The scarcity of internet-scale 3D data is a well-documented challenge. While amazing datasets like `Objaverse` and various multi-view collections exist, a pragmatic approach is necessary for large-scale pretraining.

My best shot seems to be making the system compatible with `RGBD` data, which can be sourced from internet-scale image datasets and enriched with predictions from state-of-the-art depth estimation models. An alternative route involves leveraging denoising diffusion models, but I would rather not give up and get on that path too early.

### The Path Forward

So, that's the plan: to build an autoregressive model that recursively splits large objects into smaller, more detailed parts depending on the viewer's proximity. This model will be trained on either true 3D representations from large-scale open datasets or on any available data that can be transformed into semantically enriched point clouds.

In this blog, I will be publishing my process of building this system. I'll document my experiments, design decisions, and all the relevant research I find helpful along the way.
