# Eidolon

A base-defense game about light and darkness, built mobile-first. Single file, no build step. Open `index.html` in a browser to play.

**Play:** https://zoeallgaier.github.io/viscera/

On iPhone, open it in Safari, then Share → Add to Home Screen. It launches full-screen like an app.

## The loop
- **Light is territory.** Servers chain power out from the Base. You can only build within cable reach of a powered server, and turrets only fire at drones standing in light.
- **Light is a beacon.** Every lit server adds to Data, and each wave grows with your Data.
- **Lithium is your budget.** Everything costs whole points: Server 1, Refinery 2, Turret 3, Missile launcher 5, EMP 4. You start with 10. Each refinery on a glowing pool earns 1 Lithium every 8 seconds, and every wave you survive pays 2. Pools run dry, so you have to push light out into the dark to reach richer ones.
- **Turrets vs. missiles.** Turrets fire fast at close range (4.2 tiles). Missile launchers reach 6.2 tiles and fire a homing missile every 2.2 seconds at the biggest cluster in light, with splash damage. Infiltrators take half damage from both.
- **Waves get worse.** Drones gain health every wave and runners become more common. Drone types: Walker, Runner, Panzer (wave 3+; bursts and snuffs nearby servers for 10 seconds, which cuts power to everything downstream), Infiltrator (wave 6+; hunts refineries and shrugs off half of turret fire).

## Controls
**Touch / mouse:** drag a piece from the pad onto the board to build it there, or tap a tile to build the selected tool. While you drag, the target tile shows a preview and range ring, and the piece fades if it can't go there. Let go off the board to cancel. On touch the drop point sits a little above your fingertip. Long-press a building to dismantle it.

| Key | Action |
| --- | --- |
| 1 / 2 / 3 / 4 | Server (1) / Refinery (2) / Turret (3) / Missile launcher (5) |
| Right-click (or long-press) | Dismantle a building (full refund) |
| F | EMP (4): stuns and damages every drone on the map |
| Space | Pause |

## Look
The full rules, and the research behind them, are in `research_notes/Isometric pixel art design system/`.
- **Board:** one isometric grid, and nothing else. Tiles are 32 × 16 pixel diamonds in 2:1 dimetric projection, the pixel-art standard, so every edge steps cleanly two across and one down. The board is the rectangle of whole tiles that fills the screen. Every position, range, speed and radius is measured in tiles. The Base covers the centre 3 × 3 tiles; every other piece covers one.
- **One renderer:** every board sprite is a small voxel model, 16 units per tile across and 1 pixel per unit of height. It is drawn through the same projection as the ground, so pieces stand exactly on their tiles. Turret and launcher heads are rendered at 32 headings, and drones at 8 headings with 2 frames each. Each face keeps one flat shade at any heading.
- **Light and shading:** light comes from the upper left. Tops are lightest, faces toward the lower left are mid, and faces toward the lower right are darkest. Each material is a 5-step ramp whose hue shifts warmer toward the light. Top edges that meet a wall catch a lit ledge, and walls darken where they meet the ground. Every standing piece has a 1px dark outline; ground decals like pools have none. There is no dither.
- **Colour means something:** gold is the player (light, power, cables, lit windows), blue is Lithium, lime is Data, and red is hostile (drone optics, damage, a tile you can't build on). Concrete, steel, glass and moss stay muted so those four stand out.
- **The Base:** a 1990s federal campus gone to seed. It has a brutalist HQ with recessed ribbon windows and a cantilevered top floor, and a tower with a radio mast and a gold beacon. Around it are a wing with a sweeping dish, a gatehouse and boom, a helipad, sedans, a flag and a chain-link fence, with moss on the roofs, vines down the walls and trees moving back in.
- **Drawing order:** everything is sorted by ground depth. Lit pixels (windows, LEDs, optics, beacons) go on their own layer, so they glow in the dark without showing through anything in front of them.
- **The unlit field:** it isn't darkened, it's pixelated. Outside the light the scene breaks into 8-pixel blocks, and a 4-pixel ring surrounds each light. Light reaches the ground as 2:1 ellipses. Inside it, and on every lit building, everything is sharp. It's a stepped mosaic, not a gaussian blur.
- **UI:** hairline glass panels (55% tint, 35px backdrop blur, 1px rule) float over a full-bleed map.
- **Type:** Michroma for titles (a free stand-in for New Science), Oxygen for everything else. Both load from Google Fonts.

## Deploy
GitHub Pages serves the `main` branch root. Push to `main` and the site updates in about a minute.
