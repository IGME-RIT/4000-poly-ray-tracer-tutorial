Documentation Author: Niko Procopi 2019

This tutorial was designed for Visual Studio 2017 / 2019
If the solution does not compile, retarget the solution
to a different version of the Windows SDK. If you do not
have any version of the Windows SDK, it can be installed
from the Visual Studio Installer Tool

Welcome to the Ray Tracing Multi-OBJ Tutorial!
Prerequesites: 
	Ray Tracing Multi-Texture

This tutorial requires so much power to render, I had to lower the resolution to 
640x360 on my GTX 1050, or else it would render nothing. To render this in
higher resolution, I had to switch to an RTX card. The buffer that sends the triangle
array from the compute shader to the fragment shader is now gone. The buffer 
that exports from Compute and is imported to fragment has an identical structure
to the buffer that sends the array of meshes from the C++ code to the Compute Shader.

A big bug was fixed in this tutorial. Previously, OBJ Meshes could not be translated.
In the position vector (x, y, z, w) for each vertex. W was zero, which disables translation.
In this tutorial, I set W in every vertex (in the OBJ loader) to one.

To add a new model, we need to change MAX_MESHES, sometimes we have to change
MAX_TRIANGLES_PER_MESH, and we have to change NUM_TRIANGLES_IN_SCENE.
These need to be changed in main.cpp, Compute Shader, and the Fragment Shader.
We load these in the init() function. We also need to assign a texture to each mesh. We can 
load new textures by increasing MAX_TEXTURES. In Init(), we can call LoadTexture for each
new texture that we want loaded. In this case, we load 4 textures. We can load new OBJs
by calling LoadOBJ later in the init() function. In this case, we load Wheel.3Dobj one time for
meshes[3], then we use memcpy to duplicate it for other meshes (4 wheels). 

After that, we need to assign a texture to each model, and we need to set SRT for each model.
In the RenderScene function, you can edit the "test" array, which holds one 4x4 matrix for each
mesh. These matrices handle translation, rotation, and scale. After doing that, we use glUniform1i
to set a texture to each mesh. For example, 
	glUniform1i(tex_loc[7], m_texture[2]);
Will give the 3rd texture to the 8th mesh. In this case, this gives the Cat texture to the Cat mesh.

To make advanced tutorials like this compatible with low-end graphics cards, there must be
some way to change the OpenGL timeout, but I am not sure how. OpenGL appears to automatically
crash if it takes more than 1 second to render a frame. There should be a way to override this. If
there is no way to override, then I can change to a new graphics API like Vulkan