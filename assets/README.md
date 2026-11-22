# AQUA//SIM — Procedural Asset Manifest

Every asset in this project is **generated procedurally at runtime** (code + canvas
textures + GLSL). There are **no** `.glb` / `.gltf` files, no image downloads, and no
placeholder primitives standing in for real models. Nothing here is a "blob".

## Asset inventory (all procedural)

| Asset | Generator | Detail |
|---|---|---|
| Rimless glass tank (5 panels) | `THREE.MeshPhysicalMaterial` + BoxGeometry | transmission, thickness, IOR 1.52, green attenuation edges |
| Dark stand + floor | BoxGeometry / PlaneGeometry | PBR, soft shadows |
| LED light bar + volumetric rays | Box + Cylinder + custom GLSL | additive fresnel cones, flicker |
| Water surface | PlaneGeometry 96×96 + custom vertex/fragment GLSL | pump ripple, click/tap ripples, fresnel, sun glint |
| River sand substrate | Displaced PlaneGeometry 96×96 + canvas speckle texture | fbm mounds, caustics |
| Gravel pebbles | InstancedMesh (260×) displaced icosahedra | per-instance scale/rotation/color |
| Jagged stones (6) | Displaced IcosahedronGeometry, flat shaded | fbm displacement, caustics |
| Driftwood | Custom tapered tube along CatmullRom curve + branches | wood grain canvas texture |
| Vallisneria | Articulated ribbon leaves (8-seg) | GPU sway + current |
| Anubias | Broad leaves on rhizome | GPU sway |
| Java Fern | Narrow segmented leaves | GPU sway |
| Neon Tetra (6 fish) | Parametric ring-loft body + 5-bone skeleton | full fins, gills, mouth, eyes, iridescent stripe |
| Angelfish (4 fish) | Parametric disc body + 5-bone skeleton | tall dorsal/anal sails, stripes |
| Fancy Goldfish (5 fish) | Parametric rounded body + 5-bone skeleton | flowing forked tail, koi patches |
| Food pellets | Cylinder + drift/sink physics | float → sink → fade |
| Air bubbles | Instanced sphere pool, physics rise + wobble + pop | size-based buoyancy |
| Sponge filter / air pump | Cylinder + motor housing | vibrating motor, nozzle |
| Caustics | GLSL `causticPattern()` injected into PBR materials | world-space, animated |
| Underwater particulate | Points + additive sprite | drifting motes |

## Controls

- **Drag** orbit · **Wheel** zoom · **Right-drag** pan (can go inside the tank)
- **Click water** drop food · **Click glass** tap (ripple + startle)
- HUD: Drop Food · Oxygen Pump toggle · Daylight / Moonlight · Fish count

## Tech stack

- Three.js r160 (ES modules via CDN import map) + OrbitControls
- UnrealBloomPass post-processing, ACES tone mapping
- MeshPhysicalMaterial transmission glass, iridescence, clearcoat
- Custom GLSL: water, caustics, volumetric light, plant sway
- Boids + predictive look-ahead steering for collision-free swimming
