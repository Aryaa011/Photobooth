# SnapBooth 📸

A fully self-contained **3D Digital Photobooth** experience built with Three.js — no server, no build tools, no dependencies to install. Open one HTML file and it works.

---

## Live Demo
https://snapbooth01.netlify.app

---

## Features

### Exterior View
- Your photobooth GLB model loads with cinematic lighting — pink, cyan, and directional key light
- Neon ring glow at the base of the booth
- Floating particles and grid floor for atmosphere
- **Auto-rotates** when idle
- **Drag** to orbit, **scroll** to zoom, **pinch** to zoom on mobile
- Click the booth (or press **ENTER BOOTH**) to go inside

### 3D Interior
- Fully built in Three.js geometry — no second GLB file
- Authentic vintage photobooth design:
  - Large screen with live webcam feed
  - **EYE LEVEL** arrows
  - Camera lens with pulsing red LED
  - Instruction panel (4-step, canvas-drawn)
  - Small CRT preview monitor
  - Green start button, coin slot
  - Metal screws, wall trim, ceiling fluorescent strip
- **Graffiti walls** — back wall, left wall and right wall each have hand-drawn graffiti, doodles, film strips and spray paint art baked into canvas textures
- **Neon LED corner strips** — pink and cyan
- **Polaroid photos** pinned to the back wall with red pins
- **Drag to look around** — full 360° interior rotation on both mouse and touch

### Shooting
| Feature | Detail |
|---|---|
| Strip types | 1, 2, or 4 photos |
| Countdown | 3→2→1 with bounce animation and ascending tones |
| Flash | Full-screen white flash + shutter click sound |
| Filters | Normal · Golden (warm sepia) · B&W · Pink — real GLSL shaders on the live feed |

### Strip Output
- Composited on an offscreen `<canvas>`
- Border colour matches the active theme (pink / gold / green)
- Only the **date** is printed — no watermark
- **Download as PNG** with one tap

### Sticker Editor (Phase 4)
- 28 emoji stickers to choose from
- Tap a sticker to select → tap the strip to place it
- **Drag** placed stickers to reposition (mouse and touch)
- Download the final strip with stickers baked in

### Music
- Funky lo-fi beat generated entirely via **Web Audio API** — no audio files
- Bass line (sawtooth), chord pads (sine), hi-hats (noise buffer), kick drums
- 92 BPM, loops continuously
- Toggle with the 🎵 button, auto-mutes on exit

### Themes
| Theme | Lights | Strip border |
|---|---|---|
| Neon Tokyo | Pink key + cyan fill | `#ff2d78` |
| Golden Era | Amber key + warm fill | `#ffd700` |
| Forest Magic | Green key + soft fill | `#00cc55` |

---

## Mobile Responsive

- Canvas fills 100% viewport, pixel ratio capped at 2× for performance
- Bottom UI is a compact pill bar that never covers the webcam screen
- All touch events handled: drag, pinch-to-zoom, tap
- Safe area insets respected for notched phones (`env(safe-area-inset-bottom)`)
- Sticker editor and strip result scroll safely on small screens
- iOS audio unlocked on first touch

---

## Tech Stack

| Layer | Technology |
|---|---|
| 3D rendering | Three.js r128 |
| Model loading | GLTFLoader (Three.js) |
| Webcam | `getUserMedia` + `THREE.VideoTexture` |
| Filters | GLSL `ShaderMaterial` fragment shaders |
| Wall art | Canvas 2D textures baked into `THREE.CanvasTexture` |
| Strip compositing | Offscreen `<canvas>` + `toDataURL` |
| Audio | Web Audio API (oscillators + noise buffers) |
| Fonts | Google Fonts (Bebas Neue + DM Sans) |
| Build | None — single HTML file |

---

## File Structure

```
snapbooth_v4.html     ← entire app, self-contained (~9.4 MB)
README.md             ← this file
```

The GLB model is base64-encoded and embedded directly in the HTML so the file works from `file://` with no CORS issues and no internet required (except for Google Fonts on first load).

---

## How It Works

```
1. Page loads
   └── Three.js + GLTFLoader inlined as <script> tags
   └── GLB decoded from base64 → Blob URL → GLTFLoader

2. Exterior scene
   └── GLB model placed on reflective floor
   └── Orbit controls (mouse + touch)
   └── Click booth → enter

3. Interior scene (separate THREE.Scene)
   └── BoxGeometry walls with CanvasTexture graffiti
   └── getUserMedia → VideoTexture → ShaderMaterial on screen plane
   └── Look-around controls update camera.lookAt() each frame

4. Shooting session
   └── Countdown → canvas.toDataURL() snapshot → flash
   └── Repeat N times for strip type

5. Strip compositing
   └── Offscreen canvas draws border + photos + date
   └── toDataURL('image/png') → <a download>

6. Sticker editor
   └── Copy strip to second canvas
   └── PointerEvents for drag-and-drop emoji placement
   └── Final toDataURL → download
```

---

## Browser Support

| Browser | Status |
|---|---|
| Chrome 90+ | ✅ Full support |
| Edge 90+ | ✅ Full support |
| Firefox 88+ | ✅ Full support |
| Safari 15+ | ✅ Works (audio needs user gesture) |
| Chrome Android | ✅ Tested |
| Safari iOS 15+ | ✅ Works |

---





## Credits

- 3D photobooth model: `cabine_photo_booth.glb` (Sketchfab)
- Three.js r128 — [threejs.org](https://threejs.org)
- Fonts: Bebas Neue, DM Sans — Google Fonts

---

*Built for WebOreel 2025 hackathon — Phase 1 through 4 complete.*
