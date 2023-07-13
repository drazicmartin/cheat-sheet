# Blender Python API

 - [Tips](#tips)
 - [Panel](#panel)
 - [Operators](#operator)

## Blender Add-ons

<a name="tips"/>

## Tips

Local to World

```python
obj.matrix_world @ mathutils.Vector3
```

### Interaction with `bpy`

bpy :
 - `app` : Application Data
   - This module contains application values that remain unchanged during runtime.
 - `context` : Context Access
   - The context members available depend on the area of Blender which is currently being accessed.
   - Note that all context values are readonly, but may be modified through the data API or by running operators
 - `data` : Data Access
   - This module is used for all Blender/Python access.
 - `msgbus` : Message Bus
 - `ops` : Operators
   - Provides python access to calling operators, this includes operators written in C, Python or macros.
   - Calling an operator in the wrong context will raise a RuntimeError, there is a poll() method to avoid this problem.
   - Operators don’t have return values as you might expect, instead they return a set() which is made up of: {'RUNNING_MODAL', 'CANCELLED', 'FINISHED', 'PASS_THROUGH'}. Common return values are {'FINISHED'} and {'CANCELLED'}, the latter meaning that the operator execution was aborted without making any changes or saving an undo history entry.
 - `path` : Path Utilities
 - `props` : Property Definitions
   - This module defines properties to extend Blender’s internal data. The result of these functions is used to assign properties to classes registered with Blender and can’t be used directly.
 - `types` : Types
 - `utils` : Utilities


### Standalone install
```bash
pip install bpy
``

- Setup workflow for VScode

- https://polynook.com/learn/set-up-blender-addon-development-environment-in-windows

### Main

```python
def register():
bpy.utils.register_class(DeleteAllObjectsOperator)
bpy.utils.register_class(SynthKPTPanel)

def unregister():
bpy.utils.unregister_class(DeleteAllObjectsOperator)
bpy.utils.unregister_class(SynthKPTPanel)

if __name__ == "__main__":
register()
```

<a name="panel"/>

### Panel

Create a new panel named : `SynthKPT`

```python
class SynthKPTPanel(bpy.types.Panel):
bl_idname = 'synth_kpy_panel'
bl_space_type = 'VIEW_3D'
bl_region_type = 'UI'
bl_category = 'SynthKPT'
bl_label = 'SynthKPT'

def draw(self, context):
layout = self.layout

## show operator
layout.operator(
operator='object.debug_camera'
)

## display text
layout.label(text="text")

## separator
layout.separator()

## box layout with row
box = layout.box()
row = box.row()
row.prop(scene, "frame_start")
row.prop(scene, "frame_end")
```

<a name="operator"/>

### Operator

Create Operator

```python
class DebugCameraOperator(bpy.types.Operator):
bl_idname = "object.debug_camera"
bl_label = "Debug camera"

def execute(self, context):
SceneManager.vis_camera_positions_possibilities()
return {'FINISHED'}
```

### Scene Properties

import
```python
from bpy.types import PropertyGroup
from bpy.props import IntProperty, PointerProperty, BoolProperty, StringProperty
```

Create
```python
class MyProperties(PropertyGroup):

min_distance_camera: IntProperty(
name="Camera Min Distance",
description=":",
default=3,
min=0,
max=100
)
```

Register
```python
bpy.utils.register_class(MyProperties)
# Store properties under WindowManager (not Scene) so that they are not saved in .blend files and always show default values after loading
bpy.types.WindowManager.gkpt_tool = PointerProperty(type=MyProperties)
```

Show in panel
```python
layout.prop(context.window_manager.gkpt_tool, "min_distance_camera")
```

Use in code
```python
context.window_manager.gkpt_tool.min_distance_camera
```

### Hook frame

Register

```python
bpy.app.handlers.frame_change_post.append(handler)
bpy.app.handlers.frame_change_post.clear()

bpy.app.handlers.frame_change_pre.append(handler)
bpy.app.handlers.frame_change_pre.clear()
```

Definition
```python
def frame_handler(scene, depsgraph):
    print(scene)
    print(depsgraph)
```

### Deleting object

```python
def delete_objects_in_collection(collection_name):
    collection = bpy.data.collections.get(collection_name)
    if collection:
        bpy.ops.object.select_all(action='DESELECT')
    for obj in collection.objects:
        # Remove the mesh data
        if obj.type == 'MESH':
            bpy.data.meshes.remove(obj.data)
        obj.select_set(True)
    bpy.ops.object.delete()
```

### Render Image

Camera view with shading
```python
# Set output folder
bpy.context.scene.render.image_settings.file_format = 'PNG'

# Set up animation range
start_frame = bpy.context.scene.frame_start
end_frame = bpy.context.scene.frame_end

# Render each frame
for frame in range(start_frame, end_frame + 1):
    # Set current frame and update viewport
    bpy.context.scene.frame_set(frame)
    bpy.context.scene.render.filepath = str(Path(output_folder) / f"image_{frame}.png")

    # Render the frame
    bpy.ops.render.render(animation=False, write_still=True)
```

Viewport view
```python
# Render each frame using Viewport Render Animation
bpy.ops.render.opengl(animation=True, sequencer=False, write_still=True)
```

