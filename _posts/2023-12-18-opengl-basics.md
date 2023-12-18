---
layout: post
title: "Exploring OpenGL: The WalkerOGL Project"
date: 2023-12-18
author: 'Walker McGilvary'
---

I'm excited to share my latest project, **WalkerOGL**. This endeavor is a testament to my interest in computer graphics and my journey in learning OpenGL. The project is a work in progress, where I am developing my own rendering engine using GLAD and GLFW.

## Project Overview

**WalkerOGL** started as an experiment with basic OpenGL functionalities. Here's a glimpse of what I've accomplished so far:

- **Initial Rendering:** Started with rendering a simple triangle to understand the basics.

![tri](https://i.imgur.com/Ydxuixe.png)

- **Progression to 3D:** Implemented perspective projection to render a 3D cube.
![cube](https://i.imgur.com/Mgrv5NU.png)

After understanding how the view frustum works, projecting the cube to the near plane is intuitive. It can be derived by considering similar triangles, with the specified FOV.
```cpp
// Projection transformation: Create a perspective projection matrix
float fov = radians(45.0f);
float nearPlane = 1.0f;
float farPlane = 100.0f;
float aspectRatio = static_cast<float>(SCR_WIDTH) / static_cast<float>(SCR_HEIGHT);
mat4 projection(
    1 / (aspectRatio * tan(fov / 2)), 0, 0, 0,
    0, 1 / (tan(fov / 2)), 0, 0,
    0, 0, -(farPlane + nearPlane) / (farPlane - nearPlane), -2 * farPlane * nearPlane / (farPlane - nearPlane),
    0, 0, -1, 0
);
projection = transpose(projection); //because glm is column major :(
```

This diagram shows the view frustum from the side, and it is clear how `y/f = Y/Z = tan(theta)`.
![similarTri](https://www.researchgate.net/publication/254666642/figure/fig4/AS:614080926736385@1523419710190/Perspective-projection-and-similar-triangles-A-cross-section-of-threedimensional-space.png)

- **Camera Controls:** Added simple camera controls to navigate around the 3D space.

## Current Focus

Currently, I'm working on implementing a basic lighting engine. The goal is to integrate phong lighting to enhance the realism and depth of the rendered objects.

## Code Highlights

The project's core lies in its GLSL shaders:
- Vertex Shader: `VertexShader.glsl`
- Fragment Shader: `FragmentShader.glsl`

These shaders are pivotal in rendering the graphics and will play a significant role in the upcoming lighting features.

## Contribute and Explore

I'm always open to contributions and feedback. Feel free to dive into the code, suggest improvements, or share your ideas.

Check out the repository here: [WalkerOGL on GitHub](https://github.com/TheSlabby/WalkerOGL)

Stay tuned for more updates as I continue to explore the fascinating world of OpenGL and computer graphics!

---
