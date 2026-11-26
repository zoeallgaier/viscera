# Isometric Projection, Tile-Grid and Footprint Conventions in Pixel-Art Games

Scope: 2D pixel-art isometric/dimetric games, 1980s to now. The notes are meant to help a developer pick one grid for a small browser tower-defense game (base, turrets, missile launchers, refineries, lamps, resource pools, drones) that is moving off a hex grid.

Method note: the web research hit its tool budget. Several classic sources were unreachable: the GameDev.net "Isometric 'n' Hexagonal Maps" article (r747) returned HTTP 403, web.archive.org was blocked, and one itch.io devlog returned 404. To check seam behaviour, I wrote a small Python script that tiles each candidate diamond shape on a 2:1 grid and counts gap and overlap pixels. Its results are labelled "(seam script)". They come from my own computation, not from a published source. All formula derivations are labelled as inferences.

---

## 1. Projection choice: 2:1 "pixel isometric" dimetric (≈26.565°) vs true isometric (30°) vs other ratios

### Takeaway
Almost every pixel-art "isometric" game actually uses 2:1 dimetric projection: lines run 2 px across for every 1 px down, at arctan(1/2) ≈ 26.565°. A true 30° isometric line has a run:rise of about 1.732:1. No small whole-number pixel pattern matches that, so it draws as an uneven, jaggy staircase. The 2:1 projection is what you get from an orthographic camera tilted to 30° elevation and rotated 45°. Pick 2:1.

