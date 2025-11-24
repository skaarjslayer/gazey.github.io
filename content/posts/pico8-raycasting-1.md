---
title: "Raycasting in PICO-8, Part 1: Walls"
date: 2025-11-20T18:29:40-05:00
draft: true
toc: false
images:
tags:
  - pico8
  - raycasting
  - raycaster
---

This article will be the first in a comprehensive series about how to build a raycasting engine (otherwise known as a raycaster) in PICO-8.

# Introduction

## What is a raycast?

In its most basic form, a raycast is the act of projecting a ray in either 2D or 3D space to detect if and where it intersects with another piece of geometry. They are commonly used in game engines for things such as collision detection, object selection, visibility checking; to name a few.

## What is a raycasting engine?

In the early 1990s, before the first [3D accelerator graphics cards](https://en.wikipedia.org/wiki/3dfx#Company_history) began to appear on the market, computers were simply not powerful enough for true real-time 3D rendering. In 1991, John Carmack devised a raycasting-based engine to render pseudo-3D game environments.

{{< figure src="/images/posts/pico8-raycasting-1/hovertank3d.png" alt="Screenshot of the video game Hovertank 3D" caption="Hovertank 3D (1991), developed by id Software." >}}

{{< figure src="/images/posts/pico8-raycasting-1/wolfenstein3d.png" alt="Screenshot of the video game Wolfenstein 3D" caption="Of course, the most popular game to use a raycasting engine was Wolfenstein 3D (1992)." >}}

{{< figure src="/images/posts/pico8-raycasting-1/arena.png" alt="Screenshot of the video game The Elder Scrolls I: Arena" caption="The Elder Scrolls I: Arena (1994), developed by Bethesda Softworks." >}}

The first example of this technique was Hovertank 3D, developed by id Software and released in April 1991. It was later refined and popularized in games such as Wolfenstein 3D (1992) and The Elder Scrolls I: Arena (1994), both of which used raycasting to achieve fast, immersive first-person visuals on limited hardware.

Note: Raycasting should not be confused with raytracing, which is a rendering technique that uses recursive raycasts from intersection points to calculate reflections, refractions, shadows, and other complex lighting effects.
