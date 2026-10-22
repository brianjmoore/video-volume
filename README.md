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
3. Drag to orbit and pinch or scroll to zoom.

### Render

- **Volume** blends voxels front to back. **Max intensity** shows the
  strongest voxel along each line of sight.
- **Opacity from** sets which voxels are visible:
  - **Motion**: how far a pixel differs from its median over the clip, so
    moving objects become tubes through time.
  - **Bright** or **Dark**: based on luminance.
  - **Uniform**: everything at the same opacity.
- **Time hue** colours voxels by time instead of using the video's colours.
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
- **Sweep plane** animates the plane through the volume. **Drag: plane** in the
  top bar makes dragging on the view move the plane instead of orbiting.

## Limitations

- Frames are captured by seeking the video once per frame, so building
  ~100 frames takes a few seconds.
- With a moving camera, nearly every pixel counts as motion. Use **Bright** or
  **Dark** opacity for that kind of footage.
- Supported video formats depend on the browser. iPhones handle H.264 and
  HEVC from the camera roll.
- Volume size is limited by the GPU's 3D texture limit. Volumes are also
  capped at 96 MB on touch devices and 256 MB elsewhere.