### Cited Findings
- The dimetric form used in games most commonly has "a 2:1 pixel ratio"; axis lines drawn with "a 2:1 pixel pattern ratio" sit at "≈26.565° (arctan(1/2))" to the horizontal — [Wikipedia: Isometric video game graphics](https://en.wikipedia.org/wiki/Isometric_video_game_graphics)
- True isometric uses a camera angle of ≈35.264° (arctan(sin 45°)). Its axis lines sit at 30° and form "a pixel ratio of ≈1.732:1". The three axes are 120° apart — [Wikipedia](https://en.wikipedia.org/wiki/Isometric_video_game_graphics)
- The 2:1 form "is more accurately described as a variation of dimetric projection", with axis angles of ≈116.565°, ≈116.565° and ≈126.870°. The term "isometric perspective" is "frequently misapplied to any game with an angled, overhead view" — [Wikipedia](https://en.wikipedia.org/wiki/Isometric_video_game_graphics)
- Why 2:1 won: lines at 30° "would not follow a neat pixel pattern". Early hardware also lacked the colours and CPU for anti-aliasing, so the 2:1 pattern was used, and it "allows smoother lines than in the isometric one" — [Wikipedia](https://en.wikipedia.org/wiki/Isometric_video_game_graphics)
- Pixel-art practitioner view: the 2:1 "2-step" line is "closer to 26.5º", and "a true 30º degree line in pixel art results in a jaggy rhythm", so isometric pixel art uses 2:1 as a compromise. Advice: "Don't trust your eyes". Count pixels, and use a 2:1 ruler layer to check that elements fall on 2-step lines — [Slynyrd, Pixelblog 41: Isometric Pixel Art](https://www.slynyrd.com/blog/2022/11/28/pixelblog-41-isometric-pixel-art)
- Render-pipeline recipe for 2:1 (Flare engine author): an orthographic camera at RotX 60°, RotY 0°, RotZ 45° (that is, 30° elevation and 45° azimuth). He says explicitly that this is not true engineering isometric but "a slightly different projection" chosen for cleaner diagonals. The base tile should be "exactly 2x as wide as it [is] tall" — [Clint Bellanger, Isometric Tiles](https://clintbellanger.net/articles/isometric_tiles/)
- Wikipedia's figure caption gives the same geometry: arctan(sin 30°) ≈ 26.565°, which forms "a 2:1 pixel ratio". So a 30° camera elevation produces 2:1 lines — [Wikipedia](https://en.wikipedia.org/wiki/Isometric_video_game_graphics)
- Other projections in shipped games: Ultima Online (1997) and Divine Divinity (2002) are classed as oblique. Fallout (1997) and SimCity 4 (2003) are trimetric. SimCity 2000, X-COM (1994), Syndicate (1993), Populous (1989), Diablo (1996) and Tactics Ogre (1995) are cited as fixed isometric (meaning 2:1 dimetric). Final Fantasy Tactics (1997) used 3D polygon maps with a rotatable camera and 2D sprites — [Wikipedia](https://en.wikipedia.org/wiki/Isometric_video_game_graphics)
- Ultima Online's land tiles are 44×44 px diamonds whose rows widen "from 2 to 44 and back to 2". That is 2 px of growth per row, or 1 px per side, which makes 45° edges in a 1:1 diamond rather than a 2:1 one — [uo-rust-libs Tile docs](https://docs.rs/uo-rust-libs/0.1.0/uorustlibs/art/struct.Tile.html); land images are "fixed size at 44x44" — [wpdev UO art format](https://wpdev.sourceforge.net/docs/formats/vbasic6/art.html)

### Inferences
- Geometry (derived): with an orthographic camera at azimuth 45° and elevation φ, a ground square of side L projects to a diamond √2·L wide and √2·L·sin φ tall. At φ = 30°, sin φ = 0.5, which gives exactly 2:1. A vertical edge of length L projects to L·cos φ = 0.866·L. Relative to tile width W, a true cube is therefore 0.612·W tall: 39.2 px on a 64-wide tile, 19.6 px on a 32-wide tile. Since 39.2 is not a whole number, pixel games choose their own height unit (see Q4).
- 30° (1.732:1) has no short repeating integer pattern. Approximations like 7:4 or 5:3 give irregular step lengths, which is the jaggy rhythm Slynyrd describes. A 2:1 line is a perfectly regular "2 px, step, 2 px" pattern, mirrors cleanly, and keeps tile maths in whole numbers (tile half-width = 2 × half-height).
- Other ratios: 1:1 (45°, Ultima Online style) gives square-ish diamonds that show the ground with less foreshortening, but walls and objects look tall and oblique. 3:1 (≈18.4°) or 4:1 flatten the ground so much that north–south depth is hard to read. For a tower-defense game, the player has to read lanes and ranges in both ground directions. 2:1 is the best-supported compromise, and every engine, tool and art reference assumes it.
- For a browser game: 2:1 also scales cleanly at integer zoom (×2, ×3) with `image-rendering: pixelated`. Any non-integer ratio blurs or shimmers edges at any scale.

### Gaps
- I found no developer postmortem that explains why a specific title chose 2:1 over 30°. The rationale above comes from encyclopedic and tutorial sources, not from first-hand 1990s developers.

---

## 2. Tile sizes used, and why (32×16, 64×32, 64×31, the 2px-tip convention); per-game numbers

### Takeaway
The tile families are 32×16 (small, 8/16-bit era and X-COM) and 64×32 (most 1990s PC games and modern engines). The artwork is often one row or column short (64×31 or 62×32) so neighbouring tiles tessellate exactly. Some engines use odd sizes such as AoE2's 97×49, which share a 1 px outline with each neighbour. For this project, a 64×32 grid is the best-documented choice.

### Cited Findings
**Per-game numbers (verified where linked):**
- **Transport Tycoon Deluxe / OpenTTD:** at normal zoom, a ground tile is 64 px wide and 31 px high. Zoomed-out variants scale down from that — [NewGRF specs: RealSprites](https://newgrf-specs.tt-wiki.net/wiki/RealSprites). OpenTTD source: `TILE_SIZE = 16` (world units per tile), `TILE_PIXELS = 32` ("pixel spacing between tile columns and rows at the base zoom level"), `TILE_HEIGHT = 8` — [OpenTTD source docs, tile_type.h](https://docs.openttd.org/source/d0/ddf/tile__type_8h_source.html). A community thread warns that upscaled replacement sprites can come out "3 pixels too low" against the 64×31 layout — [tt-forums: Adapting EZ 32bpp ground sprites](https://www.tt-forums.net/viewtopic.php?p=1003247)
- **X-COM: UFO Defense (1994):** Battlescape terrain and unit sprites are 32×40 px, and BIGOBS.PCK sprites are 32×48 — [UFOpaedia: Image Formats](https://www.ufopaedia.org/index.php/Image_Formats). OpenXcom's renderer moves `_spriteWidth/2` horizontally and `_spriteWidth/4` vertically per tile step: `screenPos->x = mapPos.x * (_spriteWidth / 2) - mapPos.y * (_spriteWidth / 2); screenPos->y = mapPos.x * (_spriteWidth / 4) + mapPos.y * (_spriteWidth / 4) - mapPos.z * ((_spriteHeight + _spriteWidth / 4) / 2);` — [OpenXcom Camera.cpp](https://github.com/OpenXcom/OpenXcom/blob/master/src/Battlescape/Camera.cpp). With 32×40 sprites, that is a step of 16 px × 8 px, so the floor diamond is 32×16.
- **Ultima VIII:** a ground tile of 32×32 in 3D "translates to 32x16 in 2D" — [Exult forum: Ultima VIII tile size](https://exult.sourceforge.io/forum/viewtopic.php?p=12210)
- **Ultima Online:** land tiles are 44×44 px 1:1 diamonds with 2 px tips (rows 2→44→2) — [uo-rust-libs](https://docs.rs/uo-rust-libs/0.1.0/uorustlibs/art/struct.Tile.html); [wpdev art format](https://wpdev.sourceforge.net/docs/formats/vbasic6/art.html)
- **Age of Empires II:** terrain tiles are a rhombus image of 97 × 49 px. The openage doc's text says "height = 97px, width = 49px", but its own diagram shows the widest row as 97 px with rows growing by 4 px, so the two values are swapped in the text. The real tile is 97 wide × 49 tall, with a single centre pixel on row 25. Each terrain SLP holds a 10×10 set of tile variants, and frame = (x % 10) + (y % 10) × 10 — [openage terrain docs](https://simonsan.github.io/openage-webdocs/sphinx/doc/media/terrain.html)
- **RollerCoaster Tycoon (OpenRCT2):** a tile is "64 pixels across, 32 pixels high". This is a single forum user's statement — [OpenRCT2 forum: tile dimensions](https://forums.openrct2.org/topic/3562-tile-dimensions)
- **Diablo (1996):** level graphics are built from 32×32 px CEL frames. "Tiles" are 128×192 px (dungeon, catacombs, caves, nest, crypt) or 128×288 px (town, hell) — [d1-file-formats: MIN/TIL](https://github.com/savagesteel/d1-file-formats/blob/master/PC-Mac/MIN-TIL.md). Each tile is subdivided into 4 "dungeon pieces" for rendering — [Boris the Brave: Dungeon generation in Diablo 1](https://boristhebrave.com/2019/07/14/dungeon-generation-in-diablo-1)
- **Flare engine (modern open-source ARPG):** base floor tile 64×32, tall tiles 64×128, and an example 2×3-footprint building rendered at 160×256. The author recommends power-of-2 dimensions "for most cases" — [Clint Bellanger, Isometric Tiles](https://clintbellanger.net/articles/isometric_tiles/). His isometric-math article uses 128×64 tiles in its examples — [Clint Bellanger, Isometric Math](https://clintbellanger.net/articles/isometric_math/)
- **Modern hand-pixelled asset:** Slynyrd chose 36×36 px cube tiles to match his sprites. He advises "even dimensions are your friend" — [Slynyrd Pixelblog 41](https://www.slynyrd.com/blog/2022/11/28/pixelblog-41-isometric-pixel-art)
- **Community conventions:** many 2:1 sets use 64×32, and edge artefacts "usually" mean the tile is placed 1–2 px off — [OpenGameArt comment 796](https://opengameart.org/comment/796). One 32-wide asset is "technically 32x17", with its bottom pixel row meant to overlap the next tile — [OpenGameArt node 99646](https://opengameart.org/node/99646)

**SimCity 2000 (partial):** the terrain has up to 64 altitude levels, and each tile represents about 200×200 ft — [Inside Mac Games, Oct 1993](https://discmaster.textfiles.com/view/2769/TricksOfTheMacGameProgrammingGurus.iso/Information/Inside%20Mac%20Games/9310%20(October%201993)/Inside%20Mac%20Games%20October%2093/Inside%20Mac%20Games%20October%2093.rsrc/TEXT_134.txt). The altitude field covers 50–3150 ft — [djm.cc SimCity 2000 info](https://www.djm.cc/simcity-2000-info.txt). A fan estimate puts the scale at roughly 1 px per metre, so a tile is about 16×16 m, and the same page disputes the 200 ft figure — [DM's Sim City Page](https://www.dangermouse.net/games/simcity/). Sprite headers store per-sprite width and height in pixels — [OpenCity2k SC2k-docs](https://uithub.com/OpenCity2k/SC2k-docs)

### Inferences
- Size families: 32×16 (X-COM, Ultima VIII) suits 320×200 screens. 64×32 (TTD/OpenTTD, RCT, Flare, likely Diablo's dungeon pieces) suits 640×480. AoE2's 97×49 is a hi-res outlier built on a 96×48 step with a 1 px shared outline.
- Diablo (inferred, not verified): a 128-px-wide tile split into 4 dungeon pieces suggests each piece is 64 px wide, a 64×32 floor diamond, with the height going into tall columns built from 32×32 frames.
- **Candidate single-grid spec for the tower-defense game (inference):**
  - Logical tile 64×32, step 32 px across and 16 px down. Draw the floor art as a 64×31 diamond (4 px tips, one widest row) or a 62×32 diamond (2 px tips, two widest rows), both inside a 64×32 cell. Either tessellates with zero gaps and zero overlaps (seam script, Q3).
  - At 64×32, footprints come out at 1×1 = 64×32, 2×2 = 128×64, 3×3 = 192×96 (Q5). That leaves room for readable turret detail.
  - Alternative for a chunkier retro look: 32×16 art (drawn 32×15 or 30×16) shown at ×2 or ×3. Turrets would get only about 32 px of width each, which is tight for showing different turret types.
- Odd-width tiles (AoE2-style 97×49, or 65×33) put a single centre pixel at each tip and require a 1 px overlap with neighbours. That is fine for hand-placed terrain blending but complicates hit-testing and outline drawing. Even widths are easier, which matches Slynyrd's advice.

### Gaps
- **No verified pixel tile sizes found for:** SimCity 2000 (often claimed to be 32×16 at a given zoom level; unverified), original Transport Tycoon (1994) beyond the TTD-derived 64×31 spec, Populous, Syndicate, Tactics Ogre, Into the Breach, Habbo Hotel (a community wall-furni spec exists but gives no floor tile size — [HWFLN v2.0](https://habbianos.github.io/notations/wall-furni-position/spec/HWFLN-v2_0.html)), and Age of Empires I.
- Final Fantasy Tactics used 3D polygon maps ([Wikipedia](https://en.wikipedia.org/wiki/Isometric_video_game_graphics)), so it has no pixel tile size.
- Sprite-ripping sites (The Spriters Resource) could give measured numbers for the missing games, but I did not reach them within the tool budget.

---

## 3. Diamond-tile drawing conventions: odd vs even widths, flat 2px tips, seaming without gaps or overlaps, the 1px-shorter trick

### Takeaway
On a 2:1 grid with step (W/2, H/2), a filled diamond tessellates perfectly only if its pixel area is exactly W·H/2. A "full" 64×32 diamond (4 px tips and a doubled 64 px middle row) is one row too big and overlaps its neighbours by 64 px per tile. The fix is the 1px-shorter tile (64×31, as in TTD/OpenTTD) or the 2px-tip tile (62 px wide, 2 px flat tips, doubled middle row). Both have zero gaps and zero overlaps. Odd-width tiles with 1 px tips (AoE2-style) overlap by a shared 1 px edge.

### Cited Findings
- TTD/OpenTTD ground sprites are 64 px wide by 31 px high, one row less than the nominal 2:1 height of 32 — [NewGRF specs: RealSprites](https://newgrf-specs.tt-wiki.net/wiki/RealSprites)
- Ultima Online land tiles use 2 px flat tips: rows go 2→44→2 — [uo-rust-libs](https://docs.rs/uo-rust-libs/0.1.0/uorustlibs/art/struct.Tile.html)
- AoE2 terrain: 97 px widest row, rows changing by 4 px (2 px per side, a 2:1 slope), and a single marked centre pixel. That is an odd-width tile with 1-px tips — [openage terrain docs](https://simonsan.github.io/openage-webdocs/sphinx/doc/media/terrain.html)
- Overlap convention: a 32-wide tile drawn "technically 32x17", whose bottom row is designed to overlap the tile above so the lines meet — [OpenGameArt node 99646](https://opengameart.org/node/99646)
- Outline only two sides of each tile so the outlines tessellate without doubling — [PixelJoint forum thread](https://pixeljoint.com/Forum/printer_friendly_posts.asp?TID=8018)
- Corner styles: a sharp-cornered cube style ("D1") "follows the 2:1 grid, and tiles perfectly". Wide-cornered styles "require a bit of clever overlapping to tile well". Tiles should loop seamlessly across all four sides of each face — [Slynyrd Pixelblog 41](https://www.slynyrd.com/blog/2022/11/28/pixelblog-41-isometric-pixel-art)
- For rendered or anti-aliased tiles, "it's better for the tile to overflow slightly than to be too small", which "helps prevent translucent seams between tiles". Rendering with Straight Alpha rather than Sky also avoids sky-coloured edge fringes — [Clint Bellanger, Isometric Tiles](https://clintbellanger.net/articles/isometric_tiles/)
- Search-snippet only (the page returned 404 when fetched): an itch.io tileset devlog describes corner runs of 2 px with other runs of 3 px, and seamless joins that depend on exact pixel counts from neighbouring tiles — [itch.io devlog 336797](https://itch.io/devlog/336797/how-to-use-it-seamlessly.amp). Unverified.

### Inferences
- **Seam script results.** Each filled shape was tiled on a 2:1 lattice and gap/overlap pixels were counted in an interior window:

  | Shape (filled) | Bounding box | Row widths | Step | Gaps/tile | Overlap/tile |
  |---|---|---|---|---|---|
  | "Full" diamond, 4 px tips, 2 widest rows | 64×32 | 4,8…64,64…8,4 | 32,16 | 0 | **64 px** (one doubled row) |
  | **1px-shorter**, 4 px tips, 1 widest row | 64×31 | 4,8…60,64,60…4 | 32,16 | **0** | **0** |
  | **2 px flat tips**, 2 widest rows | 62×32 (inside a 64×32 cell) | 2,6…62,62…6,2 | 32,16 | **0** | **0** |
  | 2 px tips, 1 widest row | 62×31 | 2,6…62…6,2 | 32,16 | 62 px | 0 |
  | Odd width, 1 px tip (AoE2-style, scaled) | 65×33 | 1,5…65…5,1 | 32,16 | 0 | 31 px (shared edge) |
  | Small full, 4 px tips | 32×16 | | 16,8 | 0 | 32 px |
  | Small 1px-shorter | 32×15 | | 16,8 | 0 | 0 |

- Rule (derived): each lattice cell holds W·H/2 pixels (2048/2 = 1024 for 64×32). A tile tessellates exactly when its area equals that and its edges follow the 2-px stair. Both 64×31 (4 px tips) and 62×32 (2 px tips) have area 1024. The "full" 64×32 has 1088.
- Which to pick:
  - **64×31 / 4 px tips:** the left and right corners are sharp single rows, and the top and bottom tips are 4 px wide. This is the TTD convention.
  - **62×32 / 2 px tips:** the left and right corners are 2 px tall, the top and bottom tips are 2 px flat, and a 1 px empty column sits at each side of the 64×32 canvas. This is close to the UO style.
  - Both work. Choose by how the tips should look, then keep that shape for every footprint outline, selection highlight, range overlay and hit mask so they all line up.
- Outlines: if each tile draws its own full outline, outlines double up on shared edges (a 2 px line). Either outline only two edges per tile (for example, top-left and top-right, per the PixelJoint advice) or draw grid lines as a separate overlay pass.
- Odd-width tiles: let a shared 1 px edge belong to both neighbours, and accept it as overdraw. This works for opaque terrain but double-draws semi-transparent overlays such as range circles and placement ghosts. Even-width shapes avoid that.
- Browser specifics: draw at integer device-pixel offsets and scale the whole canvas by an integer. Sub-pixel camera positions cause one-pixel shimmer along seams, which looks like the gaps described in the OpenGameArt comment.

### Gaps
- I could not reach a canonical pixel-art tutorial (Pixel Logic book, Lospec, Saint11, Derek Yu) that names the "2px tip" or "1px-shorter" convention in so many words. The table above is my own computed verification. It is consistent with TTD's 64×31 and UO's 2 px tips, but no source states it as a rule.

---

## 4. Height units: pixels per z-level relative to tile width; building, wall and unit heights

### Takeaway
There is no single standard. TTD/OpenTTD uses 8 px per height level on a 64-px tile (1/8 of tile width, half the 16 px row step). X-COM uses 24 px per level on a 32-px tile (3/4 of tile width), on a voxel grid of 16×16×24 per tile. A geometrically true cube on a 2:1 tile is about 0.61×W tall, but pixel games pick an integer height unit chosen for gameplay readability.

### Cited Findings
- **OpenTTD:** `TILE_HEIGHT = 8` is "the height of one height level", the same value in world coordinates and in pixels at base zoom. `TILE_SIZE = 16` world units per tile. `MAX_TILE_HEIGHT = 255` (15 in older builds) — [OpenTTD tile_type.h docs](https://docs.openttd.org/source/d0/ddf/tile__type_8h_source.html); [older patchpack tile_type.h](https://code.theleruby.com/cpp/openttd-patchpack/source/files/26ae4b426f21d6597e3f16b89cdfda3462d8ca08/src/tile_type.h). Community check: in the original TTD slope sprite 3982, the vertical difference for a slope is 8 px — [tt-forums: Height of tile](https://www.tt-forums.net/viewtopic.php?p=762336)
- **X-COM / OpenXcom:** the per-level screen offset is `(_spriteHeight + _spriteWidth / 4) / 2`. Voxel-to-map conversion divides by 16 in x and y and by 24 in z (`voxelPos.x / 16, voxelPos.y / 16, voxelPos.z / 24`). Camera up/down moves by `_spriteHeight * 3 / 5` — [OpenXcom Camera.cpp](https://github.com/OpenXcom/OpenXcom/blob/master/src/Battlescape/Camera.cpp). Eye positions use tile × 16 horizontally and tile z × 24 vertically — [OpenXcom commit](https://www.github.com/MeridianOXC/OpenXcom/commit/1c188b9af2576fd7109bbf8e7c1749f93760f8fb). Per-tile voxel models are "16x16x24" — [OpenXcom forum](https://openxcom.org/forum/index.php/topic,571.0.html). Terrain sprites are 32×40 px, and big objects 32×48 — [UFOpaedia Image Formats](https://www.ufopaedia.org/index.php/Image_Formats).
- **X-COM MCD:** a tile's height offset is relative to its level (usually 0). A unit can step between tiles only if the effective height difference is "8 or less" — [UFOpaedia: MCD](https://ufopaedia.org/index.php?title=MCD)
- **Ultima Online:** tiledata item heights include floors 0, stairs 5 and walls 20. These are z-units, not pixels — [UOX3 tiledata tutorial](https://www.forums.uox3.org/viewtopic.php?p=13550). A ServUO renderer write-up describes raising a neighbour's z showing a ridge and expanding textured tiles "by 4" in one direction. This does not clearly state a pixels-per-z value — [ServUO: how the UO software renderer works](https://www.servuo.dev/threads/how-it-works-uo-software-renderer.1957/)
- **SimCity 2000:** 64 altitude levels — [Inside Mac Games](https://discmaster.textfiles.com/view/2769/TricksOfTheMacGameProgrammingGurus.iso/Information/Inside%20Mac%20Games/9310%20(October%201993)/Inside%20Mac%20Games%20October%2093/Inside%20Mac%20Games%20October%2093.rsrc/TEXT_134.txt); altitudes 50–3150 ft — [djm.cc](https://www.djm.cc/simcity-2000-info.txt)
- **Flare:** floor tile 64×32, tall-tile canvas 64×128, and 1 Blender unit = 1 m = 1 tile side for all assets. Use one fixed scale and a shared template scene — [Clint Bellanger, Isometric Tiles](https://clintbellanger.net/articles/isometric_tiles/)
- Slynyrd: blocks are cuboids, not perfect cubes. Heights can vary as long as the top surface stays uniform, and blocks slide up or down to change elevation — [Slynyrd Pixelblog 41](https://www.slynyrd.com/blog/2022/11/28/pixelblog-41-isometric-pixel-art)

### Inferences
- **X-COM derived:** (40 + 32/4)/2 = **24 px per level** on a 32-wide, 16-high diamond. That is 0.75×W, or 3× the 8-px row half-step. The 32×40 sprite = 16 px of floor diamond + 24 px of wall height. Screen z equals voxel z one-to-one (24 voxels = 24 px), so walls are 24 px tall and a level is 1.5 tile sides in voxel units. A geometrically true projection would put 24 voxels at about 29 px, so X-COM's vertical is about 82% of true.
- **X-COM step limit:** if MCD heights use the same 24-per-level voxel scale, the 8-unit step limit is 1/3 of a level, or 8 px on screen. That is an inference; the MCD page does not state the unit.
- **OpenTTD derived:** 8 px per level = W/8 = H/2 at 64×32. In world units a level is half a tile side (8/16). The geometrically true value would be about 19.6 px, so TTD terrain relief is compressed about 2.4×. That is why its hills read as gentle steps.
- **Practical height ladders at 64×32 (inference):**
  - 8 px fine step (TTD-like) for terrain and plinths.
  - 16 px (= H/2) as a "storey" or low wall.
  - 32 px (= W/2) as a full "cube". This is the common pixel-art cuboid: a 64×64 sprite holding a 32-px diamond plus 32-px faces.
  - True-cube height would be 39 px, which is awkward. Use 32.
- **Tower-defense heights (inference):** a lamp of about 24–40 px; a turret body of about 16–24 px plus a barrel; a refinery or launcher of 32–64 px; a 3×3 base of 64–96 px. Keep every height a multiple of one unit (8 px) so stacked parts and elevation offsets line up.

### Gaps
- No verified pixel height per altitude step for SimCity 2000 terrain.
- No verified UO pixels-per-z. A figure of 4 px per z is widely repeated but I did not confirm it.
- No measured wall or character heights for Diablo, AoE or Habbo.

---

## 5. Multi-tile footprints (1×1, 2×2, 3×3) and sprite anchoring

### Takeaway
A w×h footprint on a W×H 2:1 grid is a diamond (w+h)·W/2 wide and (w+h)·H/2 tall. Flare's 2×3 building at 160 px wide on a 64-px grid confirms the formula. Games either draw a building as one large sprite anchored to the footprint, or slice it into per-tile pieces so the painter's-algorithm depth sort stays correct. SimCity 2000 designed its 3×3 buildings first and cut them down to 2×2 and 1×1.

### Cited Findings
- Flare example: an oversized 2×3 building tile at 160×256 px on a 64×32 base grid — [Clint Bellanger, Isometric Tiles](https://clintbellanger.net/articles/isometric_tiles/)
- SimCity 2000: "3 x 3 tile buildings were designed first, then were cut down to create 2 x 2 tile buildings, which were in turn cut down to create 1 x 1 tile buildings" — [Wikipedia: SimCity 2000](https://en.wikipedia.org/wiki/SimCity_2000)
- X-COM keeps a separate sprite archive for larger objects at 32×48 instead of 32×40 — [UFOpaedia Image Formats](https://www.ufopaedia.org/index.php/Image_Formats)
- Isometric sprite origin: the tile's origin is its top corner, not the top-left of its bounding box. Shift by `screen.x -= TILE_WIDTH_HALF` before drawing the sprite — [Clint Bellanger, Isometric Math](https://clintbellanger.net/articles/isometric_math/)
- AoE2 terrain identifies a single "center pixel for the entire tile" as its reference point — [openage terrain docs](https://simonsan.github.io/openage-webdocs/sphinx/doc/media/terrain.html)

### Inferences
- **Footprint sizes at 64×32:**
  - 1×1: 64×32 (turret, lamp)
  - 2×2: 128×64 (refinery, missile launcher, resource pool)
  - 3×3: 192×96 (base)
  - 2×3: 160×80, matching Flare's 160-px width
  - Sprite canvas = footprint diamond height + building height, for example 3×3 base at 192 × (96 + 80) = 192×176. At 32×16, halve everything (1×1 = 32×16, 3×3 = 96×48).
- **Anchoring (inference; standard practice):**
  - Anchor every building sprite to the screen position of its footprint's back (top) corner, the origin tile's top tip per Clint's convention. Alternatively, anchor to the footprint's front (bottom) tip, which makes "ground contact" easy to author.
  - Store the anchor as a per-sprite (x, y) offset, the way AoE/SLP hotspots and OpenTTD sprite x/y offsets work.
  - For odd footprints (1×1, 3×3), the footprint centre falls on a tile centre, which is convenient for turret rotation pivots and range circles.
  - For 2×2, the centre falls on a tile corner (grid vertex). Range circles and pivots should use that vertex.
- **Depth sorting (inference):**
  - A single big sprite sorted by its anchor tile draws wrongly when a drone moves "behind" its right or left wing.
  - Option 1: slice multi-tile buildings into per-tile vertical strips, each sorted by its own tile. OpenTTD's multi-tile houses and industries are commonly understood to work this way (each tile has its own sprite), but that is unverified here.
  - Option 2: sort by the footprint's front-most depth, max(x+y), and keep drones out of footprints. In tower defense, drones usually path around buildings, so option 2 is usually enough.
  - Option 3: draw tall buildings after units in the same row band.
- **Tower-defense mapping (inference):** keep footprints odd-sized where you can (1×1 turrets and lamps, 3×3 base) for centred pivots. Use 2×2 only for blocky utility structures (refinery, pool). Footprints are axis-aligned in map space, so placement validation is a simple rectangle check in tile coordinates, much simpler than hex footprints.

### Gaps
- I could not verify exact SimCity 2000 building sprite widths, AoE building hotspots, or the OpenTTD per-tile building-sprite rule against primary sources within the budget.

---

## 6. Coordinate math (diamond vs staggered), picking and hit-testing, and which layout suits a small rectangular field

### Takeaway
For a diamond layout, tile→screen is `sx = (x−y)·W/2, sy = (x+y)·H/2`, with a simple linear inverse. Neighbours are ordinary square-grid neighbours, so pathfinding, footprints and range checks stay Cartesian. A staggered layout fills a screen rectangle with no wasted corners, but neighbour and inverse maths depend on row parity. For a small tower-defense field, use diamond (rotated-square) map coordinates. Either accept a diamond-shaped board, or mask and decorate the off-screen corners.

### Cited Findings
- Diamond tile→screen with 128×64 tiles: `screen.x = (map.x - map.y) * TILE_WIDTH_HALF; screen.y = (map.x + map.y) * TILE_HEIGHT_HALF;` — [Clint Bellanger, Isometric Math](https://clintbellanger.net/articles/isometric_math/)
- Diamond screen→tile: `map.x = (screen.x / TILE_WIDTH_HALF + screen.y / TILE_HEIGHT_HALF) / 2; map.y = (screen.y / TILE_HEIGHT_HALF - (screen.x / TILE_WIDTH_HALF)) / 2;` The float simplification is `map.x = screen.x / TILE_WIDTH + screen.y / TILE_HEIGHT; map.y = screen.y / TILE_HEIGHT - screen.x / TILE_WIDTH;`. The origin is the tile's top corner, and a camera is just a drawing offset (`screen.x += SCREEN_WIDTH_HALF` to centre). Do game logic in map coordinates and project only for input and rendering. Sub-tile positions work: map (2.5, 1.5) → screen (64, 128) — [Clint Bellanger, Isometric Math](https://clintbellanger.net/articles/isometric_math/)
- OpenXcom's inverse (screen→map) as shipped: `*mapY = -screenX + _mapOffset.x + 2*screenY - 2*_mapOffset.y; *mapX = screenY - _mapOffset.y - *mapY/4 - (_spriteWidth/4); *mapX /= (_spriteWidth/4); *mapY /= _spriteWidth;`, after adjusting screenY for level height — [OpenXcom Camera.cpp](https://github.com/OpenXcom/OpenXcom/blob/master/src/Battlescape/Camera.cpp)
- Another diamond formula, with the origin set so the whole map sits on screen: `screenx = (tilewidth*tilex)/2 + (height*tilewidth)/2 - (tiley*tilewidth)/2; screeny = ((height - tiley - 1)*tileheight)/2 + (width*tileheight)/2 - (tilex*tileheight)/2`. The page notes that picking must work "for any point inside the diamond, not only its center" but leaves the inverse unwritten — [isometric-tiles docs](https://isometric-tiles.readthedocs.io/en/latest/)
- Staggered maps: every other row is offset, so indexing differs from a plain 2D array. Tiled's "Isometric" and "Isometric (staggered)" orientations use different coordinate systems — [GameDev.net: staggered isometric screen to tile](https://gamedev.net/forums/topic/469674-staggered-isometric-screen-to-tile/); [Tiled forum: cartesian coords from isometric map](https://discourse.mapeditor.org/t/how-to-get-cartesian-coords-of-objects-from-tileds-isometric-map/4623)
- Common picking approach: take a rough candidate from the linear inverse, then test whether the point falls inside that candidate's diamond (or its neighbours') — [GameDev.net: detecting tile beneath the cursor](https://gamedev.net/forums/topic/542187-detecting-tile-beneath-the-cursor/)
- The classic reference on "slide / staggered / diamond" maps and the "mousemap" technique (look up the sub-cell offset in a colour-coded diamond image) is TANSTAAFL's "Isometric 'n' Hexagonal Maps Part I" — [GameDev.net r747](https://gamedev.net/tutorials/programming/general-and-gameplay-programming/isometric-39n39-hexagonal-maps-part-i-r747). The page returned HTTP 403, so its content was not verified here.

### Inferences
- **Diamond picking, exact (derived).** With the origin at tile (0,0)'s top tip and the camera offset removed, `u = sx/(W/2)`, `v = sy/(H/2)`, `tx = floor((u+v)/2)`, `ty = floor((v−u)/2)`. Using `floor` rather than truncation keeps negative coordinates correct. Because of the 64×31 or 62×32 art shapes (Q3), set the pick boundary with the same rule your art uses (for example, a top-left fill rule) so clicks on the seam pixel go to the tile that owns it.
- **Elevated tiles or buildings:** add `z·heightUnit` to `sy` before inverting, as OpenXcom does with `_mapOffset.z`. For tall sprites, do a second pass: walk candidate tiles front-to-back and test sprite bounding boxes or alpha masks, so clicking a turret's barrel selects the turret.
- **Point-in-diamond test (derived):** `|dx|/(W/2) + |dy|/(H/2) ≤ 1`, measured from the tile centre. This is useful for hover highlights and footprint validity on a single tile.
- **Staggered layout (derived):**
  - Tile→screen: `sx = col·W + (row odd ? W/2 : 0)`, `sy = row·H/2`.
  - Picking: find the W×H cell, then use a 4-corner test (or a mousemap) to move to a diagonal neighbour.
  - Neighbour offsets change with row parity, much like the offset hex grid the project is leaving. A 3×3 footprint becomes a zig-zag set of (col, row) pairs.
  - Converting staggered → diamond, from equating the two projections (x+y = row, x−y = 2·col + (row&1)): `x = col + ceil(row/2)`, `y = floor(row/2) − col` (sign conventions vary). Many engines store diamond coordinates and use staggering only for which tiles get drawn.
- **Recommendation for a small rectangular TD field (inference):** store the map in diamond coordinates, a plain N×M array. Pathfinding (BFS/A*), 2×2 and 3×3 footprint checks, range checks and drone steering all stay simple square-grid maths. There are two ways to fill a rectangular screen:
  - (a) Show the map as a diamond and decorate the four empty screen triangles with UI or backdrop.
  - (b) Use a larger diamond array and mark the tiles outside a screen-aligned rectangle as void or unbuildable. This gives the staggered look without staggered maths.
  - A staggered array only wins if the playfield must be a perfect screen rectangle and you never need footprint or neighbour maths, which is not the case for tower defense.
- **Drawing order (derived):** iterate by increasing `x+y`, and within each diagonal by `x`, drawing ground, then buildings/units sorted by depth key `x+y` (plus a z tie-break). Drones move freely, so use their continuous map position `(mx+my)` as the sort key.

### Gaps
- I could not retrieve the TANSTAAFL mousemap article or a primary staggered-map inverse. The staggered formulas above are derived, not sourced.

---

## 7. Unit size compared with tile size

### Takeaway
I found little measured data. X-COM units are drawn in the same 32×40 frame as terrain tiles and occupy one tile each. Exact figure-width percentages of the tile were not documented in any source I reached.

### Cited Findings
- X-COM Battlescape sprite archives (terrain and units) use 32×40 px frames; big objects use 32×48 — [UFOpaedia Image Formats](https://www.ufopaedia.org/index.php/Image_Formats)
- X-COM collision and line of sight use a 16×16×24 voxel cell per tile, and units are placed by tile × 16 (x, y) and z × 24 — [OpenXcom commit](https://www.github.com/MeridianOXC/OpenXcom/commit/1c188b9af2576fd7109bbf8e7c1749f93760f8fb); [OpenXcom forum](https://openxcom.org/forum/index.php/topic,571.0.html)
- X-COM units can step between tiles only when the height difference is 8 or less — [UFOpaedia MCD](https://ufopaedia.org/index.php?title=MCD)
- Clint's isometric-math article treats unit positions as continuous map coordinates (for example (2.5, 1.5)) projected with the same formula as tiles. That supports units moving between tile centres without snapping — [Clint Bellanger, Isometric Math](https://clintbellanger.net/articles/isometric_math/)

### Inferences
- The X-COM frame is 32 px wide, the same as the tile diamond, so a soldier's visible body is only a fraction of that frame. From typical X-COM screenshots it is roughly 12–16 px, or 40–50% of tile width. This is an unverified estimate.
- **Tower-defense guidance (inference):**
  - Drones should read clearly against 64-px tiles and leave lanes visibly wider than the units. Body widths of about 24–40 px (≈35–60% of W) fit, with smaller scouts near 24 px and heavy drones near 40 px.
  - Draw a ground shadow ellipse of about W/2 × H/2 (32×16) under flying drones to anchor them to the grid.
  - Keep drone hover height in the same 8-px height unit as terrain (for example, 16 or 24 px).
  - Sort drones by continuous `mx+my` so they pass correctly in front of and behind 2×2 and 3×3 buildings.
- With a 1:1 tile-to-unit footprint convention like X-COM's, at most one drone fits per tile. Tower-defense swarms usually need several drones per tile, so drones should be smaller than a tile and move on continuous coordinates.

### Gaps
- No measured unit-to-tile ratios were found for SimCity 2000, OpenTTD vehicles, Diablo, AoE, Into the Breach, Tactics Ogre or Habbo. Sprite rips from The Spriters Resource would be the next step for measured numbers.
- No source documents large multi-tile units, such as X-COM's 2×2 tanks, as 4 sub-sprites. That is commonly reported but unverified here.
