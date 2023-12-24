---
layout: post
title: "Creating a Voxel Engine with OpenGL"
date: 2023-12-24
---

After learning the fundamentals of OpenGL in C++, I've started a new project: **VoxelEngine**. I've laid the foundation for a voxel engine by integrating model importing with ASSIMP and implementing advanced lighting effects for the rendered voxels. This first step marks the beginning of my journey with OpenGL and showcases the potential of realistic lighting in voxel-based computer graphics.

## Creating Voxels

![voxelengine texture start](https://github.com/TheSlabby/TheSlabby.github.io/assets/33563846/f545cfda-7d16-41a4-95e4-bcdf74ca78b7)
The first step in this project is to render a textured voxel (cube). Using ASSIMP, I loaded a simple 16x16 texture atlas for all the voxels I'll use in this game. This approach is foundational in creating the diverse and intricate world of my voxel engine, providing a visual base for the complex systems to come.


## Chunks and Terrain Generation with Perlin Noise

<video width="700" height="500" controls>
  <source src="/assets/terraingeneration.mp4" type="video/mp4">
  Your browser does not support the video tag.
</video>
As you can see, I've already setup diffuse lighting and ambient lighting from my WalkerOGL project.

![voxelengine](https://github.com/TheSlabby/TheSlabby.github.io/assets/33563846/36f25737-1cd7-4ca7-8a6d-3e2c44903e3f)

I decided to take a common approach among voxel engines, which is rendering voxels using chunks. Each chunk holds an array of block IDs (`uint8_t[16][64][16]`). Because there is a lot of empty space in each chunk, I plan on using an octree datastructure in the future to store block IDs.
For simple terrain generation, I used exponential perlin noise. This code snippet shows the simple terrain generation function, and returns a block ID (uint8_t). This method of terrain generation allows for a naturally varied landscape, setting the stage for a more immersive and dynamic world.
```c++
uint8_t Chunk::generateTerrain(int x, int y, int z) {
	double noise = perlin.octave2D_01((x * 0.01), (z * 0.01), 4);
	noise = pow(noise, 3);
	int height = noise * 70;
	if (y > height) {
		return 255;
	}
	else if (y == height) {
		//snow
		int variance = ((float)rand() / RAND_MAX) * 7; //between 0-7 (so not all snow is the same height, it looks goofy lol)
		if (y > 20 + variance)
			return 3;	//snow
		return 0;		//grass
	}
	else {
		return 1;		//dirt
	}
}
```
In the main application class, I store chunks in a vector of smart pointers. I decided that because a relatively small number of chunks will be loaded at a time, I can just use a vector datastructure.
The reason I decided to use smart pointers is because of memory management. Chunks hold all block data and openGL vertex information, and the destructor properly frees up memory. Earlier, when iterating with a foreach loop, the Chunk instance was copied and this was a memory disaster.
After using smart pointers for this project, I feel I have a much more intuitive understanding of memory management and pointers in C++.

## Shadow Mapping
![shadowmapping](https://github.com/TheSlabby/TheSlabby.github.io/assets/33563846/a8dfe3c9-f4de-4baf-a6a0-e8d0c7c26b92)
![image](https://github.com/TheSlabby/TheSlabby.github.io/assets/33563846/a3924532-6f40-4c6e-845c-ad579c8e6ec1)
Finally, I added shadow mapping to my engine. This involved setting up a separate framebuffer (FBO) to hold depth information from the light's perspective, then compare that with the depth information rendered from the camera's POV. I then used a bias & texels to improve the shadows.
I was suprised at how well this engine was performing so far. On my laptop RTX3050, I run over 200fps while rendering about 5,000,000 blocks. This includes 4096x4096 shader resolution and chunk generation. The addition of shadow mapping not only enhances the visual depth of the scene but also adds a layer of realism that is crucial for immersive gameplay.

## UI for Debugging
To facilitate development and debugging, I've incorporated a user interface using ImGui. This interface displays crucial real-time data such as camera position, chunk position, the number of chunks loaded, and the current frames per second (fps). These features are invaluable for monitoring performance and understanding how different elements interact within the engine. The ability to see these metrics in real-time greatly aids in the iterative process of optimizing and refining the engine's performance and functionality, and this was crucial for debugging the chunk memory management.
![image](https://github.com/TheSlabby/TheSlabby.github.io/assets/33563846/9c93905e-9088-4c3b-b170-f6a89a378033)


## Future Plans
In the future, I plan on adding more lighting techniques, such as bloom & SSAO. I also plan on using different data structures for further optimization. These enhancements aim to increase the visual fidelity and performance of the engine, ensuring a seamless and engaging user experience.

## Wrapping Up
The journey of building a voxel engine with OpenGL has been a challenging yet rewarding endeavor. Through each stage, from creating basic voxels to implementing advanced lighting techniques, the project has grown in complexity and capability. I'm excited to continue this journey, pushing the boundaries of what's possible with my VoxelEngine project.

I heavily referenced [LearnOpenGL's Tutorial](https://learnopengl.com/Advanced-Lighting/Shadows/Shadow-Mapping).
[GitHub Repo for VoxelEngine](https://github.com/TheSlabby/VoxelEngine) 
