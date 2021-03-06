# Scene

[[toc]]

---

This menu lets you manage the different objects that you might have in Nomad.

## List
| Action       | Icon                                                           | Description  |
| :---:        | :---:                                                          | :---:        |
| Visible      | <img src='./images/common/show.png'      class='icon-image'/>  | Hide the object (or use the object as subtractive mesh by the [Voxel Merger](#subtraction)) |
| Edit Name    | <img src='./images/common/pencil.png'    class='icon-image'/>  | Edit the object |
| Delete       | <img src='./images/common/trash.png'     class='icon-image'/>  | Delete the object |
| Move         | <img src='./images/common/move.png'      class='icon-image'/>  | Move the object in the list, it doesn't have any effect on the viewport |
| Duplicate    | <img src='./images/common/duplicate.png' class='icon-image'/>  | Duplicate the object |

## Multiselection
You can select multiple object, it can help you achieve two things:
- using the gizmo tool to move several objects at once
- merging the objects thanks to the [Simple Merge](#simple-merge) or [Voxel Merge](#voxel-merge) feature

You can do that by using the `Multiselection` checkbox, and then by clicking on the object in list.

::: tip Quick multiselection
You can also multiselect in the viewport by holding the `Smooth` shortcut and tapping on another mesh.

You can unselect an object by tapping on it again (only if your selection has more than one object).
:::

::: warning Limited gizmo feature
When using multiselection, the gizmo tool will always ignore masking.
Also, X/Y/Z scaling is removed.

The reason is that multiselection only allow whole mesh transformation, not per vertex transformation.
This might be improved in the future.
:::



## Simple Merge
This option will simply make one single object entry from multiple selected objects.

You can see an example in video in the [Separate](#separate) section.


## Voxel Merge
Voxel Merge works differently than [Simple merging](#simple-merge) in that the topology is completely recomputed.
More importantly, there won't be any separation between the two meshes, so the potential intersections between the two meshes will be gone.

When using the voxel merger you can change the resolution of the voxel by going in the [Topology](topology.md) menu.

<video width='360' preload='metadata' poster='./videos/merge_add.jpg' controls>
    <source src='./videos/merge_add.mp4' type='video/mp4'>
</video>


### Subtraction
You can use the remesher to perform boolean operations, for example by subtracting one mesh from another,
The objects that are hidden will be subtracted, they will appear with a crosshatching look.

<video width='360' preload='metadata' poster='./videos/merge_sub.jpg' controls>
    <source src='./videos/merge_sub.mp4' type='video/mp4'>
</video>


### Intersection
If you use the voxel remesher with all the objects hidden, the boolean intersection will be computed.

<video width='360' preload='metadata' poster='./videos/merge_inter.jpg' controls>
    <source src='./videos/merge_inter.mp4' type='video/mp4'>
</video>


## Primitive

If you need to add a new objects in your scene, you can add primitive.
You can configure the primitive with some parameters.

However, you need to `Validate` the primitive in order to sculpt or paint it.
When you `Validate` a primitive you won't be able to edit the primitives settings anymore.

| Primitive   | Icon                                                            | Description |
| :---:       | :---:                                                           |:---:|
| Box         | <img src='./images/common/cube.png'        class='icon-image'/> | It's a simple cube, you can control the division in X, Y and Z |
| Sphere      | <img src='./images/common/sphere.png'      class='icon-image'/> | For convenience this is named a Sphere but it is actually a subdivided box, with the `Project on sphere` forced |
| Cylinder    | <img src='./images/common/cylinder.png'    class='icon-image'/> | You can add a center hole for the cylinder primitive, for example to make an hollow pipe |
| Torus       | <img src='./images/common/torus.png'       class='icon-image'/> | The torus can be a good starting point for rings |
| Cone        | <img src='./images/common/cone.png'        class='icon-image'/> | - |
| Icosahedron | <img src='./images/common/icosahedron.png' class='icon-image'/> | - |
| UV-sphere   | <img src='./images/common/sphere.png'      class='icon-image'/> | You should probably not use this primtive, see [Warning below](#sphere-warning) |
| Plane       | <img src='./images/common/rectangle.png'   class='icon-image'/> | It's a simple plane, note that this is the only primitive that is not closed |
| Triplanar   | <img src='./images/common/sphere.png'      class='icon-image'/> | A special primitive, see [Triplanar](triplanar) |

::: tip
If you wonder what is the base mesh when you launch Nomad: this is a subdivided box as well.
However the base mesh in Nomad doesn't use `Project on sphere`, meaning it is not perfectly round.
:::

### Primitive Config
Here are a some options that you can find in some primitive:
- `Flat subdivision` Preserve hard-edges by not smoothing the mesh when subdividing
- `Overall subdivision` The number of subdivision
- `Constant density` It means that the topology of the primitive will be kept uniform when you play with the settings
- `Project on sphere` To get a perfect geometrical sphere but keeping the underlying primitive topology


### UV Sphere
::: warning
You should probably never use it, the topology of the UV Sphere is not well suited for sculpting, especially on the poles.

Please prefer the [Sphere](#sphere), [Box](#box) or [Icosahedron](#icosahedron) primitive, along with the `Project on sphere` option.

Note that the topology can be acceptable for sculpting if you use a very low value for the `Division` sliders.
You can then use the `Overall Subdivision` slider to raise the number of polygons.
:::


### Triplanar
This primitive is special in that you should use the [Masking tool](tools.md#mask) on it to shape the geometry.

<video width='360' preload='metadata' poster='./videos/triplanar.jpg' controls>
    <source src='./videos/triplanar.mp4' type='video/mp4'>
</video>

::: tip
Double-tap on a plane and the camera will focus on this particular plane.
This won't work if you rotate the primitive with the gizmo though.
:::

Triplanar is using the mask information from 3 planes to fill a voxel grid that is then polygonized (thanks to the [Voxel Remesher](topology.md#voxel-remeshere)).

Each plane has its own symmetry plane.

::: warning
Each time you update the size of the Triplanar primitive, the quality of the mask painting will degrade.

For now there is no option to 'lock' the painting on a single plane, but maybe it will come in the future.
You can use the [Connected Topology](stroke.md#connected-topology) to help a little bit, in that if your cursor lie precisely on one plane it won't impact the the other planes.
:::
