# Blackout Protocol

A base-defense game about light and darkness, built mobile-first. Single file, no build step. Open `index.html` in a browser to play.

**Play:** https://zoeallgaier.github.io/viscera/

On iPhone, open it in Safari, then Share → Add to Home Screen. It launches full-screen like an app.

## The loop
- **Light is territory.** Lamps chain power out from the Core. You can only build within cable reach of a powered lamp, and turrets only fire at husks standing in light.
- **Light is a beacon.** Every lit source adds to Glow, and each night's wave grows with your Glow.
- **Bloom is a bargain.** Harvesters mine the glowing pools for Bloom, your only currency. Every unit mined raises Mutation, which makes husks tougher and unlocks new strains. Purge (50 Bloom) knocks Mutation back down.
- **Strains:** Walker, Runner, Bloater (bursts and snuffs nearby lamps for 10 seconds, which cuts power to everything downstream), Shade (hunts harvesters and shrugs off half of turret fire).

## Controls
**Touch:** tap a tile to build the selected tool. Long-press a building to dismantle it.

| Key | Action |
| --- | --- |
| 1 / 2 / 3 | Lamp (12) / Harvester (25) / Turret (30) |
| 4 or right-click | Dismantle (refunds half) |
| F | Flare (40): stuns and damages every husk in light |
| P | Purge (50): lowers Mutation by 15% |
| Space | Pause |

## Deploy
GitHub Pages serves the `main` branch root. Push to `main` and the site updates in about a minute.
