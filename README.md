# High-Graphics-FlappyBird

 A cinematic 3D Flappy Bird experience built entirely in a single HTML file — no build step, no installation, and no framework.

 It is designed to look like **camera footage rather than a traditional game-engine render**: a rigged GLB parrot with a real skeletal flap cycle flies through a procedurally lit sunset, weaving between weathered copper pipes with depth of field, film grain, and a blinking `REC` timecode.

 Developed by **Hussain**, the film opens on a black leader where the credit types itself out letter by letter, accompanied by mechanical keyboard clicks and a typewriter bell.

 `single file` · `no build step` · `three.js` · `WebGL2`

---
## Gameplay
<img width="1920" height="1080" alt="Screenshot (11)" src="https://github.com/user-attachments/assets/8b62325d-5e4b-4420-8fdb-c709b89cc318" />
<img width="1920" height="1080" alt="Screenshot (15)" src="https://github.com/user-attachments/assets/4368c173-a8a2-4071-ad0d-7bbcd2ad5752" />
<img width="1920" height="1080" alt="Screenshot (17)" src="https://github.com/user-attachments/assets/f07ffcc0-cf9b-4272-9ecb-93c545182892" />







---
 ## Quick Start

 ### Option A — Just Open It

1. Download `index.html`.
2. Double-click it.
3. That's it.

 ### Option B — Play Online

 **Live:**\
 https://xpxxxu.github.io/High-Graphics-FlappyBird/

 > **Note:** Requires a modern browser such as Chrome, Edge, Firefox, or Safari 16.4+, plus an internet connection on first load to fetch `three.js` and the bird model from CDN.
>
>  Nothing is uploaded. High scores are stored locally using `localStorage`.

---

 ## Controls

 | Input | Action |
| --- | --- |
| `Tap` / `Click` / `Space` / `↑` | Flap |
| `Any key` on cover | Roll the film or skip the intro |
| `R` | Restart after a crash |
| Speaker icon | Mute / unmute |

 Survive longer and the lighting changes.

 Every point deepens the sunset, while every 10 points introduces a new reel caption:

 `LOW SUN` → `EMBER` → `BLUE HOUR` → `NIGHTFALL`

 Thread a pipe gap within a feather's width to trigger a brief micro slow-motion pulse.

---

 ## The Film Treatment

 Everything is designed to emulate cinematic video optics rather than a conventional real-time game render.

 - **Image-based lighting:** A procedural sunset sky with FBM clouds and a sun disc is baked into an environment map, allowing surfaces to reflect the active sky.
- **ACES filmic tonemapping & bloom:** Produces realistic highlights and soft halation around the sun and rim edges.
- **Depth of field:** Dynamic focus follows the bird, with the aperture opening into a macro-camera effect during the death sequence.
- **Film grade pass:** Real-time chromatic aberration, vignette, live grain, and impact flashes.
- **Cinematic OSD:** Letterbox bars, blinking `REC` timecode, and camera OSD elements activate once the ident finishes.
- **Dynamic sky progression:** Sky color, fog density, sun position, and exposure gradually drift toward dusk.
- **Dynamic chase camera:** Handheld sway, field-of-view pulses on flaps, and a slow-motion orbit around the bird on collision.
- **Synthesized audio pipeline:** Native Web Audio synthesis provides wind, wing whooshes, score chimes, near-miss turbulence, analog chord pads, and mechanical typewriter sounds for the intro.

---

 ## The World

 - **Rigged GLB Parrot:** A full skeletal flap animation adapts to gameplay state — frantic during flaps, gliding during dives, and frozen mid-tumble on impact.
- **Instanced Vegetation:** 8,000 GPU-instanced grass blades with real-time wind sway through vertex displacement, plus drifting wildflower specks.
- **Weathered Copper Pipes:** Procedurally generated with patina, drip streaks, structural seams, and rivets.
- **Environmental Depth:** Distant mountain ridges, scrolling foliage, backlit dust motes, and occasional stork flocks crossing the sky.
- **Dynamic Lighting:** Real-time long soft shadows rendered at 4K on supported hardware.

---

 ## Custom Models

 The game automatically checks for local assets placed directly alongside `index.html`.

 | File | Replaces | Technical Notes |
