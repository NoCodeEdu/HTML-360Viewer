# HORIZON — 360° Photo Viewer

A standalone, single-file 360° panorama viewer built with Three.js. No server, no install, no dependencies to manage — just open `360viewer.html` in a browser.

---

## Features

- **Drag & drop loading** — drop any equirectangular image directly onto the viewer
- **Full pan / tilt / zoom** — click-drag to look around, scroll to zoom, arrow keys to nudge
- **Momentum** — panning glides naturally after release
- **Auto-rotate** — configurable speed slider in settings
- **Touch support** — works on mobile and tablet browsers
- **Fullscreen** — press `F` or use the button

### Camera & Image Support

- DJI drones (Mavic, Mini, Air series) — primary target
- Insta360, GoPro Max, and any other equirectangular 2:1 panorama
- JPG, PNG, WEBP
- Images up to 18K+ — resolution is capped by your GPU's actual texture limit (queried at runtime), not a hard-coded value
- EXIF orientation correction (handles rotated exports automatically)

### HUD & Settings

- Live azimuth, elevation, and FOV readouts
- Animated compass (left side, clears the bottom bar)
- Crosshair reticle
- Per-element visibility toggles (top bar, file info strip, bottom controls, compass, crosshair)
- Vignette intensity slider
- Film grain slider
- Auto-pan speed slider
- All settings persist to `localStorage` — restored automatically on next open
- GPU info panel showing renderer name, vendor, max texture size, and an iGPU warning if Chrome appears to be using integrated graphics

---

## Usage

1. Download `360viewer.html`
2. Open it in Chrome or Firefox (no web server needed — `file://` works fine)
3. Drop a 360° photo onto the viewer, or click **Browse Files**

That's it.

---

## Keyboard Shortcuts

| Key | Action |
|-----|--------|
| `H` | Toggle HUD on/off |
| `R` | Reset view to default |
| `F` | Toggle fullscreen |
| `+` / `−` | Zoom in / out |
| `↑ ↓ ← →` | Pan |

---

## Getting Full Resolution on High-End GPUs

Chrome sometimes defaults to the integrated GPU, capping texture size at 8192px instead of the 16384px+ available on a discrete card. The GPU info panel in Settings will show what the browser is actually using.

**To force Chrome to use your dedicated GPU:**

**Windows**
1. Open *Settings → System → Display → Graphics settings*
2. Browse to `chrome.exe` and set it to **High performance**
3. Restart Chrome

**Chrome flags (any OS)**
1. Go to `chrome://flags`
2. Enable **Override software rendering list**
3. Relaunch Chrome

After switching, reopen the viewer and check the GPU panel — the renderer string should show your discrete card (e.g. `ANGLE (NVIDIA GeForce RTX ... )`) and the texture cap will increase accordingly.

---

## Technical Notes

- **Renderer** — Three.js r128, `MeshBasicMaterial` on an inverted `SphereGeometry`
- **Texture cap** — queried via `gl.getParameter(gl.MAX_TEXTURE_SIZE)` at runtime; 90% of the reported value is used as the working cap
- **Decoding** — `createImageBitmap()` for off-main-thread JPEG decoding; falls back to `<img>` on older browsers
- **GPU detection** — `WEBGL_debug_renderer_info` extension; iGPU heuristic based on renderer string keywords
- **Settings storage** — `localStorage` key `horizon360_settings`
- **No external dependencies at runtime** — Three.js is loaded from cdnjs; Google Fonts loads `Bebas Neue`, `DM Mono`, and `DM Sans`; everything else is vanilla JS

---

## File Structure

```
360viewer.html   ← entire application, self-contained
README.md        ← this file
```

---

## License

Free for personal and non-commercial use under the
PolyForm Noncommercial 1.0.0 licence.
Commercial use requires a separate written agreement.
Copyright © nocodeedu
