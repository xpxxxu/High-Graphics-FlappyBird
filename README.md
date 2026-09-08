# High-Graphics-FlappyBird

A cinematic 3D Flappy Bird in a single HTML file — no build step, no installs, no framework. It renders like camera footage, not a game engine: a rigged GLB parrot with a real skeletal flap cycle, flying into a procedurally-lit sunset through weathered copper pipes, shot with depth of field, film grain and a blinking REC timecode.

Developed by Hussain — the film opens on a black leader where the credit types itself out, letter by letter, with mechanical keyboard clicks and a typewriter bell.

`single file` · `no build step` · `three.js` · `WebGL2`

---

## Quick Start

### Option A — Just Open It
1. Download `index.html`
2. Double-click it. That's it.

### Option B —
Click the link: https://xpxxxu.github.io/High-Graphics-FlappyBird/

> **Note:** Requires a modern browser (Chrome / Edge / Firefox / Safari 16.4+) and an internet connection on first load to fetch `three.js` and the bird model from CDN. Nothing is ever uploaded; high scores are saved locally via `localStorage`.

---

## Controls

| Input | Action |
| :--- | :--- |
| `Tap` / `Click` / `Space` / `↑` | Flap |
| `Any key` (on cover) | Roll the film — or skip the intro |
| `R` | Restart after a crash |
| `Speaker icon` (top right) | Mute / unmute |

Survive longer and the light changes: every point deepens the sunset, and every 10 points rolls a new "reel" caption (`LOW SUN` → `EMBER` → `BLUE HOUR` → `NIGHTFALL`). Thread a gap by a feather's width to trigger a micro slow-mo pulse.

---

## The Film Treatment

Everything designed to emulate video optics rather than a real-time render:

* **Image-based lighting:** Procedural sunset sky (fbm clouds, sun disc) is baked into an environment map so every surface reflects the active sky.
* **ACES filmic tonemapping & bloom:** Realistic highlights and halation on the sun and rim edges.
* **Depth of field:** Dynamic focus follows the bird; aperture opens into a macro camera dynamic during the death sequence.
* **Film grade pass:** Real-time chromatic aberration, vignette, live grain, and impact flashes.
* **Cinematic OSD:** Letterbox bars, blinking REC timecode, and camera OSD active once the ident finishes.
* **Dynamic sky progression:** Sky color, fog density, sun position, and exposure drift steadily toward dusk.
* **Dynamic chase camera:** Handheld sway, field-of-view pulses on flaps, and a slow-motion orbit around the bird upon collision.
* **Synthesized audio pipeline:** Native WebAudio synthesis for wind, wing whooshes, score chimes, near-miss air turbulence, analog chord pads (muffled via low-pass filter during slow-mo), and mechanical typewriter sounds for the intro.

---

## The World

* **Rigged GLB Parrot:** Features a full skeletal flap animation that adapts to gameplay state: frantic on flap, gliding on dive, frozen mid-tumble on impact.
* **Instanced Vegetation:** 8,000 GPU-instanced grass blades with real-time wind sway vertex injection and drifting wildflower specks.
* **Weathered Copper Pipes:** Generated with procedural patina, drip streaks, structural seams, and rivets.
* **Environmental Depth:** Distant mountain ridges, scrolling foliage, backlit dust motes, and occasional stork flocks crossing the sky.
* **Dynamic Lighting:** Real-time long soft shadows rendered at 4K resolution on supported hardware.

---

## Custom Models

The game automatically checks for local assets placed directly alongside `index.html`:

| File | Replaces | Technical Notes |
| :--- | :--- | :--- |
| `bird.glb` | The parrot | Any rigged bird model; active flap clip must be mapped to `animations[0]` |
| `tree.glb` | Default trees | Auto-scaled to ~3.4 units, planted, and cast shadows |
| `grass.glb` | Instanced grass | Instanced ×8,000 with custom vertex wind displacement |
| `bush.glb` | Default bushes | Auto-scaled to ~1 unit |
| `stork.glb` | Background flock | Optional environmental decoration |

