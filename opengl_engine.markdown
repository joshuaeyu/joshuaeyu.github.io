---
layout: default
title: Graphics Engine in OpenGL
permalink: /opengl_engine/
---


# "Plum" - A simple graphics engine

[![Plum graphics engine screenshot](/graphics/screenshot0.png "Plum graphics engine screenshot")](/graphics/screenshot0.png)
**[github.com/joshuaeyu/plum](https://github.com/joshuaeyu/plum)**

Plum is a simple real-time graphics engine built using OpenGL 4.1 and C++17. It features a deferred rendering pipeline and physically based shading. Users can instantiate lights, primitives, and models into the scene from the engine's GUI.

My goal in creating this engine was to synthesize and expand upon the concepts taught in Learn OpenGL. This has been my first foray into 3D graphics programming with a low-level graphics API, so the program design and structure are still pretty raw (apologies!). I am still actively developing this (see To Do) but plan to begin work on a Vulkan-based project once I feel like this has reached a nice stopping point.

## Features
### Renderer
* Deferred rendering (plus a forward rendering pass)
* Physically based rendering
    * Metallic-roughness material support
    * Direct and image-based lighting
* 3D model support - .obj, .gltf, .3mf
* Primitives - sphere, cube, rectangle
* Directional and point lights 
* Shadow mapping
* Skybox
* Post processing
    * SSAO (screen-space ambient occlusion)
    * HDR tone mapping
    * Bloom
    * FXAA (fast approximate antialiasing)

### Interface
* User-specifiable...
    * Shape templates (primitives with colored, textured, or hybrid surfaces)
    * Scene - skybox, light sources, nodes
* First-person freefly camera
* FPS counter

## To-do
* Rendering
    * Mesh instancing with glDraw*Instanced()
    * PBR specular-glossiness workflow support
    * Blending / transparency support
    * [Clustered shading](https://www.humus.name/Articles/PracticalClusteredShading.pdf)
    * Scene graph hierarchy
* Interface
    * Preview new scene nodes before publishing to scene
    * File system explorer to load assets from GUI
    * Save/load scene state
* Software design
    * Split .hpp files into proper .hpp headers and .cpp definitions
    * Abstract further

Check out the repo for more details!