# Readability of Units and Boards in Isometric Tactics, Strategy and Tower-Defense Games

Context: a small, dark-themed browser tower defense on a 2:1 isometric grid. Enemy drones fly toward a central base. The player places turrets, missile launchers, refineries and lamps, and lamps light up the dark. These notes collect sourced findings plus clearly labelled inferences for the report writer.

Source quality note: developer primary sources were scarce for several sub-topics. I could not fetch the Rock Paper Shotgun Into the Breach interview, gamedev.stackexchange, or the GDC Vault video directly; I worked from Game Developer's rewrite of the RPS piece. Much of the practical guidance online is forum posts and tutorial blogs, which are marked as such. Where a claim rests only on a search-engine summary rather than a fetched page, it is flagged "(search summary, page not fetched)".

---

## 1. Silhouettes: making units readable at 16–32 px (value contrast, squint test)

### Takeaway
Pixel artists check readability with three tests: a solid-black silhouette fill, a squint or blur at arm's length, and a desaturated screenshot. The usual fixes are to exaggerate one signature feature and raise the value contrast between the unit and the ground, not to add internal detail. Game-scale value hierarchy matters as much as sprite design: keep ground textures low in contrast so units can be the brightest, highest-contrast things on screen.

### Cited Findings
- **Silhouette test:** fill the sprite with pure black and squint from about a metre away. You should still be able to tell a warrior, mage, scout or monster type. Suggested fixes for a failing sprite are a bigger or angled headpiece, a held object, or more contrast — [freegamesprites.com, "sprite silhouette test"](https://freegamesprites.com/es/news/sprite-silhouette-test-character-readability) (tutorial blog)
- **Squint test:** hold the design at arm's length until details blur and check the object is still identifiable. For 8-pixel-scale design, "exaggerate the most characteristic feature of each object until it becomes unmistakable" — [Pixel Art Nerds, "Art of 8-pixel game design"](https://pixelartnerds.pages.dev/content/art-of-8-pixel-game-design) (tutorial blog)
- **Blur in software** is an accepted alternative to squinting — [GB Studio Central](https://gbstudiocentral.com/?p=2094) (search summary, page not fetched)
- **Background test:** paste the sprite onto pure black, pure white and a mid-grey tile colour. If it vanishes on any of them, raise outline contrast or change the fill value rather than adding internal detail — [Sorceress Games, "How to make pixel art (browser canvas)"](https://sorceress.games/blog/map-how-to-make-pixel-art-browser-canvas) (tutorial blog; search summary)
- **Don't polish too early:** adding micro-dither and cheek blush on the first pass means "you will polish a silhouette that still fails the squint test." At 16×16 the 256 available pixels force clear shape design — [sprite-ai.art 16×16 guide](https://www.sprite-ai.art/guides/how-to-create-16x16-pixel-art) (search summary)
- **Desaturation test (Peter Angstadt, Game Developer, 2015):** "Take a screenshot, desaturate it, and see if you can immediately and easily understand the image." In DOTA 2, "the heroes and units are the brightest and highest areas of contrast," while the ground's value range is kept small "so as not to draw attention." He also warns that "if everything is drawing attention, nothing will." — [Game Developer, "How to Reduce Visual Confusion in Your Game"](https://www.gamedeveloper.com/design/how-to-reduce-visual-confusion-in-your-game)
- **Angstadt's fix for a muddy scene** (DIGHARD, where "The hero's silhouette is barely visible!"): darken the background, brighten the walkable surfaces, and raise the character's brightness slightly. To check, add a black layer in Saturation blend mode in Photoshop and toggle it. Limbo shows the scheme can be inverted, with light backgrounds and very dark gameplay elements — [Game Developer](https://www.gamedeveloper.com/design/how-to-reduce-visual-confusion-in-your-game)
- **Tempest Tower (Half Past Yellow) value cap:** a value chart limits the greyscale value of background textures so "the background never has too much attention-grabbing contrast, while characters and enemies are allowed to stand out." Small enemies use "bolder colors and contrast to make their standout feature 'pop'" so they read "even with their small size on the screen." — [Unity blog, Half Past Yellow on Tempest Tower enemies](https://unity.com/blog/half-past-yellow-creating-enemy-visuals-tempest-tower)
- **Into the Breach** chose "a relatively basic pixel art style," and all of its pixel art was made in Photoshop — [Game Developer, Road to the IGF: Into the Breach](https://gamedeveloper.com/game-platforms/road-to-the-igf-subset-games-i-into-the-breach-i-) (search summary)
- **Subway legibility (Zach Gage, GDC 2018):** a good screenshot is readable in layers. The first glance should catch big iconic shapes, using Into the Breach's grid, giant mechs, insect enemies and fire as examples. Layer 2 carries harder information such as turn counters, and layer 3 holds detailed rules — [GIGAZINE summary of Gage's "Building Games That Can Be Understood at a Glance"](https://gigazine.net/gsc_news/en/20180501-game-design-subway-legibility) (original lecture: stfj.net/DesigningForSubwayLegibility)

### Inferences
- On a dark board, the ground should sit in a narrow low-value band, for example the bottom 15–35% of the value range. Drones and threats then need the highest value or saturation, and player buildings sit in between. This follows directly from Angstadt's DOTA 2 example and the Tempest Tower value cap.
- At 16–32 px, every drone class should differ in its silhouette mass, not only in colour. Options are wing count or shape, a body that is wider or taller, or one exaggerated protrusion. That way the classes still read in the black-fill test and for colour-blind players.
- Turrets, missile launchers, refineries and lamps should each have a different top silhouette, because the top is what shows above other sprites in an iso stack. Examples: turret as a squat dome with a barrel, launcher as a box rack, refinery as a tall stack or chimney, lamp as a thin pole with a bright head.
- A practical pipeline check: render every sprite as a black fill on the actual dark ground tile and on a lamp-lit tile, and run the desaturate test on real gameplay screenshots.

### Gaps
- I couldn't retrieve primary tutorials from Saint11 (Pedro Medeiros), Slynyrd (Raymond Schlitter) or AdamCYounis on silhouettes or outlines. The searches only surfaced secondary tutorial blogs, so the report should not attribute specific rules to those artists without checking.
- I found no sourced numbers for minimum value contrast (for example a ΔL* threshold) between a unit and the ground for pixel art.

---

## 2. Drop shadows: ellipse vs projected, size, anchoring hovering units

### Takeaway
The dominant convention for iso and top-down sprite games is a separate, simple dark ellipse on the ground plane. It is not baked into the sprite and is not a true projected shadow. Keeping it separate lets its offset from the unit show height, which is what anchors a hovering or flying unit to a tile. I found no authoritative sizing numbers. The guidance in Inferences is practitioner reasoning.

### Cited Findings
- On isometric tiles, players report difficulty telling which tile a character stands on, and a shadow under characters is the suggested fix — [itch.io comment thread](https://itch.io/post/13109850) (forum; search summary, exact post not verified)
- A developer explains why they did not bake a drop shadow into each sprite: "it wouldn't give me any differences in heights, and secondly it wouldn't allow me to 'kit bash' sprites on any individual layer as they'd all have conflicting drop shadows." — [Unity Discussions, "2.5D Geometric shadows"](https://discussions.unity.com/t/2-5d-geometric-shadows/928850.md) (forum; search summary)
- Shadows cast by tall tiles will not overlap the tiles that cast them unless height-map data is added to change the shadow mask's shape — [BlitzBasic forum backup](https://blitz-community.github.io/blitzbasic-backup/forums/monkey-programming/208084.html) (forum; search summary)
- The common tutorial advice is that a simple dark ellipse under the character is the easiest option. It should be a dark tone, not pure black, ideally a darker version of the floor colour. A more advanced option is a shadow that follows the character's silhouette. Shadows on the ground help players judge an airborne character's height and make characters look grounded — [ModDB, "Eight Ways to Improve Your Pixel Art Characters"](https://www.moddb.com/games/innkeep/tutorials/eight-ways-to-improve-your-pixel-art-characters) and [ModDB, "How to make 2D games look better by adding realism"](https://www.moddb.com/tutorials/how-to-make-2d-games-look-better-by-adding-realism) (search summary; the ModDB page returned 403 when fetched, so the attribution of each point to each page is not verified)
- A forum fix for characters that appear to "float": move the shadow down or tighten it to the bottom edge of the feet, then tune it per sprite — [Articulate community thread](https://community.articulate.com/discussions/articulate-presenter/shadow-beneath-figure) (search summary)
- Warcraft III uses a standard shared "unit shadow" texture for ground units, applied separately from the model — [Hive Workshop thread](https://www.HiveWorkshop.com/threads/special-effect-shadows.353487/) (search summary, excerpt only)

### Inferences
- **Grounded units (turrets, launchers, refineries, lamps):** a soft ellipse in a 2:1 aspect ratio, matching the iso tile, centred on the footprint's ground point. A width of roughly 60–90% of the footprint diamond works, so it reads as contact without spilling onto neighbouring tiles. These figures are practitioner guidance, not sourced. The 2:1 ratio does follow from the projection.
- **Hovering drones:** draw the shadow at the ground point of the tile the drone is over, and draw the drone sprite raised by its hover height in screen pixels. The vertical gap between them is the height cue, and the shadow's position is the true board position for targeting and range checks. When a drone bobs, shrink the shadow slightly and lighten its alpha as it rises, and grow and darken it as it descends. That reinforces altitude and keeps the shadow tied to the tile.
- **In a dark game,** a dark ellipse on a dark floor nearly disappears. Two alternatives: a slightly lighter or emissive "ground-contact glow" ring under drones (a light-pool that inverts the shadow), or a shadow drawn only inside lamp-lit areas. In unlit areas a faint enemy-hued ring or underglow can do the anchoring job instead. This idea is extrapolated from the emissive-readability findings in section 8.
- **Use pixel-art ellipses** with hard edges and 1–2 alpha steps, not blurred ones, to stay consistent with the art style. Render all shadows in one pass beneath every unit so they never draw over a neighbouring sprite.
- **Projected shadows** pointing away from a light source would conflict when there are many lamps, since each lamp implies a different direction. Prefer a non-directional ellipse; if lamp-direction shadows are wanted, add them as a subtle extra layer.

### Gaps
- I found no sourced convention for shadow width as a fraction of sprite width, shadow alpha, or shadow offset per pixel of hover height. These should be tuned in-engine.
- I found no developer commentary from an iso TD, such as Kingdom Rush or Defense Grid, on how flying-enemy shadows are handled.

---

## 3. Facing: 4, 5, 8 or 16 directions, "draw 5, mirror 3", and turret rotation

### Takeaway
For 8-direction characters, the classic practice is to author 5 unique facings (N, NE, E, SE, S) and mirror the other 3 (NW, W, SW) horizontally. N and S cannot be mirrored because they are the back and front views. In 2:1 iso, horizontal flipping maps screen-left to screen-right facings cleanly, so a 4-direction iso set needs only 2 drawn facings. Mirroring breaks handed details and directional lighting. For turrets and rotating heads, the usual approaches are separate turret and base sprites with 16 or 32 pre-rendered angles, or sprite-stacking slices rotated at runtime.

### Cited Findings
- **Which directions mirror:** "you really only have 3 mirrors: NW/NE, W/E, and SW/SE." N and S can't be mirrored because one shows the back and the other the front, so you author 5 and extrapolate to 8. This holds only if the sprites are roughly symmetrical about the vertical axis or "you don't care about the mirroring artifacts." — [gamedev.net, "Direction in isometric games"](https://gamedev.net/forums/topic/632342-direction-in-isometric-games/4987130/) (forum; search summary)
- Mirroring the diagonal frames saves roughly 37% of the art, but in iso games mirroring is "less effective than in other formats" — [gamedev.net thread, "8 directions for one unit"](https://gamedev.net/forums/topic/281429-8-directions-for-one-unit) (forum; search summary, attribution to this specific thread not verified)
- **Historical precedent:** older pseudo-3D games such as Doom usually had 8 directions for character sprites but stored only 5 unique sprites and mirrored the other 3 — [TV Tropes, Sprite Mirroring](https://tvtropes.org/pmwiki/pmwiki.php/Main/SpriteMirroring) and [ZDoom wiki, Sprite](https://zdoom.org/wiki/Sprite) (search summaries)
- **Sprite counts by perspective (cxong, 2022):**
  - Isometric with 4 directions costs about 2–2.5× the art: draw front-facing and back-facing sprites and mirror them for the other 2.
  - 8 directions costs about 5–5.5×: at least 5 sets, the 3 cardinal sets plus 2 diagonals.
  - Workarounds: draw only 4 directions and allow diagonal movement, use a pure top-down view and let the renderer rotate the sprite (Alien Breed), or use 3D-rendered sprites for any number of angles (Diablo).
  - Isometric examples: Super Mario RPG, Theme Hospital, Tactics Ogre.
  - Handed characters "can look odd when mirrored" (the "Ambidextrous sprite" trope). Dragon Quest is cited for keeping swords and shields on the correct sides.
  - — [cxong, "How many sprites do different perspectives need"](https://cxong.github.io/2022/03/how-many-sprites-do-different-perspectives-need)
- **Separate turret sheets:** a pre-rendered RTS vehicle pack uses 32 rotations at 4 camera angles (0°, 45°, 60°, 90°), with separate sheets for tank body and tank turret. The sheets were generated from voxel models and sprite stacking — [General Games, RTS Vehicles Sprite Pack](https://generalgames.itch.io/vehicle-sprite-sheets) and [devlog on turret sheets](https://generalgames.itch.io/vehicle-sprite-sheets/devlog/414066/added-new-sprite-sheets-for-turrets-and-tank-body)
- **Sprite stacking:** a pseudo-3D object built from horizontal 2D slices stacked with about a 1-pixel offset, which allows continuous rotation — [Voided Pixels, "The Magic of Sprite Stacking"](https://voidedpixels.itch.io/superfluous-sand/devlog/161222/the-magic-of-sprite-stacking). At runtime, code "iterate[s] quickly through each sprite slice and sets their rotation to match the boats current angle every tick" — [Wee Boats devlog](https://itch.io/devlog/685630/using-sprite-stacking-for-pseudo-3d-in-wee-boats.amp). The Sprite Stacker tool exports iso pseudo-3D sheets or stacks "to allow for any 360° rotation on the up-axis" — [Sprite Stacker](https://berlinnights.itch.io/sprite-stacker). See also [80.lv, 3D aiming with sprite stacking in GameMaker](https://80.lv/articles/how-to-implement-3d-aiming-using-sprite-stacking-in-gamemaker)
- **Snapping to pre-drawn directions:** switch directional frames at the midpoints between angles, for example from the 0° frame to the 45° frame at 22.5°. A known artefact: "when my sprite is facing 45° angle but is shooting towards 65° angle it looks bad." — [Construct forum](https://construct.net/en/forum/construct-2/how-do-i-18/direction-animations-shoot-124536) (search summary)
- **Rotating flat pixel art** works best with the RotSprite algorithm — [spriterot (GitHub)](https://github.com/samdze/spriterot)

### Inferences
- **2:1 iso mirroring.** A horizontal flip maps screen-NE↔NW, E↔W and SE↔SW. N (straight up-screen) and S (straight down-screen) are self-symmetric and must be drawn.
  - For a 4-facing iso unit whose facings follow the grid axes, the 4 screen diagonals: draw SE (front-3/4) and NE (back-3/4), and mirror them to SW and NW. This matches cxong's "front + back, mirror the rest".
  - For 8 facings: draw S, SE, E, NE, N and mirror 3.
  - For 16 facings: draw 9 (S, N and the 7 in between on one side) and mirror 7.
- **Mirroring flips any baked directional lighting.** If sprites are lit from the top-left, mirrored frames will be lit from the top-right. With lamps as the only dynamic light, a neutral, top-lit sprite shading scheme keeps mirroring safe. This is inference; I could not fetch the gamedev.stackexchange question on mirroring isometric angles to confirm it.
- **Drones** can often be near-radially symmetric (quadcopter or saucer). A 1-facing or 4-facing drone with a small directional "eye" or light, flipped as needed, saves a lot of art and stays readable. Direction of travel is already obvious from the drone's motion.
- **Turrets and launchers:** split each into a static base sprite (1 facing, drawn once for the iso view) and a rotating head.
  - At 16–32 px, 16 pre-rendered head angles (22.5° steps) are usually enough. 32 angles (11.25°) look smoother for slow tracking.
  - Mirroring cuts authoring to 9 of 16, or 17 of 32, if the head is left-right symmetric.
  - Sprite-stacked heads, rendered at runtime with integer-pixel offsets and nearest-neighbour sampling, are a good fit for a browser game because one small slice sheet covers 360°. The rotation must be snapped so the pixel art doesn't shimmer.
- **Aim in world space.** On a 2:1 iso board, an aim angle in world (grid) space is not the same as the screen angle. Convert the target vector from world to screen before choosing the frame, or the barrel will visibly point off-target on diagonals.

### Gaps
- I could not confirm StarCraft or Brood War's facing count. The commonly repeated "32 facings from 17 rendered and mirrored" is unverified, and I found no source for it.
- I found no primary source specifically on Into the Breach, FFT or Tactics Ogre facing counts. Tactics Ogre is cited by cxong only as a 4-direction iso example.

---

## 4. Team colour: separating friend from foe without harming cohesion

### Takeaway
Strategy games usually recolour only a reserved region of each sprite (a sash, flag, trim or light panel) through palette-index or colour-key swapping. The rest of the sprite keeps the shared world palette, which keeps cohesion. Readability comes from a dedicated team hue that appears in a consistent place on every unit, sometimes reinforced by team-coloured health bars or bases.

### Cited Findings
- **Wargroove:**
  - Units are drawn in a "red team" source palette with three ranges: faction colour, skin tone and fixed colours.
  - A build step stores each pixel's palette index as a grey value (index 2 → #020202), and a shader maps the indices to the active faction or skin palette at draw time.
  - The colours on the greyscale example were adjusted for readability.
  - The approach saves VRAM and bandwidth and allows several players on the same faction in multiplayer. Each faction has a default colour (Cherrystone is red), and colour-blind palettes were being investigated.
  - — [Wargroove dev blog, "Palette Swapping"](https://wargroove.com/palette-swapping/)
- **Battle for Wesnoth:**
  - Artists paint the team-coloured region in a specific, reserved set of magenta values. The engine shifts only those exact pixels to the team hue, which "will completely overrun the saturation of the original color," while luminosity is kept relative to a median value.
  - Units should be designed around "some costume element on the unit which can be designated as the team-colored piece of their outfit."
  - — [Wesnoth wiki, Team Color Shifting](https://wiki.wesnoth.org/Team_Color_Shifting)
  - Magenta is used because it "largely has no use as a normal color for general unit graphics" — [Wesnoth wiki, Choosing your palette](https://wiki.wesnoth.org/Choosing_your_palette) (search summary)
- **Lookup tables:** for modern PNG art without an indexed palette, a lookup table can swap "any color we want during the rendering" — [Geefest Tactics devlog, "Palette Shifting"](https://stales.itch.io/geefesttactics/devlog/401748/palette-shifting) (search summary)
- **Age of Empires** players debated whether saturated team colours hurt visibility. One request asked for the opposing team to be filled automatically with blueish variants so enemies stay distinct from allies — [AoE forums, "Revert the unit saturation/brightness"](https://forums.ageofempires.com/t/revert-the-unit-saturation-brightness/134192.md) and [AoE forums, additional team colours](https://forums.ageofempires.com/t/feature-request-additional-team-colors-and-color-settings/289337) (forums; search summary)
- **Warcraft III:** team-coloured health bars help players tell their units from enemies in mirror matches — [Blizzard Warcraft III forums](https://us.forums.blizzard.com/en/warcraft3/t/change-the-enemy-health-bars/801) (forum; search summary, exact thread attribution not verified)
- **Tempest Tower:** "It's important that the player can easily tell the difference between enemy types so they can engage with them properly." The team uses bolder colours and contrast on small enemies' standout features — [Unity blog](https://unity.com/blog/half-past-yellow-creating-enemy-visuals-tempest-tower)

### Inferences
- A TD has only two sides, so full palette swapping is unnecessary. Use two fixed accent hues instead, both kept out of the environment palette:
  - **Player structures:** a cool or warm "friendly" accent, such as cyan-teal, or amber that matches lamp light.
  - **Enemy drones:** a contrasting "hostile" accent, such as magenta-red or acid green, used on emissive parts (eyes, thrusters, cores).
  - Give each accent a small, consistent area, for example a 2–4 px light panel or eye. Keep bodies in shared neutral greys and gunmetal so everything still looks like one world.
- **Pair hue with shape.** Friendly structures sit on the grid with square or diamond bases. Enemies fly, are rounded, and have glowing eyes. This helps colour-blind players. Red/green should never be the only friend-foe channel.
- **Outlines.** A dark outline on everything, plus a selective coloured outline or rim only on enemies (an enemy-hue 1 px rim on the side facing away from lamp light), separates foes without recolouring their bodies.
- **Health bar colours should follow team colour.** Friendly bars in the friendly accent, enemy bars in the hostile accent or red, matching the Warcraft III practice.

### Gaps
- I found no primary designer source with rules on what percentage of a sprite should be team colour. Wesnoth only says "some costume element."
- I didn't retrieve Advance Wars or Wargroove-specific guidance on how they keep blue vs red armies cohesive on one map.

---

## 5. Board overlays: tile highlights, ranges, telegraphs, placement ghosts, selection and hover

### Takeaway
The tactics-genre conventions are:
- Tile-fill highlights for where you can move.
- A second channel, usually outline or a different colour, for where you can attack.
- Explicit, deterministic previews of enemy intent.
- Green/red ghosts for placement validity.

Into the Breach is the strongest example of putting clarity above everything. The team says it would "sacrifice cool ideas for the sake of clarity every time," shows every enemy attack before it lands, and replaced text weapon descriptions with animated previews.

### Cited Findings
- **Into the Breach, design principle (Justin Ma, via Game Developer's write-up of a Rock Paper Shotgun interview):**
  - "As a game design principle, we would sacrifice cool ideas for the sake of clarity every time." and "If it can be shown in a clear way, [then] yes, we can have that."
  - Weapons and firing patterns that were hard to convey to playtesters were cut.
  - — [Game Developer](https://www.gamedeveloper.com/design/-i-into-the-breach-i-dev-on-ui-design-sacrifice-cool-ideas-for-the-sake-of-clarity-every-time-) (original: RPS, https://www.rockpapershotgun.com/2018/03/05/into-the-breach-interface-design/, not fetchable)
- **Into the Breach, telegraph iteration:** early builds filled the map with damage indicators. The designers understood them, but new players needed clarity: "we needed to make it very clear who was doing what." — [Game Developer](https://www.gamedeveloper.com/design/-i-into-the-breach-i-dev-on-ui-design-sacrifice-cool-ideas-for-the-sake-of-clarity-every-time-)
- **Into the Breach, animated tooltips:** text weapon descriptions confused playtesters ("It was so complex to describe some of these weapons"). The animated tooltip, which demonstrates the weapon using the player's own units, was called the most important UI decision: "showing that little animation of them moving is a thousand times more effective." — [Game Developer](https://www.gamedeveloper.com/design/-i-into-the-breach-i-dev-on-ui-design-sacrifice-cool-ideas-for-the-sake-of-clarity-every-time-)
- **Into the Breach, board language:**
  - Every Vek attack is shown before you act, with arrows marking exactly which tiles will take damage — [Game Developer, "Reimagining failure in Into the Breach"](https://www.gamedeveloper.com/design/reimagining-failure-in-strategy-game-design-in-i-into-the-breach-i-) / reviews (search summary)
  - "Cracked earth with red arrows" marks an enemy's next target tile, and blocking that tile with a unit stops an emerging Vek and deals it 1 damage.
  - Upcoming environmental events are marked in red (for example a rocket launch to defend) or yellow (for example an air strike or electrical discharge). Yellow tile icons match an Air Support indicator in the top-right corner.
  - Attacks show icons for push, damage, or both.
  - — [Gamepressure ITB combat guide](https://www.gamepressure.com/into-the-breach/combat/z1aa78)
  - Players can check the order in which events resolve (environment hazards, lava, lightning, collapsing tiles) and each enemy's move range. Pushing an enemy one tile redirects its telegraphed attack. Abilities are checkable at any time through animated explanation — [Kotaku review](https://kotaku.com/into-the-breach-the-kotaku-review-1823365331)
  - Every map fits on the screen at once — [Shacknews preview](https://shacknews.com/article/99108/into-the-breach-preview-ftls-creators-pit-mechs-against-aliens-in-post-apocalyptic-warfare) (search summary)
  - Subset reportedly spent about half of the four-year development cycle on UI — [WhatCulture](https://whatculture.com/gaming/10-best-video-game-user-interfaces?page=10) (secondhand, treat with caution)
- **Into the Breach, GDC:** Matthew Davis gave "'Into the Breach' Design Postmortem" at GDC 2019, covering cut content, iteration and difficulty. One listing mentions a segment on UI-guided design — [GDC Vault](https://www.gdcvault.com/play/1026333/contactUs), [80.lv announcement](https://80.lv/articles/gdc-2019-an-inside-look-at-into-the-breach). I could not access the video or a transcript.
- **XCOM (Enemy Unknown and XCOM 2):** after selecting a soldier, blue-outlined tiles are a one-action move and yellow tiles are the two-action dash. A red "eye" icon warns that the destination is watched (overwatch) — [Pixel Twelve XCOM 2 guide](https://pixeltwelve.com/guides/xcom-2-beginner-guide-first-mission-cover-overwatch), [Pixel Twelve XCOM EU move/dash guide](https://pixeltwelve.com/guides/xcom-enemy-unknown-move-dash-cover), [UFOpaedia Movement (EU2012)](https://new.ufopaedia.org/Movement_(EU2012)) (fan guides; no Firaxis design-intent source found)
- **Advance Wars:** selecting a unit highlights its movement range. In Days of Ruin, holding B (or holding the stylus on a unit) shows its attack range, including enemy units' ranges, and this works after moving — [StrategyWiki, Advance Wars: Days of Ruin gameplay](https://strategywiki.org/wiki/Advance_Wars:_Days_of_Ruin/Gameplay)
- **Indie tactics, dual channel:**
  - Quest for Dominion shows movement on the tile fill and attack range on a separate tile outline (a second material slot), so both can display at once. An enemy-occupied tile in range turns red — [Quest for Dominion devlog](https://quest-for-dominion.itch.io/quest-for-dominion/devlog/643140/movement-and-attack-indicators)
  - Shardpunk uses a green area for "move and still act" and a red area for a move that ends the turn. The developer notes it was unsure the scheme would read until the final floor art was in place — [Shardpunk devlog](https://bryqu.itch.io/shardpunk/devlog/56204/movement-range-display)
  - Fell Desert adds a toggle to highlight enemy range — [Fell Desert devlog](https://chadius.itch.io/fell-desert/devlog/787410/highlight-enemy-range) (title only)
- **Placement ghosts:**
  - The common convention is to swap the ghost to a green material on a valid spot and a red one when blocked — [StudyRaid, material swapping for placement preview](https://app.studyraid.com/en/read/97925/4407101/material-swapping-for-placement-preview)
  - Invalid states can also use opacity or outline changes so colour isn't the only signal — [StudyRaid, placement ghost previews](https://app.studyraid.com/en/read/100582/4504713/visualizing-placement-ghost-previews)
  - One builder spec reserves red for collision or missing support, orange for permission-blocked placement, and adds a pulse animation plus grid and rotation snapping — [builder-ux skill spec](https://skill-search-ten.vercel.app/skills/294de8cea9b86e1f6fdaf6fbaa6593b6bfdf9a82)
  - Validity is recomputed every frame as the cursor moves — [Unreal forums](https://forums.unrealengine.com/t/actor-placement-in-realtime/1080984)
  - Players explicitly ask for red/green because "many other games use red and green" — [itch.io player feedback](https://itch.io/post/1859333)
  - These are all tutorial or low-authority sources describing a widely observed convention.
- **Tower defense range previews:**
  - Players ask for "some range indication circle when you click on a tower, or hover over it." Developers raise "the visuals getting too busy" as the counter-concern — [itch.io comments](https://itch.io/post/3775023) (search summary; exact post not verified)
  - JusTD added range display to the placement ghost: "when building a turret, now you can see its range before you actually place it" — [JusTD devlog](https://fairytalesgames.itch.io/jastd/devlog/817232/range-indicator-added-to-placement-indicator)
  - Age of Empires players requested the same feature (tower range while placing) — [AoE forums](https://forums.ageofempires.com/t/feature-show-tower-range-while-placing-it/105998)

### Inferences
- **Layer order for the TD board, bottom to top:**
  1. Ground tiles.
  2. Persistent coverage overlays: lamp light-pools and an optional "turret coverage" heatmap toggle.
  3. Hover tile (a 1 px bright diamond outline).
  4. Placement ghost and footprint tint.
  5. Range ring of the ghost or selected tower.
  6. Unit shadows.
  7. Units and buildings in depth-sorted order.
  8. Enemy telegraphs: target lines or lock-on markers, drawn above units because they are urgent.
  9. Health bars and status icons.
  10. Cursor.
- **Two channels, à la Quest for Dominion and XCOM.** Use a fill (low alpha, about 20–35%) for area-type info such as range coverage or valid footprint. Use an outline (1 px, full alpha) for the single focused tile or the range boundary. Don't use the same channel for two meanings.
- **Placement ghost.** Draw the building sprite at about 50–60% alpha (or as a flat silhouette) over a footprint diamond tinted green when valid, red when invalid. Add a second non-colour cue for invalid placements, such as a ✕ glyph, diagonal hatching, or a jitter or pulse. Show the range ring of the ghost during placement, since JusTD and AoE players asked for exactly this. For refineries and lamps, show their effect area during placement: the lamp's light radius, and the refinery's resource tiles.
- **Telegraphs (Into the Breach lesson).** Show the drones' intent, not just their position:
  - A thin dotted path or arrow from each wave spawn toward the base before the wave starts.
  - An icon at the spawn edge giving count and type.
  - For drones that fire or dive, a target marker on the targeted tower tile one beat before impact (cracked-tile or red reticle style).
  - Keep the telegraph colour reserved: one hostile hue used only for enemy intent.
- **Selection vs hover.** Hover is a thin outline and selection is a thicker or animated outline (marching-ants or pulse) plus the range ring. Avoid lighting full tiles for hover on a dense board, because it competes with lamp pools.
- **Restraint.** Show range rings only for the hovered, selected or ghost tower. Offer a hotkey to show all coverage. This answers the "visuals getting too busy" concern.

### Gaps
- **FFT and Tactics Ogre:** I found no fetchable source documenting their exact tile-highlight colours. My recollection, unverified, is that FFT uses blue movement tiles and differently coloured attack or effect areas. Check against Game UI Database (gameuidatabase.com) screenshots.
- **Advance Wars range colours:** StrategyWiki confirms highlighting but not the colours. I could not verify the commonly recalled blue (move) and red (attack) scheme.
- **Defense Grid and Kingdom Rush:** no developer or UI source found for their range indicators, placement validity or overlays. Searches returned only unrelated forum posts.
- **Firaxis on XCOM UI intent:** no talk or interview found. Only fan guides describe the colours.
- **Into the Breach GDC 2019 postmortem:** content not accessible. The specific UI-guided-design segment is unverified.

---

## 6. Range display on a diamond grid: circle vs tile-count radius (ellipse vs diamond vs tile set)

### Takeaway
Keep range logic in grid or world space and draw it in screen space. A Euclidean world-space radius projects onto a 2:1 iso screen as an axis-aligned 2:1 ellipse. A tile-count range should be drawn as the actual set of qualifying tiles, a stepped diamond-ish blob, ideally with an outline around the set. Developers who drew iso circles, or computed hits in screen space, report mismatches between what the player sees and the real range.

### Cited Findings
- **Range differs by axis:** a Backyard Monsters analysis found that tower range differs horizontally and vertically on its diamond-grid iso view, so a simple circle is misleading — [The BYM Blog, "True range of towers"](https://thebymblog.blogspot.com/2012/06/true-range-of-towers.html) (search summary, page not fetched)
- **Iso tower defense with ellipse indicators:** one iso TD dev used an ellipse for the range indicator because the projected view meant "it was not technically a perfect circle," then struggled because they were calculating ellipse intersections with 2D screen positions, "which needlessly complicated everything." — itch.io dev comment (search summary; exact post among [itch.io/post/4475509](https://itch.io/post/4475509) / [itch.io/post/4474995](https://itch.io/post/4474995) not verified)
- **Bloons TD:** tower range is "visually displayed as a circle around it, where the radius of the circle is equal to its range in distance units". Some cosmetic circles don't represent a real attack — [Bloons Wiki, Range](https://www.bloonswiki.com/Range) (search summary)
- **Red Blob Games, circle drawing on grids:**
  - Mark a tile if its distance to the centre is less than the radius, comparing squared distances to skip the square root.
  - Circles "look nicer when the radius is 1.5, 2.5, 3.5, etc. than when it's 1.0, 2.0, 3.0". The goal is that "the sides of the circle should be at the boundary between tiles, not in the middle of a tile."
  - Integer-only math works by comparing `4 * distance_squared` against `diameter*diameter`.
  - Outlines need eight-way symmetric stepping (the simple row method "only works reliably when the triangle's angle is ≤45°").
  - — [Red Blob Games, Circle drawing on a grid](https://www.redblobgames.com/grids/circle-drawing/)
- **Flat 2D approach:** pin a dotted circle of the range diameter on the tower's origin point. Notably, this doesn't account for iso projection — [Construct forum](https://construct.net/en/forum/construct-2/how-do-i-18/show-tower-rangemethod-127329) (search summary)

### Inferences
These are derived from the standard 2:1 iso projection, `screen_x = (gx − gy)·W/2`, `screen_y = (gx + gy)·H/2` with tile width W and height H = W/2. The geometry is straightforward and verifiable, but no single source states it.
- **Euclidean range of r tiles → screen ellipse.** It is axis-aligned, with horizontal semi-axis r·W/√2 ≈ 0.707·r·W and vertical semi-axis r·H/√2 ≈ 0.354·r·W, so exactly 2:1. Example: W = 32 px and r = 3 tiles gives an ellipse of about 68 × 34 px semi-axes (136 × 68 px overall). Draw it with `ctx.ellipse(cx, cy, r*W/Math.SQRT2, r*H/Math.SQRT2, 0, 0, 2*Math.PI)` and snap the stroke to whole pixels.
- **Manhattan range (|dx|+|dy| ≤ r, "tile steps") → screen rectangle.** The grid-space diamond's vertices map to (±rW/2, ±rH/2), so it shows as an axis-aligned, stair-stepped rectangle about rW wide and rH tall. This is counter-intuitive for players, so draw the tile set rather than a smooth shape.
- **Chebyshev range (max(|dx|,|dy|) ≤ r, "king moves") → screen diamond.** A grid-space square appears as a 2:1 diamond with vertices at (0, ±rH) and (±rW, 0). A clean diamond ring therefore honestly represents a Chebyshev range, not a circular one.
- **Recommendations by entity:**
  - **Turrets and missile launchers** use continuous targeting of free-flying drones, so use a Euclidean world-space radius, drawn as a 2:1 ellipse ring (1 px stroke, plus an optional 15–25% alpha fill). Hit tests use world coordinates, never screen ellipses.
  - **Lamps** light an area, so match the visual light-pool shape (also a 2:1 ellipse) to the gameplay radius exactly. The light itself then works as the range indicator, which suits the theme.
  - **Refineries and any tile-based effects** (adjacency bonuses, build radius) use a tile-count range. Highlight the qualifying tile diamonds, with fill plus a traced outer border, so players see exactly which tiles count.
  - Use half-integer radii (2.5, 3.5) when converting a Euclidean range to a tile set (per Red Blob), so the boundary falls between tiles and the drawn ellipse and tile set agree visually.
- **Elevation.** If the board has height, draw range ellipses at ground level, not at turret-head height, so the boundary lines up with the tiles drones cross.

### Gaps
- I found no AAA or published iso TD (Kingdom Rush, Defense Grid) documentation of whether their range rings are true circles, ellipses or tile sets, or how they handle height.
- I couldn't fetch the Backyard Monsters analysis to get its actual measurements.

---

## 7. Health bars and status icons in iso

### Takeaway
RTS convention is to make health-bar visibility a context or option setting rather than always on: show them for damaged, selected or targeted units, or always. Team-coloured bars help separate friend from foe. Into the Breach keeps per-unit HP compact on the board and puts global status (grid power) in a separate HUD meter. I found no sourced pixel sizes.

### Cited Findings
- **StarCraft II health-bar modes:** Normal, Targeting, Selected, Damaged, Always — [Blizzard SC2 forum](https://us.forums.blizzard.com/en/sc2/t/starcraft-ii-heroes-and-units-health-bars/27931). "Always" shows the health of every unit, while "Selected" shows only selected units — [Gaming Q&A (2011)](https://gamingqna.blogspot.com/2011/12/how-do-i-have-health-bars-displayed-at.html). Players use "Damaged" to see bars only on damaged units — [TL.net thread](https://tl.net/forum/tech-support/561839-damaged-only-health-bars-disappearing-in-replays) (forums)
- **Age of Empires** players request a "healthbars always on" mode that is really always on, which shows contextual hiding is the default and can frustrate players — [AoE forums](https://forums.ageofempires.com/t/make-healthbars-always-on-really-always-on/195949) (search summary)
- **Warcraft III:** team-coloured health bars help tell your units from the enemy's in mirror matches — [Blizzard WC3 forums](https://us.forums.blizzard.com/en/warcraft3/t/change-the-enemy-health-bars/801) (search summary, attribution not verified)
- **Into the Breach:** a bright orange power-grid meter in the upper left is the persistent "health" of the run, separate from units. Attack icons show push and damage effects. Civilian buildings carry a one-hit shield — [Kotaku review](https://kotaku.com/into-the-breach-the-kotaku-review-1823365331), [Gamepressure combat guide](https://www.gamepressure.com/into-the-breach/combat/z1aa78)

### Inferences
- **Placement.** Anchor bars to the unit's ground point, not the sprite's bounding box, then offset upward by a fixed amount per unit type (or per hover height for drones). Bars then don't jitter with animation frames, and tall sprites don't push bars into the row behind. Put enemy bars above the sprite and consider friendly building bars at the footprint's bottom edge so the two don't stack on a crowded tile. This split is an inference, not a sourced convention.
- **Size at 16–32 px units.** Make the bar 2–3 px tall with a 1 px dark border, and about 60–100% of the tile width (for example 16–24 px on a 32 px tile). Use segmented pips for low-HP units, Into-the-Breach style, and a continuous bar for high-HP buildings and the base. These are practitioner numbers, not sourced.
- **Clutter control (SC2 "Damaged" default).** Hide bars at full HP. Show them on damage, fade them out about 2–3 s after the last hit, and always show them for selected or hovered units. Keep the base's HP in the HUD, as with ITB's grid meter, rather than as a world-space bar.
- **Status icons.** Use at most one 5–7 px icon slot beside the bar, with a dark backing (for example "unpowered", "unlit" or "low ammo"). Stacked statuses become a count badge. For buildings, prefer tinting or animating the sprite (an unpowered building dims its lights) over adding icons. That is especially natural in a lighting-themed game.
- **Colour.** Friendly bars in the friendly accent and enemy bars in the hostile hue. Avoid relying on red-to-green gradients alone to show HP fraction.

### Gaps
- No primary source found for pixel dimensions, offset conventions or fade timings for health bars in any iso tactics or TD game.
- No source found on how Kingdom Rush or Defense Grid place enemy health bars.

---

## 8. Darkness and lighting: keeping units and threats readable in the dark

### Takeaway
Games built around darkness make light a gameplay resource: Darkest Dungeon's 0–100 torch meter and Don't Starve's Charlie, who attacks only in total darkness. Threats must still read when they sit outside the light, which is solved with emissive elements (glowing eyes, cores), artist-controlled rim lights, and a strict "most important = most emissive" hierarchy. Primary sources on They Are Billions, Frostpunk and Diablo darkness readability were not found.

### Cited Findings
- **Darkest Dungeon light level:** a value from 0 to 100, divided into stages topped by "Radiant Light" — [Gamepressure, DD light level](https://www.gamepressure.com/darkestdungeon/light-level-and-its-influence/z28424). Community-reported thresholds are 0/25/50/75%, and hovering the torch shows the current effects. Lower light raises the chance of being surprised, stress, enemy crit chance and loot — [Steam DD discussions](https://steamcommunity.com/app/262060/discussions/0/594020645016182064) (community reports)
- **Darkest Dungeon art (Chris Bourassa):** the hard edges reflect the uncompromising game, "the pooling blacks are a nod to the light mechanic," and empty eyes convey hopelessness. Mike Mignola was a key influence because his work was "simple and dark" — [Kotaku, "The Art of Darkest Dungeon"](https://kotaku.com/the-art-of-darkest-dungeon-1798520287) / [GameSpot, "The Gothic Sensibilities of Darkest Dungeon"](https://www.gamespot.com/articles/the-gothic-sensibilities-of-darkest-dungeon/1100-6424880/) (search summary; which outlet carries which quote not verified)
- **Don't Starve:**
  - Charlie is an unseen damage source that attacks only when the character is in complete darkness — [Don't Starve wiki, Charlie](https://dontstarve.wiki.gg/wiki/Charlie)
  - Light lowers sanity drain at night and protects against Charlie. The wiki lists about 50 sanity/min drained in darkness vs about 5/min near light — [Don't Starve wiki, All About Night](https://dontstarve.wiki.gg/wiki/Guides/All_About_Night), [Light sources](https://dontstarve.wiki.gg/wiki/Light_sources)
  - Winona's Spotlight lights a 2.5-tile radius instead of the normal 1.5 tiles, with an unchanged 6-tile range — [DST wiki, Spacious Spotlight](https://dontstarve.wiki.gg/wiki/Spacious_Spotlight) (search summary)
  - This shows light radius as an explicit, tile-measured gameplay quantity.
- **Emissive readability (From Software, per a designer's analysis):** interactables must stay legible regardless of lighting, since lighting quality can't be relied on across platforms. Collectibles use a light emissive shader plus a little bloom so they read under any lighting — [Game Designer Notes, "Elden Ring bloom"](https://www.patreon.com/GameDesignerNotes/posts/elden-ring-bloom-143530240) (search summary; secondary analysis)
- **Emissive hierarchy:** a dev comment on a game with background creatures reports that bright emissive creatures competed with gameplay decals, and suggests the most important elements should be the most emissive ("Decals should always be more emissive than the creatures") — itch.io comment (search summary; exact post not verified)
- **Rim light for silhouettes (Tempest Tower):** MatCap-based rim lighting is used "to create an artist-controlled rimlight effect, helping to define enemy silhouettes" and "to help enemies stand out regardless of the main light source." Emission on pustules and tentacles makes those parts look self-lit. The team names "How do we keep our enemies visible and distinct against entirely new backdrops and lighting conditions?" as an open problem — [Unity blog](https://unity.com/blog/half-past-yellow-creating-enemy-visuals-tempest-tower)
- **Light radius vs enemy visibility:** a player reviewing an indie game noted a small light area made enemies hard to see — itch.io review comment (search summary; exact post not verified)
- **Glowing eyes** are a long-standing, recognised visual concept for marking creatures in darkness — [Giant Bomb, Glowing Eyes concept](https://giantbomb.com/wiki/Concepts/Glowing_Eyes) (concept wiki; search summary)

### Inferences
- **Reserve emissive pixels for meaning.** In a dark TD the darkness is the default, so whatever glows gets attention. A suggested hierarchy, brightest first:
  1. Enemy eyes, cores and thrusters in the hostile hue.
  2. Enemy telegraphs.
  3. Player turret muzzle flashes and projectiles.
  4. Lamp light-pools.
  5. Friendly status lights in the friendly hue.
  6. Unlit environment.
- **Drones must never fully disappear outside lamp light.** Give every drone a 1–4 px emissive "eye" or core that ignores lighting, drawn additively or full-bright, plus a faint additive glow sprite that brightens as the drone nears the base. Lamp light then reveals body detail and silhouette, but the threat is always locatable. That mirrors the Gylt, From Software and Tempest Tower approaches.
- **Rim lighting.** Pixel-paint a 1 px rim on drones and buildings in a mid-value or enemy-hue tone. Alternatively, apply a runtime light-facing rim (brighten edge pixels on the side facing the nearest lamp). Either keeps silhouettes intact in low light, the 2D analogue of Tempest Tower's MatCap rim.
- **Lamps as gameplay light radius, Don't Starve-style.** Make lamp radius an explicit tile-measured value, and draw the pool as the same 2:1 ellipse as the gameplay radius (see section 6). If being lit matters (for example turrets can only target lit drones), lit vs unlit must be unmistakable. Use a crisp pixel-stepped edge or a 1 px boundary on the pool, not a soft gradient that hides the cutoff.
- **Don't darken UI overlays.** Range rings, placement ghosts, health bars and telegraphs should render after the lighting or darkness pass so they stay readable whatever the light level.
- **Value cap.** Keep unlit ground tiles in a very narrow, low value band and lit ground in a moderate band. Drones stay readable against both because their emissive accents sit above both bands.

### Gaps
- **They Are Billions:** no source found on darkness or night readability. The searches only gave general art reviews, such as "all of your units are easy to identify with a passing glance" — [PlayStation LifeStyle review](https://www.playstationlifestyle.net/?p=789257) (search summary).
- **Frostpunk, Diablo (light radius) and Darkest Dungeon's on-screen darkness rendering:** no primary developer sources retrieved. Diablo's light radius and Frostpunk's heat or light overlays would be good follow-ups.
- **Gylt glowing eyes:** described only through a search summary of a developer comment. I couldn't identify or verify the original post.
