# CyberDefense: AI

A base-defense game about light and darkness, built mobile-first. One page and its sound cues in `sfx/`, no build step. Open `index.html` in a browser to play.

**Play:** https://zoeallgaier.github.io/viscera/

On iPhone, open it in Safari, then Share → Add to Home Screen. It launches full-screen like an app.

## The loop
- **Light is territory.** Servers chain power out from the Base. You can only build within cable reach of a powered server, and turrets only fire at bots standing in light.
- **Data cuts both ways.** Every powered server, turret and launcher (and the Base) adds 1 Data. Each point adds 4% to turret, missile and EMP damage, and each wave grows with your Data.
- **Lithium is your budget.** Everything costs whole points: Server 1, Refinery 2, Turret 3, Missiles 4, EMP 5. You start with 10. Each refinery on a glowing pool earns 1 Lithium every 8 seconds, and every wave you clear (last bot down) pays 2, plus 1 more for every 3 waves held, up to 6 per wave from wave 12 on. Pools run dry (the two by the Base go first, around waves 3–5), so you have to push light out into the void to reach richer ones. A purple gauge under each refinery shows what's left in its pool, and a dry one goes dark and reads DRY; scrap it for its full 2 Li back.
- **Turrets vs. missiles.** Turrets fire fast at close range (4.2 tiles). Missile launchers reach 6.2 tiles and fire a homing missile every 2.2 seconds at the biggest cluster in light, with splash damage. Infiltrators take half damage from missiles, full from turrets.
- **Waves get worse.** Bots gain health every wave and runners become more common. Bot types: Walker, Runner, Panzer (wave 3+; bursts and snuffs nearby servers for 10 seconds, which cuts power to everything downstream), Infiltrator (wave 6+; hunts refineries and shrugs off half of missile damage).
- **The Megapanzer.** A boss on wave 10 and every 5th wave after (15, 20, 25…). It's a 3 × 3 siege crawler with three treads, a crusher drum and a caged red core. It has 500 HP on wave 10 and grows 12% a wave from there. It doesn't hunt: it takes a random three-row lane (not based on what you've built), the lane flashes red for 3 seconds, then it cuts across the board on a diagonal (along the tile lines) end to end, never turning, and crushes everything its body covers for 12 damage a second. A pass over the Base does 200. Kill it and it bursts (30 damage to buildings within 2.6 tiles, servers snuffed) and pays a 3 Li bounty. The EMP stuns it for half as long. The wave isn't held until it's dead or has rolled off the far edge.

## Controls
**Touch / mouse:** drag a piece from the pad onto the board to build it there, or tap a tile to build the selected tool. While you drag, the target tile shows a preview and range ring, and the piece fades if it can't go there. Let go off the board to cancel. On touch the drop point sits a little above your fingertip. Long-press a building to scrap it.

**Score:** waves held. Your best is saved on the device.

| Key | Action |
| --- | --- |
| 1 / 2 / 3 / 4 | Server / Refinery / Turret / Missiles (keys follow pad order; the pad labels are prices) |
| Right-click (or long-press) | Dismantle a building (full refund) |
| 5 or F | EMP (5 Li): a pulse runs out across the board; every bot it reaches is stunned for 5 seconds and damaged |
| Space | Pause |
| M (or the speaker in the top bar) | Sound on / off (remembered on the device) |

## Look
The full rules, and the research behind them, are in `research_notes/Isometric pixel art design system/`.
- **Board:** one isometric grid, and nothing else. Tiles are 32 × 16 pixel diamonds in 2:1 dimetric projection, the pixel-art standard, so every edge steps cleanly two across and one down. The board is the rectangle of whole tiles that fills the screen. Every position, range, speed and radius is measured in tiles. The Base covers the centre 3 × 3 tiles; every other piece covers one.
- **One renderer:** every board sprite is a small voxel model, 16 units per tile across and 1 pixel per unit of height. It is drawn through the same projection as the ground, so pieces stand exactly on their tiles. Turret and launcher heads are rendered at 32 headings, and bots at 8 headings with 2 frames each. Each face keeps one flat shade at any heading.
- **Light and shading:** light comes from the upper left. Tops are lightest, faces toward the lower left are mid, and faces toward the lower right are darkest. Each material is a 5-step ramp whose hue shifts warmer toward the light. Top edges that meet a wall catch a lit ledge, and walls darken where they meet the ground. Every standing piece has a 1px dark outline; ground decals like pools have none. There is no dither.
- **Colour means something:** blue is yours, red is theirs.
  - **Yours:** blue light and power (servers, cables, lit windows, turret bands, missiles, the EMP), on warm graphite and weathered concrete. Everything you build carries a blue light.
  - **Theirs:** every bot class shares one family, cold slate or oxblood bodies with red optics and cores. The classes are told apart by shape. Red only ever means bots and the damage they do. A tile you can't build on is a dim dashed outline, and refusals are never red.
  - **Lithium** is violet purple. **Data** is phosphor green, the LEDs on your racks.
  - **The world** is one dark, low-chroma grade: near-black warm greys, ochre highlights, muted olive moss. It is muted so the meaning colours stand out, and the UI uses the same grade (warm ink and parchment, the same four accents).
  - Palette: blue `#3cc2ee`, Lithium `#9b6ef0`, Data `#7fd48a`, hostile red `#ff5d57`, ink `#0c0a0a`, parchment `#eee4d4`.
- **The Base:** a 1990s listening station gone to seed. A geodesic radome on a concrete drum is the landmark. Beside it are a low brutalist ops block with a radar bar turning on its roof, a turf-roofed bunker with a lit blast door, two ground dishes, and a short lattice tower carrying the gold beacon. A helipad, a guard hut and boom, and a chain-link fence complete it, with moss on the roofs, vines down the walls and trees moving back in.
- **Drawing order:** everything is sorted by ground depth. Lit pixels (windows, LEDs, optics, beacons) go on their own layer, so they glow in the dark without showing through anything in front of them.
- **Damage you can see:** damage is cut into the voxel models themselves and rendered through the same light. Below two-thirds health a piece has a bite out of it and soot on its walls. Below one-third it has bigger holes, dead lights and a few embers, and it smokes and sparks. A chunk blows off each time it drops a stage. A destroyed building leaves rubble, and when the Base falls its dark shell goes up in a chain of blasts.
- **Effects:** two layers. Debris, smoke and spent casings belong to the world, so the void shades them. Fire, sparks, tracers and hit flashes are light, drawn at full strength on top. Your fire is blue and theirs is red. Every fade steps down a colour ramp, and screen shake moves the view by whole pixels.
- **The EMP:** blue sparks gather in the radome, a beam fires from it, and a square wavefront of tile outlines races out along the board's own grid. Each bot is hit when the wave reaches it: it glitches, its red optics go dark, and they flicker back on as it reboots.
- **Small things:** new pieces print in from the ground behind a scanline, and scrapped ones wipe out from the top. Lights stutter when power comes and goes, pulses run along the cables, idle turrets sweep, and each point of Lithium flies up into the counter.
- **The ground:** the cracked apron of an abandoned campus. The upper-left edge of each slab catches the light and the upper-right one falls into shadow. Moss and dry grass take back patches across tile edges, and a slow drift in value keeps the field uneven.
- **The unlit field:** it's shaded, never blurred, so the bot designs read clearly in the void. The shade steps along the board's own grid: a lighter ring of quarter-tile diamonds (8 × 4 px) just outside each light, and a deeper shade on half-tile diamonds (16 × 8 px) beyond it. Inside the light, and on every lit building, there's no shade.
- **Light on the ground:** the Base and servers lift the ground in three stepped bands (a near-neutral warm light, so the ground keeps its colour) and live pools cast two bands of purple, on the same diamonds. Light lands on the ground, not on the buildings standing in it. There are no gradients on the board.
- **UI:** the chrome follows the board's rules. Every panel is a concrete instrument plate: one flat warm fill, the same 1px dark outline the sprites wear, a lit ledge along the top and a darker foot, and corners cut at 45°. Nothing is blurred. The name and the readouts share one console plate, split by an engraved groove. Pad pieces stand on small concrete slabs shaded like board tiles, and the chosen one is ringed in blue like the target tile. When a menu opens, the chrome steps away and the field breaks into a stepped mosaic behind the card. This is the only place the board is broken up.
- **Type:** Michroma (a free stand-in for New Science) is the voice of the station: the name, menu titles and eyebrows, readout labels and numbers, wave calls, the paused tag and the main button. Oxygen carries sentences, rules and pad labels. Both load from Google Fonts.

## Sound
Every cue is a short `.wav` in `sfx/`, mapped to events in the `SFX` table in `index.html`. Each cue has a level, a gap it can't restart inside, and for the busy ones a small random pitch drift, so many turrets firing don't all sound on one note.

| Cue | File | Plays on |
| --- | --- | --- |
| turret | `fire-turrets` | a turret firing |
| launch | `fire-missles` | a missile leaving its tube |
| botHit | `damage-bot` | any hit on a bot: rounds, missile splash, the EMP front |
| userHit | `damage-user` | a bot striking one of your buildings (louder on the Base) |
| pop | `explosion02` | a bot destroyed, a missile landing, a chunk blowing off a building |
| blast | `explosion01` | a Panzer going up, a building wrecked, the Base's chain of blasts (the last one deeper) |
| emp | `EMP` | firing the EMP |
| wave | `wave` | a wave incoming |
| confirm | `ui-confirm` | scrapping (lower), a wave held (higher), Start / Again / Resume. Building a piece is silent |
| blip | `ui-blip` | picking a tool, pause, speed, ?, the field guide; a refusal plays it low |

On the web the cues play through Web Audio, which a browser only starts after a press, so the first tap or key unlocks it. Explosions go through a limiter, so many at once sound dense instead of clipping. Opened from disk, where browsers refuse `fetch`, the page falls back to `<audio>` elements. The iOS app plays the same cues natively (see the app's `Sound.swift`), as ambient audio: the ring switch silences it, and music from other apps keeps playing.

## Deploy
GitHub Pages serves the `main` branch root. Push to `main` and the site updates in about a minute.