All imported models are normalized automatically (bounding box scaling, centered origin, grounded base). Assets from [Quaternius](https://quaternius.com/) (CC0) or [Sketchfab](https://sketchfab.com/) (CC-BY / CC0) can be dropped in directly.

> **Local File Access:** Chrome restricts cross-origin requests on `file://` URIs. To run custom local models, initialize a simple local server:
> ```bash
> python -m http.server
> # Navigate to http://localhost:8000
> ```
> *If your custom bird flies backward, open `index.html`, locate `birdYaw.rotation.y = Math.PI`, and change the value to `0`.*

---

## Configuration & Tuning

Game behavior can be adjusted by editing the configuration block inside `index.html` (search for `const MENU_WS`):

| Variable | Default | Function |
| :--- | :--- | :--- |
| `GRAVITY` | `26` | Downward acceleration rate |
| `FLAP_V` | `8.6` | Upward impulse strength |
| `BASE_WS` | `9` | Base forward world scroll speed (+0.11/point) |
| `MAX_FALL` | `13.5` | Terminal velocity during dives |
| `CEILING` | `8.7` | Soft upper flight boundary |
| `SPACING` | `13` | Horizontal gap distance between pipes |
| `0.55` | *(in `updatePipes`)* | Distance threshold for triggering a near-miss pulse |
| `45` | *(in `updateSkyProgression`)* | Score required to reach full dusk |

---

## Codebase Architecture

The application is fully contained within a single structured file. Jump to specific subsystems using these structural markers:

renderer ──► sky ──► lights ──► procedural textures ──► ground ──► grass (instanced + wind)
│
├──► scenery ──► GLB vegetation slots ──► stork flock ──► pipes ──► bird (GLB)
│
└──► feathers ──► dust ──► post pipeline (bokeh / bloom / grade) ──► audio state
│
└──► cover ident ──► update logic ──► input handlers ──► main loop

```markdown
# High-Graphics-FlappyBird

A cinematic 3D Flappy Bird in a single HTML file — no build step, no installs, no framework. It renders like camera footage, not a game engine: a rigged GLB parrot with a real skeletal flap cycle, flying into a procedurally-lit sunset through weathered copper pipes, shot with depth of field, film grain and a blinking REC timecode.

Developed by Hussain — the film opens on a black leader where the credit types itself out, letter by letter, with mechanical keyboard clicks and a typewriter bell.

`single file` · `no build step` · `three.js` · `WebGL2`

---

## Quick Start

### Option A — Just Open It
1. Download `index.html`
2. Double-click it. That's it.

### Option B — Share Live with GitHub Pages
1. Create a repository containing `index.html` and this `README.md`
2. Navigate to **Repo → Settings → Pages**
3. Under **Source**, select **Deploy from a branch** set to `main` / `root`
4. Access your live film at: `https://<username>.github.io/<repo>/`

> **Note:** Requires a modern browser (Chrome / Edge / Firefox / Safari 16.4+) and an internet connection on first load to fetch `three.js` and the bird model from CDN. Nothing is ever uploaded; high scores are saved locally via `localStorage`.

---

## Controls

| Input | Action |
| :--- | :--- |
| `Tap` / `Click` / `Space` / `↑` | Flap |
| `Any key` (on cover) | Roll the film — or skip the intro |
| `R` | Restart after a crash |
| `Speaker icon` (top right) | Mute / unmute |

Survive longer and the light changes: every point deepens the sunset, and every 10 points rolls a new "reel" caption (`LOW SUN` → `EMBER` → `BLUE HOUR` → `NIGHTFALL`). Thread a gap by a feather's width to trigger a micro slow-mo pulse.

---

## The Film Treatment

Everything designed to emulate video optics rather than a real-time render:

* **Image-based lighting:** Procedural sunset sky (fbm clouds, sun disc) is baked into an environment map so every surface reflects the active sky.
* **ACES filmic tonemapping & bloom:** Realistic highlights and halation on the sun and rim edges.
* **Depth of field:** Dynamic focus follows the bird; aperture opens into a macro camera dynamic during the death sequence.
* **Film grade pass:** Real-time chromatic aberration, vignette, live grain, and impact flashes.
* **Cinematic OSD:** Letterbox bars, blinking REC timecode, and camera OSD active once the ident finishes.
* **Dynamic sky progression:** Sky color, fog density, sun position, and exposure drift steadily toward dusk.
* **Dynamic chase camera:** Handheld sway, field-of-view pulses on flaps, and a slow-motion orbit around the bird upon collision.
* **Synthesized audio pipeline:** Native WebAudio synthesis for wind, wing whooshes, score chimes, near-miss air turbulence, analog chord pads (muffled via low-pass filter during slow-mo), and mechanical typewriter sounds for the intro.

---

## The World

* **Rigged GLB Parrot:** Features a full skeletal flap animation that adapts to gameplay state: frantic on flap, gliding on dive, frozen mid-tumble on impact.
* **Instanced Vegetation:** 8,000 GPU-instanced grass blades with real-time wind sway vertex injection and drifting wildflower specks.
* **Weathered Copper Pipes:** Generated with procedural patina, drip streaks, structural seams, and rivets.
* **Environmental Depth:** Distant mountain ridges, scrolling foliage, backlit dust motes, and occasional stork flocks crossing the sky.
* **Dynamic Lighting:** Real-time long soft shadows rendered at 4K resolution on supported hardware.

---

## Custom Models

The game automatically checks for local assets placed directly alongside `index.html`:

| File | Replaces | Technical Notes |
| :--- | :--- | :--- |
| `bird.glb` | The parrot | Any rigged bird model; active flap clip must be mapped to `animations[0]` |
| `tree.glb` | Default trees | Auto-scaled to ~3.4 units, planted, and cast shadows |
| `grass.glb` | Instanced grass | Instanced ×8,000 with custom vertex wind displacement |
| `bush.glb` | Default bushes | Auto-scaled to ~1 unit |
| `stork.glb` | Background flock | Optional environmental decoration |

All imported models are normalized automatically (bounding box scaling, centered origin, grounded base). Assets from [Quaternius](https://quaternius.com/) (CC0) or [Sketchfab](https://sketchfab.com/) (CC-BY / CC0) can be dropped in directly.

> **Local File Access:** Chrome restricts cross-origin requests on `file://` URIs. To run custom local models, initialize a simple local server:
> ```bash
> python -m http.server
> # Navigate to http://localhost:8000
> ```
> *If your custom bird flies backward, open `index.html`, locate `birdYaw.rotation.y = Math.PI`, and change the value to `0`.*

---

## Configuration & Tuning

Game behavior can be adjusted by editing the configuration block inside `index.html` (search for `const MENU_WS`):

| Variable | Default | Function |
| :--- | :--- | :--- |
| `GRAVITY` | `26` | Downward acceleration rate |
| `FLAP_V` | `8.6` | Upward impulse strength |
| `BASE_WS` | `9` | Base forward world scroll speed (+0.11/point) |
| `MAX_FALL` | `13.5` | Terminal velocity during dives |
| `CEILING` | `8.7` | Soft upper flight boundary |
| `SPACING` | `13` | Horizontal gap distance between pipes |
| `0.55` | *(in `updatePipes`)* | Distance threshold for triggering a near-miss pulse |
| `45` | *(in `updateSkyProgression`)* | Score required to reach full dusk |

---

## Codebase Architecture

The application is fully contained within a single structured file. Jump to specific subsystems using these structural markers:


```

renderer ──► sky ──► lights ──► procedural textures ──► ground ──► grass (instanced + wind)
│
├──► scenery ──► GLB vegetation slots ──► stork flock ──► pipes ──► bird (GLB)
│
└──► feathers ──► dust ──► post pipeline (bokeh / bloom / grade) ──► audio state
│
└──► cover ident ──► update logic ──► input handlers ──► main loop

```

* **Environment Integration:** The procedural sky shader generates an active light map via PMREM.
* **Shader Injection:** Wind physics are directly injected into standard instanced material vertex shaders.
* **Performance Watchdog:** Automatically downscales pixel ratio and disables high-cost depth-of-field passes if frame rates drop below target, maintaining fluid performance on lower-tier hardware.

---

## Troubleshooting

* **Black Screen / Infinite Loading:** Ensure you have an active internet connection on the initial load to download CDN dependencies (`three.js` runtime and default GLB assets). Requires WebGL2 support.
* **Console 404 Warnings:** Harmless fallback checks raised while the asset loader searches for optional local model overrides (`tree.glb`, `grass.glb`, etc.).
* **No Audio Playback:** Modern browsers block autoplay context creation. Pressing any key or clicking on the initial cover screen initializes the WebAudio pipeline.

---

## Credits & Dependencies

* **Engine:** [three.js](https://threejs.org/) (MIT License). Default animated `Parrot`, `Flamingo`, and `Stork` models sourced from three.js example repositories (models by Mirada).
* **Typography:** [Fraunces](https://fonts.google.com/specimen/Fraunces), [IBM Plex Mono](https://fonts.google.com/specimen/IBM+Plex+Mono), and [Doto](https://fonts.google.com/specimen/Doto) (SIL Open Font License).
* **Audio:** 100% synthesized in real-time via WebAudio API (no external sound files).
* **Textures:** Generated dynamically via HTML canvas context at runtime.

---

**Author:** Developed by Hussain

```
