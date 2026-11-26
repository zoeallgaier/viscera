# Blackout Protocol

A base-defense game about light and darkness. Single file, no build step. Open `index.html` in a browser to play.

## The loop
- **Light is territory.** Lamps chain power out from the Core. You can only build within cable reach of a powered lamp, and turrets only fire at husks standing in light.
- **Light is a beacon.** Every lit source adds to Glow, and each night's wave grows with your Glow.
- **Bloom is a bargain.** Harvesters mine the glowing pools for Bloom, your only currency. Every unit mined raises Mutation, which makes husks tougher and unlocks new strains. Purge (50 Bloom) knocks Mutation back down.
- **Strains:** Walker, Runner, Bloater (bursts and snuffs nearby lamps for 10 seconds, which cuts power to everything downstream), Shade (hunts harvesters and shrugs off half of turret fire).

## Controls
| Key | Action |
| --- | --- |
| 1 / 2 / 3 | Lamp (12) / Harvester (25) / Turret (30) |
| 4 or right-click | Dismantle (refunds half) |
| F | Flare (40): stuns and damages every husk in light |
| P | Purge (50): lowers Mutation by 15% |
| Space | Pause |

## Deploy to GitHub Pages
Push this folder to a repo, then go to Settings → Pages → Deploy from branch → `main` / root.
