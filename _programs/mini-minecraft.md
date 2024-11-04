---
layout: page
title: Mini Minecraft
description:
img: 
importance: 1
category: graphics
mathjax: true
---
<b>Note: Full code repository available upon request</b>

I developed a Minecraft-inspired game using C++ and OpenGL, tackling the challenges of voxel-based worlds with a focus on functional graphics and physics. I built a physics engine that handled basic interactions, such as collisions and movement, along with some camera controls and fluid mechanics for water and lava.

To get the game running smoothly, the game runs with multithreading, allowing for some concurrent processing of physics and rendering tasks. I also created a procedural generation system that generated decent landscapes using temperature and moisture mapping, pseudo-random noise functions, and some algorithmic terrain generation.

To improve visual effects, I implemented frame buffering and some custom shader programs, resulting in passable lighting and shading. I also developed a system for procedurally placing assets, such as natural elements and voxelized characters and objects, including some Mario-themed assets, to populate the environment in a somewhat balanced and engaging way.

<img src="mini-minecraft" alt="Easter Island Biome">