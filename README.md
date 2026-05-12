# 3dsMax-ImagePlane

A MAXScript tool for **3ds Max** that attaches a camera-aligned image plane as a modifier — similar to Maya's image plane workflow.

The plane is frustum-correct (sized to match the camera's field of view at a given distance), lives on a dedicated frozen layer, and is non-renderable by default.

---

## Features

- **Modifier-based** — the image plane is stored as a modifier on the camera, so it travels with it and saves with the scene
- **Frustum-correct sizing** — calculated from FOV and distance (Maya-style), not guessed
- **Broad camera support** — Target Camera, Free Camera, VRay Physical Camera (with or without target node), and any camera that exposes a focus or target distance property
- **Lock Optical Size** — keeps the apparent screen size constant as you move the plane closer or farther. X/Y offsets are also scaled proportionally so the plane holds its screen position
- **Alpha-aware materials** — automatically sets up a Mix map for opacity when the image has an alpha channel
- **Visibility toggle** — synced correctly every time you reopen the modifier panel
- **Reset Distance** — snaps the plane back to the camera's actual target distance
- **Recalculate** — resizes the plane geometry to match the current FOV at the current distance (resets scale to 100%)
- **Reset Offsets X/Y** — zeroes out horizontal and vertical offsets in one click
- **Export to Scene** — clones the plane to the active layer as a fully independent, unfrozen, renderable object. The original stays linked to the modifier; the exported copy has no relation to it and won't be deleted if the modifier is removed

---

## Installation

**Option A — Drag and drop**
Drag `Pipe3D_ImagePlane.ms` directly onto the 3ds Max viewport. The dialog opens automatically.

**Option B — Run Script**
Go to `MAXScript > Run Script`, navigate to the file and open it.

> The script does not auto-run on startup. To have it launch automatically, add it to your startup scripts folder or create a toolbar button using `MacroScript`.

---

## Usage

### Creating an image plane

1. Select a camera in the scene
2. In the **3dsMax-ImagePlane** floating dialog, click **CREATE IMAGE PLANE**
3. Go to the **Modify** panel — you will see the **Image Plane Controls** rollout
4. Click **SELECT IMAGE...** and pick a bitmap
5. The plane is created, parented to the camera, placed at the target distance, and sized to fill the frustum

### Controls

| Control | Description |
|---|---|
| **Visible** | Show or hide the image plane in the viewport |
| **SELECT IMAGE...** | Pick or replace the bitmap |
| **Distance** | Move the plane along the camera's forward axis |
| **RESET DISTANCE** | Snap back to the camera's actual target distance |
| **RECALCULATE** | Resize geometry to match current FOV at current distance |
| **RESET OFFSETS X/Y** | Zero out horizontal and vertical offsets |
| **Lock Optical Size** | Scale and offset automatically when distance changes |
| **Scale (%)** | Uniform scale relative to the frustum-correct size |
| **Offset X / Y** | Shift the plane horizontally or vertically in camera space |
| **Opacity (%)** | Fade the image plane |
| **EXPORT TO SCENE** | Clone to active layer as a free, renderable object |

### Deleting an image plane

Select the camera, then click **DELETE IMAGE PLANE** in the floating dialog. This removes the modifier and deletes the plane. If the `ImagePlanes` layer is now empty it is also removed.

---

## How Lock Optical Size works

When enabled, changing the **Distance** applies a linear perspective ratio to both the scale and the X/Y offsets:

```
ratio    = newDistance / oldDistance
newScale = oldScale * ratio
newOffsetX = oldOffsetX * ratio
newOffsetY = oldOffsetY * ratio
```

This mirrors how perspective projection works: an object twice as far needs twice the world-space offset to sit at the same screen position. All writes are batched with `disableSceneRedraw` so the viewport updates once per tick with no tearing.

---

## Compatibility

Tested on **3ds Max 2022 – 2026**.  
Should work on any version that supports `custAttributes`, `NodeTransformMonitor`, and `EmptyModifier`.

---

## License

MIT — see [LICENSE](LICENSE)
