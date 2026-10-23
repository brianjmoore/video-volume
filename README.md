# Video volume

Renders a time range of a video as a 3D volume in the browser. Width and
height come from the frame, and time runs along the third axis. You can view
it as a cloud of semitransparent voxels or cut through it with a plane to see
a slice. Works on mobile browsers.

Everything is in `index.html`: plain WebGL2, no libraries, no build step.

## Run

Serve the folder and open it in a browser:

```sh
python3 -m http.server 8000 --bind 0.0.0.0
```

Open `http://localhost:8000/`, or `http://<your-computer's-IP>:8000/` on a
phone on the same Wi‑Fi. A demo volume loads on start.

## Use

1. **Open video**, scrub the preview, and tap **Set start** / **Set end**.
2. Choose **Frames** (samples along time) and **Width** (pixels per frame),
   then tap **Build volume**. The memory estimate turns red if the volume is
   too large for the device.
3. Drag to orbit and pinch or scroll to zoom. Click an end of the axis gizmo
   (top right) to look along X, Y or T (time), or its centre to reset the view. On a keyboard, **1**, **3** and **7**
   give the front, side and top views (add **Ctrl** for the opposite side),
   **Home** resets the view and **N** toggles the properties panel.

### Layout

**2×2** in the top bar splits the view like a medical image viewer: the 3D
view plus three slice views through a shared crosshair. **Frame (XY)** shows
one frame, **Row (XT)** one row of pixels over time (time runs down), and
**Column (YT)** one column over time (time runs right). Each view and its
crosshair line take the colour of the axis the slice is normal to, the same
colours as the gizmo, and the 3D view outlines all three slices.

- Click or drag in a slice view to move the crosshair there. Scroll, or drag
  with two fingers on a touch screen, to step through slices one voxel at a
  time.
- The rulers are in pixels and seconds.
- Double-click (or double-tap) any view to maximize it, and again to go back.
- With the 2×2 layout, the 3D view shows the colour and motion value of the
  voxel under the crosshair (**CUR**, **RGB**, **MOT**).

With **Time hue**, **Gray** or **Hot iron** colours, a legend shows the
colour scale.

The corners of the viewport show readouts like a medical image viewer: source
and voxel dimensions, the render settings (**W**/**L** are the opacity
softness and threshold), the plane's position (with the frame, row or column
index for axis-aligned planes), and the zoom and camera angles.

### Render

- **Volume** blends voxels front to back. **MIP** (maximum intensity
  projection) shows the strongest voxel along each line of sight.
- **Jello** turns the volume into a block of gelatin on a plate. The video
  content is suspended inside, seen through a refracting, glossy, tinted gel.
  Tap the block to poke it, spin the view to make it twist, or tap **Wobble**.
  The **Jello** section sets the **Gel** colour, **Wobbliness** (how long it
  keeps jiggling) and **Refraction**. Cutting with the plane slices the jello.
  **Video content: Dyed gel** makes every voxel a little see-through, like
  gelatin dyed with the video's colours: it tints the light passing through
  rather than blocking it. Pair it with **Uniform** opacity and **Clear** gel;
  **Density** sets how strong the dye is.
- **Opacity from** sets which voxels are visible:
  - **Motion**: how far a pixel differs from its median over the clip, so
    moving objects become tubes through time.
  - **Bright** or **Dark**: based on luminance.
  - **Uniform**: everything at the same opacity.
- **Colour map**: **Video** keeps the video's colours, **Time hue** colours
  voxels by time, and **Gray** and **Hot iron** map luminance through a
  grayscale or black–red–yellow–white ramp. The map applies to the slice face
  too.
- **Threshold**, **Softness** and **Density** shape opacity. **Time length**
  stretches the time axis. **Quality** and **Resolution** trade detail for
  speed.

### Plane

- **Cut volume** removes everything on one side of the plane and shows the
  slice face. **Slice only** shows just the slice, with an optional faint
  **Ghost vol.**
- The presets give a single frame (**XY**), one row over time (**XT**) or one
  column over time (**YT**). **Azimuth** and **Elevation** tilt the plane
  freely.
- **Sweep plane** animates the plane through the volume. Switching the top bar
  from **Orbit** to **Slice** makes dragging on the view move the plane instead
  of orbiting.

## Limitations

- Frames are captured by seeking the video once per frame, so building
  ~100 frames takes a few seconds.
- With a moving camera, nearly every pixel counts as motion. Use **Bright** or
  **Dark** opacity for that kind of footage.
- Supported video formats depend on the browser. iPhones handle H.264 and
  HEVC from the camera roll.
- Volume size is limited by the GPU's 3D texture limit. Volumes are also
  capped at 96 MB on touch devices and 256 MB elsewhere.
