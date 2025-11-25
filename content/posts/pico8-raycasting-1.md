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

# Concept

To build a raycasting engine, we must first define the world as a 2D square grid where some cells represent a wall and other cells represent empty space.

The player is also represented in two dimensions as a position, direction, and field-of-view (FOV), and can freely move about and rotate in the world.

Each frame, the raycasting engine performs multiple raycasts along the player's FOV arc; one for each pixel column on the screen. This means that if your screen resolution is 128x128 (as is the case for PICO-8), then the screen width is 128 pixels therefore we would perform 128 raycasts per frame. The first raycast would correspond to screen coordinate X=0, the second raycast would correspond to screen coordinate X=1, etc.

Each ray traverses the grid until it either intersects with a cell that represents a wall, or exceeds the grid boundaries. If it exceeds the grid boundaries, we draw nothing. If it intersects with a wall, we draw a vertical line on the screen, at the screen coordinate X value that our ray represents, and we use the distance between the player and the point of intersection to determine how tall we should draw the line. The effect this should have is that wall segments that are farther away appear to be smaller on the screen, as it is in real-life.

Conceptually, that's all there is behind a basic raycaster. Further down, we'll get into the technical detail of how to implement this in PICO-8 and in future articles, we will expand upon this approach by adding textures and lighting.

There are, of course, limitations to this approach. The world geometry solely consist of blocks that are a single unit high, the player exists in only two dimensions and cannot go "up" or "down". However, these limitations are also what made raycasters fast. The simplicity allow us to make optimized assumptions based on the laws of geometry.

For example, we never have to worry about wall overdraw because, if all walls are 1 block high, then the first wall you see will always totally obscure all walls behind it. Being unable to move up/down means that we can never see the tops or bottoms of blocks, so we don't waste any processing power on rendering them.

Of course, computers have gotten more powerful, and so there are examples of more advanced raycasters that people have built that support more complex geometry, 3D navigation, etc.

# Implementation
