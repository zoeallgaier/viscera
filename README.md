# Eidolon

A base-defense game about light and darkness, built mobile-first. Single file, no build step. Open `index.html` in a browser to play.

**Play:** https://zoeallgaier.github.io/viscera/

On iPhone, open it in Safari, then Share → Add to Home Screen. It launches full-screen like an app.

## The loop
- **Light is territory.** Lamps chain power out from the Base. You can only build within cable reach of a powered lamp, and turrets only fire at drones standing in light.
- **Light is a beacon.** Every lit source adds to Glow, and each wave grows with your Glow.
- **Lithium is your budget.** Everything costs whole points: Lamp 1, Refinery 2, Turret 3, Missile launcher 5, Flare 4. You start with 6. Each refinery on a glowing pool earns 1 Lithium every 8 seconds, and every wave you survive pays 2. Pools run dry, so you have to push light out into the dark to reach richer ones.
- **Turrets vs. missiles.** Turrets fire fast at close range (4.2 tiles). Missile launchers reach 6.2 tiles and fire a homing missile every 2.2 seconds at the biggest cluster in light, with splash damage. Infiltrators take half damage from both.
- **Waves get worse.** Drones gain health every wave and runners become more common. Drone types: Walker, Runner, Panzer (wave 3+; bursts and snuffs nearby lamps for 10 seconds, which cuts power to everything downstream), Infiltrator (wave 6+; hunts refineries and shrugs off half of turret fire).

## Controls
**Touch:** tap a tile to build the selected tool. Long-press a building to dismantle it.

| Key | Action |
| --- | --- |
| 1 / 2 / 3 / 4 | Lamp (1) / Refinery (2) / Turret (3) / Missile launcher (5) |
| 5 or right-click | Dismantle (full refund) |
| F | Flare (4): stuns and damages every drone in light |
| Space | Pause |

## Deploy
GitHub Pages serves the `main` branch root. Push to `main` and the site updates in about a minute.
