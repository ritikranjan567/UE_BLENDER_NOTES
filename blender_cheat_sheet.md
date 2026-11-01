# 🎨 Blender Cheat Sheet
> **Quick reference for Blender shortcuts and modeling tools**

---

## 🧭 Navigation & Visibility

| Shortcut / Action | What it does |
|---|---|
| `M` + mouse | Rotate view |
| `Shift` + `M` + mouse | Pan view |
| Mouse wheel | Zoom in / out |
| `5` | Toggle orthographic / perspective view |
| `1` | Front orthographic view |
| `3` | Right view |
| `7` | Top orthographic view |
| `Ctrl` + `1 / 3 / 7` | Opposite-side orthographic view |
| `9` | Toggle to the opposite orthographic view (e.g. front ↔ back) |
| `H` | Hide selected object |
| `Alt` + `H` | Unhide all |
| Outliner | Unhide objects selectively |
| `Shift` + `C` | Return the 3D cursor to the origin and reset the view |
| `Shift` + right-click | Move the 3D cursor to the clicked position |

## 🛠️ General Shortcuts

| Shortcut | What it does |
|---|---|
| `Ctrl` + `Shift` + `Z` | Redo |
| `T` | Toggle the toolbar |
| `Shift` + `A` | Add objects to the scene |
| `Shift` + `D` | Duplicate the object |
| `Shift` + `D`, then `X / Y / Z` | Duplicate and move along an axis |
| `Ctrl` + `J` | Join selected objects into one |
| `Shift` + left-click | Make an object the active selection |
| `Tab` | Toggle Object / Edit Mode |
| `Shift` + `Tab` | Toggle grid snapping |
| Enable Gizmos | Show gizmos from the top toolbar |
| Scene Properties → Units | View or change scene units and scale |

**Joining objects (`Ctrl` + `J`)**
- Select the objects to join.
- The last-selected object becomes the active object; the others are joined into it and removed from the object list.

## 🔄 Transforming Objects

| Shortcut | What it does |
|---|---|
| `G` | Grab / move the geometry or mesh with the mouse |
| `R` | Rotate |
| `S` | Scale |
| `Alt` + `G / S / R` | Reset location / scale / rotation |
| `G / R / S`, then `X / Y / Z` | Transform along the chosen axis |
| `G / R / S`, then `Shift` + `X / Y / Z` | Lock that axis and transform along the other two axes |
| After a transform shortcut, type a number | Set the transform amount (e.g. `S`, then `0.5` scales to `0.5`) |
| After a transform shortcut, specify an axis and value | Transform along that axis (e.g. `R`, `X`, `90` rotates 90° around X) |
| `G / R / S`, axis, then `2` | Use the local axis reference (e.g. `G` → `X` → `X`) |

- After `G` / `R` / `S`, **left-click** to confirm the position; **right-click** to cancel the movement and return to the initial position.
- By default, transforms use global axes. For example, scaling a cube rotated 45° may not turn it into a rectangle along the direction you expect.
- To transform using local axes, press the axis key a second time (e.g. `G` → `X` → `X`).

## 🎨 Viewport Shading

| Shortcut | What it does |
|---|---|
| Hold `Z` | Open shading mode pie menu |
| `Z`, then `4` | Wireframe shading |
| `Z`, then `6` | Solid shading |
| `Z`, then `2` | Material Preview (default material appears solid white) |
| `Z`, then `8` | Rendered mode, with lights, shadows, and effects |
| Right-click → Shade | Switch between smooth and flat shading |

- You can shade only selected faces smooth or flat to shade parts of an object differently.

## 🧱 Object & Cursor Placement

- The **3D cursor** determines where a newly added object will appear.
- To move the object's origin to another position:
  1. Press `Tab` to enter Edit Mode.
  2. Select all vertices and move the geometry away from the center.
  3. Select the desired position and move the 3D cursor there with `Shift` + `S`.
  4. Right-click the moved 3D origin and choose **Origin to 3D Cursor**.

## 🧩 Edit Mode & Selection

| Shortcut / Action | What it does |
|---|---|
| `Tab` | Enter / exit Edit Mode |
| `Alt` + left-click | Select a continuous loop |
| `Shift` + `Alt` + left-click | Select multiple loops |
| `Ctrl` + click another face / vertex / edge | Select the loop between the selected elements using the shortest path |
| `O` | Toggle proportional editing |
| Mouse wheel | Increase / decrease the proportional-edit influence ring |
| Delete | Delete selected vertices, edges, or faces; choose the deletion type |

- A face-loop selector can select only quadrilateral faces.
- The **Modeling** workspace has more modeling tools in the toolbar.

## 🧰 Modeling Tools

### Extrude — `E`

- Select a face, press `E`, and move it to extend the face and create additional geometry along adjacent faces.
- `E`, then `X / Y / Z` — extrude along the chosen axis.
- Extrusion uses the local Z axis by default; press `Z` again to switch to the global axis.

> ⚠️ **Warning:** If you cancel the extrusion movement with right-click, the extrusion itself still exists. Try moving the selected face with `G` to see the extra geometry. Duplicate geometry can cause problems later.

**Fixing unwanted duplicate geometry**
- Press `M` to open the Merge menu and choose a merge type.
- Recommended: select all vertices and use **Merge by Distance**.

### Fill, Join & Dissolve

| Shortcut / Action | What it does |
|---|---|
| `F` | Fill deleted faces, edges, or vertices |
| `J` | Join selected vertices / edges; cuts or divides a face / edge when the new edge lies between them |
| Delete → Dissolve | Remove the selection and merge adjacent geometry |

- If Blender cannot dissolve the selection, it defaults to deletion.

### Loop Cuts & Subdivision

| Shortcut / Action | What it does |
|---|---|
| `Ctrl` + `R` | Add a loop cut in Edit Mode |
| `Ctrl` + `R`, then mouse wheel | Increase / decrease the number of cuts |
| `Alt` + select loop cut → Delete | Delete the loop cut |
| Right-click → Subdivide | Subdivide selected faces so they are divided equally |

- After `Ctrl` + `R`, **left-click** to place the cut at a custom position, or **right-click** to place it halfway.
- After creating a loop cut, use the **Loop Cut** panel to adjust its settings.
- To add edge loops more quickly, select faces and choose **Subdivide** from the right-click menu.

### Fill Action

Select the edget and use `f` to fill with faces

**Bridge Edge Loop**: Select edge or edges you wanna feel, then `ctrl + e` -> Bridget Edget Loops

### Bavel

Used to smooth edgets/round edges.  
Select edge/edges then `ctrl + b` to round it, introducing new edge.
Use mouse scroll to add more segments or left bottom Bavel menu.

### Inset

Press `I`

Used to create a inner face, like if circle then inner circle, square then inner square.  
This can be done using extrude but, if you wanna create inner faces in multiple faces at once, you have extrude one by one.
Use inset after selecting face press `I`
