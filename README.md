# unity-solidwire-shader

![Unity SolidWire shader preview image.](/rm_intro.gif "Unity SolidWire shader preview image.")

## Disclaimer

This is one of the first shaders I've ever written for Unity. Chances are, its implementation can be improved upon. Please enjoy it for what it is, but don't pick up the bad habits I've yet to shake.

## Requirements

__Basic knowledge of Blender:__
This shader requires all meshes it's used with to have been processed by the provided *BlenderScripts/scr_BlenderSolidWireExport.py* Blender script.

## Overview

SolidWire is a shader designed for Unity to render 3D meshes in a similar visual style to that of the Vectrex. It will renders an outline around the edges of the mesh, but also allows specific edges of the mesh to be set to be drawn at all times (kind of like a texture, but still a part of the mesh). This is hard to explain, so I'll just illustrate it:

![Blender preview](/rm_blender.png "Blender preview.") ![Unity preview](/rm_unity.png "Unity preview.")

#### How it works

For each edge in the mesh, the shader goes through the following rules to decide whether to render it or not:

- If the edge has been marked as "sharp" in Blender, then always draw the edge (if it's visible). _See: the smily face on the box above._
- If the edge has been marked as a "seam" in Blender, then __never__ render it.
- If neither of the above, then:
    - If the edge belongs to two tris, both of which have not been backface culled, __don't__ render it. _See: the edge to the left of the smily face above._
    - If the edge belongs to two tris, one of which has been backface culled, and one of which that hasn't, draw it. _See: the outer edges of the cube above._
    - If the edge belongs to only one tri, then always draw it.

#### Loose edges in the mesh

In the dancing stickbug .gif at the top of this page, you'll notice its legs are drawn as single lines. In Blender, the stickbug mesh looks like this:

![Blender preview](/rm_stick.png "Blender preview.")

However, Unity doesn't support loose edges in meshes (if you import a mesh with them, they'll just be removed)!

This issue is solved by the provided Blender script, which will generate a tri for each loose edge it finds in your mesh, and thus, allow it to be imported into Unity.

## Usage

Follow these steps to successfully import and use a mesh with SolidWire into the __example project__.

##### In Blender
1. Create the mesh you want in Blender.
1. Apply all "__Generate__ modifiers" it has (needing to do this will be removed in a later patch).
1. Select it in Object mode, and then run the scr_BlenderSolidWireExport.py script.
1. Export the mesh as a .fbx for Unity. 

##### In Unity
1. Go to the import settings of your mesh, and apply the following settings:
![Unity mesh import settings](/rm_settings.png "Unity mesh import settings.")
1. Apply the settings, then add your mesh to the Scene.
1. Add the SolidWire material and SolidWire script to your mesh object in the scene.
1. Congratulations, you should now have a SolidWire mesh to look at (don't worry, most of the edges will render __only while the game is running__).

#### Usage outside of the example project
You'll need to copy the SolidWire shader, material, and script to your other project. In addition, the "glow" effect is being generated by a __Post-processing bloom__ effect applied to the camera. If you wish to copy my settings, they are:

![Post-processing bloom settings](/rm_bloom.png "Post-processing bloom settings.")

## Additional information

I've added comments all throughout my code if you desire additional information.

### Special thanks

A special thanks goes out to the friendly folk of Unity's forums for helping me out in getting this project started.

## License
[MIT](https://choosealicense.com/licenses/mit/)