| --- | --- | --- |
| `bird.glb` | Parrot | Any rigged bird model; the active flap clip must be mapped to `animations[0]` |
| `tree.glb` | Default trees | Auto-scaled to \~3.4 units, planted, and configured to cast shadows |
| `grass.glb` | Instanced grass | Instanced ×8,000 with custom vertex wind displacement |
| `bush.glb` | Default bushes | Auto-scaled to \~1 unit |
| `stork.glb` | Background flock | Optional environmental decoration |

 All imported models are automatically normalized:

 - Bounding-box scaling
- Centered origin
- Grounded base

 Assets from [Quaternius](<https://quaternius.com/>) (CC0) or [Sketchfab](<https://sketchfab.com/>) (CC-BY / CC0) can be dropped in directly.

 ### Local File Access

 Chrome restricts cross-origin requests on `file://` URIs.

 To run custom local models, start a simple local server:

```
python -m http.server
```

 Then navigate to:

```
http://localhost:8000
```

 > **Bird orientation:** If your custom bird flies backward, open `index.html`, locate:
>
>
> ```
> birdYaw.rotation.y = Math.PI
> ```
>
>  and change it to:
>
>
> ```
> birdYaw.rotation.y = 0
> ```

---

 ## Configuration & Tuning

 Game behavior can be adjusted by editing the configuration block inside `index.html`.

 Search for:

```
const MENU_WS
```

 | Variable | Default | Function |
| --- | --- | --- |
| `GRAVITY` | `26` | Downward acceleration rate |
| `FLAP_V` | `8.6` | Upward impulse strength |
| `BASE_WS` | `9` | Base forward world scroll speed (`+0.11` per point) |
| `MAX_FALL` | `13.5` | Terminal velocity during dives |
| `CEILING` | `8.7` | Soft upper flight boundary |
| `SPACING` | `13` | Horizontal distance between pipe gaps |
| `0.55` | — | Distance threshold used by `updatePipes` for near-miss pulses |
| `45` | — | Score required by `updateSkyProgression` to reach full dusk |

---

 ## Codebase Architecture

 The entire application is contained within a single structured HTML file.

 Subsystems are separated using structural markers, making the file easier to navigate and modify.

```
renderer
   │
   ├──► sky
   │      └──► lights
   │             └──► procedural textures
   │                    └──► ground
   │                           └──► grass
   │                                └──► instancing + wind
   │
   ├──► scenery
   │      ├──► GLB vegetation slots
   │      ├──► stork flock
   │      ├──► pipes
   │      └──► bird (GLB)
   │
   ├──► feathers
   ├──► dust
   │
   └──► post pipeline
          ├──► bokeh
          ├──► bloom
          └──► film grade
                 │
                 └──► audio state
                        │
                        ├──► cover ident
                        ├──► update logic
                        ├──► input handlers
                        └──► main loop
```

---

 ## Troubleshooting

 ### Black Screen / Infinite Loading

 Make sure you have an active internet connection on the initial load so the game can download its CDN dependencies, including the `three.js` runtime and default GLB assets.

 Also verify that your browser supports **WebGL2**.

 ### Console 404 Warnings

 These are generally harmless.

 They can occur when the asset loader checks for optional local model overrides such as:

```
tree.glb
grass.glb
bush.glb
stork.glb
```

 ### No Audio Playback

 Modern browsers block autoplay and prevent audio contexts from starting automatically.

 Press any key or click the initial cover screen to initialize the Web Audio pipeline.

---

 ## Credits & Dependencies

 - **Engine:** [three.js](<https://threejs.org/>) — MIT License
- **Default Models:** Animated `Parrot`, `Flamingo`, and `Stork` models from the three.js example repositories, by Mirada
- **Typography:** [Fraunces](<https://fonts.google.com/specimen/Fraunces>), [IBM Plex Mono](<https://fonts.google.com/specimen/IBM+Plex+Mono>), and [Doto](<https://fonts.google.com/specimen/Doto>) — SIL Open Font License
- **Audio:** 100% synthesized in real time using the Web Audio API; no external sound files
- **Textures:** Generated dynamically at runtime using the HTML Canvas API

---

 ## Author

 **Developed by Hussain**
