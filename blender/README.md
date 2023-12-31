# Blender Python API

 - [Tips](#tips)
 - [Panel](#panel)
 - [Operators](#operator)
 - [Scene Properties](#sp)
 - [Hook Frame](#hf)
 - [Usefull Functions](#uf)
   - [Render Image](#ri)
   - [Find Objects by type](#fobt)
   - [Delete objects](#do)
   - [Create mesh from scratch](#cmfs)
- Mesh
 - [UV Map](#uv)
---

<a name="tips"/>

## Tips

Local to World

```python
obj.matrix_world @ mathutils.Vector3
```

Get Active element and Selected elements
```python
selected = bpy.context.selected_objects            # -> <class 'list'>
selected = bpy.context.view_layer.objects.selected # -> <class 'bpy_prop_collection'>

active = bpy.context.active_object                 # -> <class 'bpy_types.Object'>
active = bpy.context.view_layer.objects.active     # -> <class 'bpy_types.Object'>
```

Set selected specific objects
```python
# Get the active scene
scene = bpy.context.scene

# Deselect all objects
bpy.ops.object.select_all(action='DESELECT')

# Set the object by name as selected
selected_object_name = "Cube"  # Replace with the name of your desired object
selected_object = scene.objects[selected_object_name]
selected_object.select_set(True)
```

Update all dependencies of previous transformations
```python
bpy.context.view_layer.update()
```

Rotation
```python
Apply Quaternion rotation on vector
quaternion @ vector

Apply rotation
`Vector | Quaternion | Euler | Matrix`.rotate(`Quaternion | Euler | Matrix`)
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
```

- Setup workflow for VScode

- https://polynook.com/learn/set-up-blender-addon-development-environment-in-windows

## Main

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

## Panel

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

## Operator

Create Operator

```python
class DebugCameraOperator(bpy.types.Operator):
bl_idname = "object.debug_camera"
bl_label = "Debug camera"

def execute(self, context):
  SceneManager.vis_camera_positions_possibilities()
  return {'FINISHED'}
```

<a name="sp"/>

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

<a name="hf"/>

## Hook frame

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

<a name="uf"/>

## Usefull Functions

<a name="do"/>

### Delete objects

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

<a name="ri"/>

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

<a name="fobt"/>

### Find objects by type

```python
def find_objects_by_type(type_name):
    objects = []
    for obj in bpy.context.scene.objects:
        if obj.type == type_name:
            objects.append(obj)
    return objects
```

<a name="cmfs"/>

### Create mesh from scratch

```python
new_mesh = bpy.data.meshes.new('new_mesh')
new_mesh.from_pydata(vertices, edges, faces)
new_mesh.update()
new_object = bpy.data.objects.new('new_object', new_mesh)
```


## Mesh

<a name="uv"/>

### UV map

```python
obj.type == "MESH"

# Create a new UV map
obj.data.uv_layers.new(name="UV_name", do_init=False)

# Get UV map of an objects
obj.data.uv_layers.get(<UV_NAME>).uv
```
