# Source Engine Collision Tools
Blender (3.x to 5.x) addon for generating and optimizing collision models for use in Source Engine games (ie. TF2, GMod, L4D2). Works best when combined with the [Blender Source Tools](http://steamreview.org/BlenderSourceTools/).

This addon can be useful for generating collision for other game engines too. However, only support for Source is specifically offered and tested.

Finding this addon useful? Please consider starring it ⭐, [leaving a review](https://extensions.blender.org/add-ons/sourceenginecollisiontools/reviews/new/) on the Blender repository, or [donating](https://ko-fi.com/theanine3d) 🙂<br>

Need help with the addon? You can join [my Discord server](https://discord.gg/43ggeGC8A8) and reach out for help there.

## Features
Some features are customizeable and can be tweaked via optional settings. There is also a "Recommended Settings" button that will automatically guess the best settings for you based on the currently selected, active object.
Note that all of the "Generate" features support the Decimate Ratio setting, to automatically reduce the complexity of the resulting collision model.
- **Generate Collision via Bisection** - Generate a collision model for every currently selected object, by evenly dividing the model into sections.
- **Generate Collision from Weights** - Generates a collision mesh for the currently selected objects, based on each object's rigged weights (if they exist). Can be used to quickly create ragdoll physics meshes for Garry's Mod
- **Generate Collision from Faces** - Generate a collision model for every currently selected object, based on the mesh's faces/polygons.
- **Generate Collision via UV Map** - Generates a collision mesh for the currently selected objects, based on each object's UV Map. Each UV island becomes a separate hull.
- **Generate Collision via Fracture** - This operator uses the Cell Fracture addon built into Blender to generate collision. Best used on individual props, not entire scenes at once. Works best on fully sealed objects with no holes or non-manifold geometry.
  - Attempts to generate only the amount of hulls specified by the "Fracture Target" setting. ie. A "Fracture Target" of 4 will try to split up the model into only 4 parts.
  - Note that the Cell Fracture addon needs to be enabled in your Blender preferences first!
- **Split Up Collision Mesh** - Splits up selected collision models into multiple separate objects, with every part having no more than 32 hulls. (The '32' amount can be customized, if needed)
- **Merge Adjacent Similars** - Merges convex hulls with similar adjacent hulls aggressively, lowering the final amount of hulls & producing a (potentially) less accurate, but more performant model. Similarity is based on the face count and volume of the hulls.
- **Remove/Merge/Select Thin Hulls** - Thinness tools that can quickly isolate, merge, or remove any convex hulls that are significantly smaller than all other hulls.
- **Force Convex** - Forces all existing hulls in every selected collision model to be convex. Especially useful after using Blender's built-in Decimate modifier on an existing collision mesh, to ensure that any decimated hulls are still convex.
- **Find/Remove Inside Hulls** - Finds and (optionally) removes any hulls that are completely or almost completely buried inside other hulls.
- **Generate Source Engine QC** - Automatically generate QC files for one or more collision model(s), allowing you to quickly compile them with batch compile tools out there (ie. [Crowbar](https://developer.valvesoftware.com/wiki/Crowbar))
  - Supports adding custom QC commands via a QC Override system. This allows you to, for example, add a custom "$scale" or "$surfaceprop" command to all generated QC files.
- **Generate Ragdoll QCI** - Generates a ragdoll .QCI file for every *rigged* collision mesh, with $collisionjoint lines automatically generated based on your armature's specific bones. You can use the $include command in your main QC file to load the .QCI file.
- **Update VMF** - Updates a selected VMF file by automatically adding any partitioned/split-up collision models that haven't already been added to the map.
- **Clean Duplicate Collision** - Removes any duplicate collision entries ("_phys_part_") found in the user's specified VMF file.
- **Export Hulls to Hammer .VMF** - Converts hulls in the selected collision mesh(es) into Source Engine brush solids and exports them as a .VMF file that can be opened in Hammer.

## Installation
- If you are using Blender 4.2 or higher, you can install the addon via Blender's official [online extension repository](https://extensions.blender.org/add-ons/sourceenginecollisiontools/), which can also be accessed via Blender's Preferences.
- If you are using Blender 4.1 or lower, you can get the newest, bleeding edge version of the addon by pressing the big green "Code" button above, and choose "Download ZIP"
- After downloading either of the above, go into Blender's addon preferences (File → Preferences → Addons)
  - Click the "Install..." button and browse to the ZIP file you just downloaded, select it, and press "Install Add-on"

## Tips
- The UI for the addon is found in the "Object Properties" tab on the right-hand side of the Blender window.
- Decimation (by the Generate Collision Mesh operator) generally makes the Merge Adjacent Similars operator less effective, but Decimation is much faster at reducing the final complexity of the collision mesh. However, Decimation will reduce the complexity uniformly across the entire model, whereas Merge Adjacent Similars tries to reduce complexity only where similar hulls are found.
- Merge Adjacent Similars is less effective on overly large, complex models. For best results, split up a large complex model into several (3-5) separate pieces first, and then use Merge Adjacent Similars on each individual piece.
- For the "Update VMF" feature, the very first part of your split-up collision mesh, ending in 'part_000.mdl', must already be in the VMF somewhere (such as in a prop_static). The operator scans the VMF for any part_000, and any that are found are used as a template for all the other parts. So you'll need to add that first part manually in Hammer first.
- Source Engine has a limit of 32 hulls per collision mesh. Going beyond 32 hulls can lead to severe lag during gameplay. The "Split Up Collision Mesh" and "Update VMF" features are handy for this. By splitting up your collision mesh into 32-hull parts, you prevent lag, and the Update VMF feature can add numerous (even dozens or hundreds) of parts automatically for you to a VMF file.
- To utilize the QC override system, add a string-based Custom Property (in the bottom of the Object Properties panel) to one split-up collision object in your Blender scene, with the property name starting with "$". Enter the value/parameters for this property. Then press the Copy QC Overrides button to copy this override to all other selected collision parts. These will be included in the resulting generated QC files.
  - ![image](https://github.com/theanine3D/source-engine-collision-tools/assets/88953117/ca659755-e483-4866-871c-a9fc3848898d)
- If you're using this addon for Unreal Engine, there is a hidden feature called "_Rename Collision for Unreal_" which  you can find with Blender's search button (F3 or spacebar). This operator will instantly rename all of the collision meshes in your scene so that they conform to Unreal Engine's collision naming scheme (with the UCX_ prefix).

## Previews ##
### Interface ###
![image](https://github.com/user-attachments/assets/cc9d4aad-07d8-46cd-b445-739a42e1e504)
### Generate Collision from Weights ###
<img width="1055" height="740" alt="image" src="https://github.com/user-attachments/assets/06b35a58-e42d-403c-8353-1d8ea29b9c87" />

### Generate Collision by Bisection ###
![generate-by-bisection](https://github.com/user-attachments/assets/598a1280-7f10-4b24-ab31-533ad00c6d88)
### Generate Collision by Fracture ###
![Untitled](https://user-images.githubusercontent.com/88953117/231557347-ce472d26-0634-4db9-a18f-0d1e7891a019.gif)
### Generate Collision from Faces
![collision-gen-1](https://user-images.githubusercontent.com/88953117/212523161-07296101-d80f-4d7e-8cbe-5ccbc93425ba.gif)
### Merge Adjacent Similars ###
![merge-similars](https://user-images.githubusercontent.com/88953117/213289714-d13d5bb8-ef37-439e-8eac-1370b4716bab.gif)
### Remove Thin Hulls
![remove-thin-hulls](https://user-images.githubusercontent.com/88953117/216437113-22036e00-dcbe-4e74-a6c9-388fb96ac173.gif)

### Videos
Click the image below to watch a breakdown video of most of the features in the addon.

[![YouTube Video](https://user-images.githubusercontent.com/88953117/219478247-5763224f-5bb2-443d-81ee-b17532cbb7c4.png)](https://www.youtube.com/watch?v=ASLw-FMQUXM)

This video below shows some newer features that have been added since the previous video.

[![YouTube Video](https://github.com/user-attachments/assets/7244d17f-90cb-40fc-9117-e9aa341ea8d5)](https://youtu.be/yWF5ngntf5A)

An overview of the rigged collision feature in the addon.

[![YouTube Video](https://youtu.be/XA83EQjGdx0)](https://youtu.be/XA83EQjGdx0)

### Gallery
The maps below were created using Source Engine Collision Tools. They are great examples of what this addon makes possible.
- #### Saturn Valley - TF2 map by Theanine3D
  - [![YouTube Video](https://github.com/user-attachments/assets/28a24da2-f792-4ab3-abb9-473d3a75bfc0)](https://www.youtube.com/watch?v=jbJhX8MaSDQ)

- #### Destiny Islands - Garry's Mod map by Theanine3D
  - [![YouTube Video](https://github.com/user-attachments/assets/57a5d834-cf59-41b4-ba26-dbfc92e0772a)](https://www.youtube.com/watch?v=bWywVC6aR9M)

