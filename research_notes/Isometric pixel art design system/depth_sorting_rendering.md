# Depth Sorting and Rendering of Multi-Tile Buildings and Moving Units in a 2D Isometric Canvas Engine

Scope note: these notes target one HTML5 canvas using the 2D context, vanilla JS, no WebGL. The board holds one 3x3 or 2x2 base, 1x1 structures, drones that move continuously, projectiles, light and fog overlays, and cables.

Coordinate convention used in my inferences (from Clint Bellanger, below): `screenX = (x - y) * TILE_W/2`, `screenY = (x + y) * TILE_H/2`. Larger `x + y` is nearer the viewer and is drawn later. Shaun Lebron's article uses the opposite convention: in his, lesser x and y are nearer the camera. Flip the signs when you read his rules.

Source quality: the primary sources are OpenTTD's `viewport.cpp` and Flare's `MapRenderer.cpp` (both read directly from master on 2026-11-25), Shaun Lebron, Andrew Russell, Bannalia, Juwal Bose (Tuts+), Martin Pane (Game Developer), Clint Bellanger and MDN. Items marked "(search snippet)" come from a search-engine summary of the page, not from a page I read in full. Treat them as weaker.

---

## 1. Painter's-algorithm orders for diamond maps: (x+y), screen y, tile row, and what breaks with each

### Takeaway
For 1x1 objects on flat ground, any order that draws each cell after its back neighbours is correct: diagonal `x+y` buckets, nested `for y { for x }` rows, columns, or screen-y of the ground foot point. The orders only diverge, and break, when you add multi-tile objects, objects moving between cells, or elevation. A single scalar key cannot express the true "in front of" relation in general, because that relation is neither total nor transitive.

### Cited Findings
- Row-by-row drawing with a sequential sort order per tile works for static levels, and "drawing complete columns in sequence works equally well". Characters smaller than a tile can use the depth of the tile they occupy. — [Juwal Bose, Tuts+](https://code.tutsplus.com/isometric-depth-sorting-for-moving-platforms--cms-30226t)
- Row-based sorting breaks when an object moves along y between rows: "When the tile is between two rows, it would get assigned a sortingOrder based on the row it is moving from," which "essentially breaks our depth sorting approach." — [Juwal Bose, Tuts+](https://code.tutsplus.com/isometric-depth-sorting-for-moving-platforms--cms-30226t)
- Movement along x within the trench between two rows needs no special handling, because row 1 always stays behind row 2. — [Juwal Bose, Tuts+](https://code.tutsplus.com/isometric-depth-sorting-for-moving-platforms--cms-30226t)
- The common tutorial formula is `depth = cartX + cartY` (equivalently, sort by iso screen Y), with elevation added as `depth = cartX + cartY + z`. (search snippet) — [Phaser isometric tutorial, generalistprogrammer](https://generalistprogrammer.com/tutorials/phaser-isometric-game-tutorial)
- Martin Pane notes that for simple cases "you can just set the Z axis to be equal to the Y axis, and it will work just fine." Rectangular-base buildings are where this fails (see Q2). — [Martin Pane, Game Developer](https://www.gamedeveloper.com/programming/what-i-learned-from-trying-to-make-an-isometric-game-in-unity)
- Bannalia's "Filmation math" defines "is behind" for isometric boxes using six axis tests on overlapping hexagon projections. It states: "the relation 'is behind' defined by the algorithm above is not total" and "The relation 'is behind' is not transitive". The author's comments add that the relation is not a strict weak ordering, so "using a sorting algorithm can't work." — [Joaquín M López Muñoz, Bannalia (2008)](https://bannalia.blogspot.com/2008/02/filmation-math.html)
- Lebron makes the same point: "we cannot deduce relative drawing order between two non-overlapping boxes," so ordering is a partial order. He also notes that sorting is "much simpler" when every bounding box is a unit cube. — [Shaun Lebron, Drawing Isometric Boxes in the Correct Order](https://shaunlebron.github.io/IsometricBlocks/)
- Flare, an open-source isometric ARPG engine, sorts renderables by a 64-bit priority: `prio += ((tilex + tiley) << 37) + (tilex << 20) + ((commax + commay) << 8)`. Here `tilex = floor(map_pos.x)` and `commax/commay` are the sub-tile fractions scaled by 1024. The comment reads "Sort in the same order as the tiles are drawn." The key is diagonal index first, then tile x as a tiebreak, then the sub-tile fraction. — [Flare MapRenderer.cpp](https://github.com/flareteam/flare-engine/blob/master/src/MapRenderer.cpp)
- OpenTTD seeds its sorter with sprites "sorted by xmin+ymin" and relies on input that is "for the most part, already ordered" from the tile-scan order. — [OpenTTD viewport.cpp, ViewportSortParentSprites](https://github.com/OpenTTD/OpenTTD/blob/master/src/viewport.cpp)

### Inferences
- **Why 1x1 orders are interchangeable.** Tiles on the same diagonal (`x+y` equal) are a full tile width `TILE_W` apart on screen. Sprites no wider than `TILE_W` never overlap within a diagonal, so the order inside a diagonal doesn't matter. Any linear extension of "(x,y) after (x-1,y) and (x,y-1)" is a valid painter's order for 1x1 things. Diagonal buckets, `for y { for x }`, and `for x { for y }` all qualify.
- **What breaks with each order:**
  - *Tile-row loop with multi-tile objects anchored at one tile.* Anchor a 3x3 building at its back tile, and in a `for y { for x }` loop a 1x1 object at `(x0-1, y0+2)` is drawn after the building even though it is behind the NW face. Anchor it at its front tile instead, and an object at `(x1, y0)` in front of the SE face is drawn before it. No single anchor tile is correct for every neighbour (my derivation).
  - *Screen-y of the sprite's image rectangle (bottom edge).* For a multi-tile building, the image bottom is the front corner (`x1+y1`). That is the "front-most corner" key, which fails for units in front of its long faces (see Q2).
  - *Screen-y including elevation (`(x+y)*H/2 - z*scale`) or `x+y+z` as one scalar.* A hovering drone in front of a building sorts earlier as it climbs, and at some altitude it is drawn behind the building. Equally, `x+y+z` lets a high drone that is behind a building sort in front of it. Sort ground objects by ground position. Treat z separately (see Q3).
  - *Tile-bucketed moving units (draw the unit with "its" tile).* This breaks at row transitions, as Bose describes. Continuous keys avoid it (see Q3).
- **Recommended for this game.** Don't interleave floor tiles with objects. Draw the floor in its own pass (Q5). Then sort only the object list, by a continuous scalar key computed from the footprint centre (Q2). This sidesteps the row and diagonal question for the floor entirely.

### Gaps
- I found no canonical GameDev Stack Exchange answer that compares the three orders side by side. The failure analysis above is my derivation from the cited rules.

---

## 2. Multi-tile objects: avoiding sorting errors when a unit walks behind or beside a 2x2 or 3x3 building

### Takeaway
There are four families of fix: (a) slice the building into per-tile or per-segment pieces, each with its own depth; (b) give the whole building one key; (c) pairwise bounding-box tests plus a topological sort (Lebron, Bannalia, Russell, OpenTTD); (d) hide errors with occlusion cues (AoE-style silhouettes, Flare's wall fading). One single-key choice turns out to be exact here: for **square** footprints, a key at the **footprint centre** (`cx + cy`) is a correct painter's order, under conditions listed below. Every footprint in this game (3x3, 2x2, 1x1) is square, so you need neither slicing nor a topological sort, provided those conditions hold. Front-most-corner and back-most-corner keys both fail.

### Cited Findings
**Slicing**
- Martin Pane: with a rectangular base, a single floor height fails because "we could be walking along its front side and be behind it at the same time!" His fix: "To solve this we need to cut the sprite in two, and set a different floor for each slice." Windows and signs are parented to the building with a small Z offset so they render in front without being sorted against it. Thin walls are "probably the most notorious limitation," because they must be sliced as thin as the wall. — [Martin Pane, Game Developer (2017)](https://www.gamedeveloper.com/programming/what-i-learned-from-trying-to-make-an-isometric-game-in-unity)
- The Pogicity city-builder demo: "Very tall buildings can cause depth sorting issues when characters walk 'behind' them. The solution is to slice the sprite into horizontal strips and give each strip a different depth." (search snippet) — [twofactor/pogicity-demo](https://github.com/twofactor/pogicity-demo)
- A GameDev.net thread reports that depth handling "goes out of the window" for multi-tile buildings. One suggested fix: give every cell under the object the origin cell's depth, then draw a transparent cell with the correct depth on top of each. (search snippet) — [GameDev.net thread 643636](https://gamedev.net/forums/topic/643636-isometric-staggered-drawing-objects-bigger-than-1-tile-depth-error/5065262/)
- Tiled forum suggestion: have multi-tile objects "sort to the lowest tile's y value." (search snippet) — [Tiled forum, Multi-tile objects](https://discourse.mapeditor.org/t/multi-tile-objects/4524)
- LibGDX/Tiled advice: use "oversized" tiles (e.g. 32x64 art on a 32x32 grid), "then we simply order the objects and tiles by their 'feet y'." (search snippet) — [Tiled forum, LibGDX render order](https://discourse.mapeditor.org/t/libgdx-render-order-isometric-maps/1109)

**Bounding boxes and topological sort**
- Lebron's method has three steps:
  1. Project each box to a screen hexagon and test overlap on three screen axes (x, y, and `h = (isoX - isoY)*cos(π/6)`). Only pairs that overlap need ordering.
  2. For each overlapping pair, find a separating axis in the order x, then y, then z. In his convention the lower x (or y) is in front; on z, the higher one is in front.
  3. Add an edge from each box to every box behind it, then topologically sort (DFS). "A box should not be drawn until all the ones behind it are drawn."

  Cycles of three intertwined boxes can occur. He suggests detecting them with Tarjan's SCC algorithm, then splitting a box or clipping. The method assumes axis-aligned, non-intersecting boxes. — [Shaun Lebron](https://shaunlebron.github.io/IsometricBlocks/)
- Bannalia gives the costs: topological sorting is O(|V|+|E|), but building the edge set by pairwise checks is O(|V|²). Ultimate's Filmation engine did not resolve cycles; its programmers "decided to just ignore them." A Knight Lore screenshot shows the resulting rendering error. — [Bannalia, Filmation math](https://bannalia.blogspot.com/2008/02/filmation-math.html)
- Filmation's key idea was "masked sprites", as opposed to earlier "planar sprites" that overlapped without regard for depth. (search snippet) — [Wikipedia, Filmation (game engine)](https://en.wikipedia.org/wiki/Filmation_(game_engine))
- Andrew Russell (River City Ransom: Underground) describes a 2.5D side-view game, not isometric, but the graph technique transfers:
  - Each object is a vertex. Edges exist only between overlapping pairs, because computing edges for non-overlapping pairs is costly and each edge is a constraint.
  - The graph is sorted topologically.
  - Cycles (the "painter's dilemma") are found as SCCs with a modified Tarjan, then broken by removing the edges with the least visual impact.
  - Bounds are per animation, not per frame, to reduce flicker.
  - He rejected per-pixel splitting because the collision boundary is smaller than the sprite, so a character's hand would "vanish into a car."
  - Forced orderings (pick-ups, grapples) can create SCCs.

  — [Andrew Russell (2016)](https://andrewrussell.net/2016/06/how-2-5d-sorting-works-in-river-city-ransom-underground/)
- OpenTTD's production sorter (`ViewportSortParentSprites`) stores a world-space AABB per parent sprite (`xmin..xmax, ymin..ymax, zmin..zmax`):
  - Sprite `p` must precede sprite `s` unless `s->xmax < p->xmin || s->ymax < p->ymin || s->zmax < p->zmin`.
  - If the two boxes interpenetrate on all three axes, it falls back to comparing the summed centre coordinates `xmin+xmax+ymin+ymax+zmin+zmax`.
  - It processes sprites from a stack in pre-sorted `xmin+ymin` order, and moves "preceding" sprites in front of the current one.
  - Larger coordinates are drawn later.

  — [OpenTTD viewport.cpp](https://github.com/OpenTTD/OpenTTD/blob/master/src/viewport.cpp)
- An OpenTTD commit message says a zoomed-out hi-res view can mean about 150k sprites, and the O(n²) sorter can then take several seconds, which motivated an SSE4.1 sorter. (search snippet) — [OpenTTD SSE4 sorter source](https://docs.openttd.org/source/d4/d7d/viewport__sprite__sorter__sse4_8cpp_source.html)
- OpenTTD's known-bugs list: bounding boxes "are often either too big or too small, causing problems with what is drawn in front of what." (search snippet) — [OpenTTD known-bugs.txt (mirror)](https://git.blob42.xyz/Archives/OpenTTD-patches/src/branch/master/known-bugs.txt)
- OpenTTD sprite layouts are defined per house tile and per industry tile. The bug tracker discusses an extended syntax that lets a sprite share the previous sprite's bounding box. (search snippet) — [OpenTTD bug 3303](https://bugs.openttd.org/task/3303)
- A Kotlin isometric library sorts faces by mean vertex depth and finishes with a topological sort. It says intersecting geometry cannot always be ordered, and the workaround is to split shapes. (search snippet) — [Isometric library docs, Depth Sorting](https://jayteealao.github.io/Isometric/concepts/depth-sorting/)

**Occlusion cues instead of exact sorting**
- AoE3: DE has an "Obscured Unit Alpha" setting that shows coloured silhouettes of units behind buildings. AoE forums show players depend on outlines to find and select units hidden behind buildings and forests. (search snippets) — [AoE forum: Obscured Unit Alpha](https://forums.ageofempires.com/t/please-add-option-to-disable-obscured-unit-alpha/194981); [AoE forum: units hidden behind castle](https://forums.ageofempires.com/t/bug-units-completely-hidden-behind-castle-no-unit-outline/73253)
- 0 A.D. renders silhouettes as solid-coloured units in an extra pass, using inverted depth or stencil tests. (search snippet) — [0 A.D. ticket #144](https://trac.wildfiregames.com/ticket/144)
- Flare fades any wall tile that overlaps the hero (`fade_walls` / `fadeOverlapTile`), so the player stays visible behind tall tiles. — [Flare MapRenderer.cpp](https://github.com/flareteam/flare-engine/blob/master/src/MapRenderer.cpp)

### Inferences
**Comparing the single-key options for a 3x3 footprint `[x0,x1]×[y0,y1]`** (my derivation; not stated in any source):
- *Front-most corner key `x1+y1`.* A unit in front of the SW face at `(x0+0.5, y1+0.5)` has key `x0+y1+1`, which is less than `x1+y1 = x0+y1+3`. The unit is drawn first and the building covers it. **Wrong.**
- *Back-most corner key `x0+y0`.* A unit behind the NW face at `(x0-0.5, y1-0.5)` has key `x0+y0+2`, which is greater than `x0+y0`. The unit is drawn on top of the building. **Wrong.**
- *Centre key `cx+cy`.* The line `x+y = cx+cy` passes through the left corner `(x0,y1)` and the right corner `(x1,y0)` of a square footprint. Every point in front of the two front faces has a larger key, and every point behind the two back faces has a smaller one. **Correct for squares.**

**Proof sketch for the centre key.** Take axis-aligned square footprints A and B of any sizes that are separated on x with B in front (`A.x1 <= B.x0`).
- Their screen-x intervals overlap only if `B.y1 - A.y0 > B.x0 - A.x1 >= 0`.
- With `c = x0 + y0 + size`, the difference is `cB - cA = (B.x0 - A.x1) + (B.y1 - A.y0) > 0`.
- The y-separated case is symmetric.
- If one axis says A is in front and the other says B is, their screen projections cannot overlap.
- For rectangles, the difference gains a term `(wA - hA + wB - hB)/2`, which can turn negative. That matches Martin Pane's observation that rectangular bases need slicing.

**Conditions for the centre key to be exact:**
1. Footprints are square and don't overlap. Units never enter a building's footprint.
2. Each sprite's art stays horizontally inside its footprint's diamond extrusion, i.e. inside Lebron's hexagon silhouette. For a unit, model the footprint as a square of side `s >= spritePixelWidth / TILE_W` tiles around its feet. Movement must keep that square out of building footprints; drones that follow tile-centre paths keep 0.5 tile clearance. If a wide sprite hugs a building's left or right corner, expect glitches right at the corner.
3. Every object stands on, or hovers over, open ground. Being z-separated doesn't matter here, because x/y separation alone is a valid separating plane. A drone whose ground point is over a building footprint breaks this (see Q3).

**Recommendation for this game.** Keep every footprint square (3x3, 2x2, 1x1). Sort by `key = cx + cy` using continuous floats. Draw the base as one sprite. Use no slicing and no graph. If you later add a non-square footprint such as 2x3, either split it into square pieces with the sprite cut along matching clip paths (Pane's "cut the sprite in two"), or switch to Lebron's pairwise test plus topological sort. Use OpenTTD-style AABBs, and for robustness add OpenTTD's centre-sum fallback when boxes interpenetrate.

**Cost of each approach:**
- Centre key: O(n log n) per frame. Effectively free for a few hundred objects.
- Pairwise plus topological sort: O(n²) overlap tests plus O(V+E). About 40k cheap tests for 200 objects, so under 1 ms in JS. It needs cycle handling, and its output is not stable frame to frame. That only reorders non-overlapping items, so it is visually harmless.
- Per-tile slicing: N clip regions per building (9 for 3x3). That is more `drawImage` calls and fiddly clip geometry, because adjacent tile columns overlap by `TILE_W/2` horizontally, so vertical cuts don't separate them. It also needs art authored or cut per tile. OpenTTD and TTD avoid the cutting by authoring sprites per tile.

**Belt and braces.** Draw occluded-drone silhouettes (Q5) so that any remaining corner glitch, or a drone hidden behind the tall base, stays readable.

### Gaps
- I found no primary source describing how SimCity 2000 or Age of Empires internally order multi-tile buildings against units, whether by slicing, a single anchor, or something else. Only AoE's silhouette feature is documented, and only through player forums.
- I did not read OpenTTD's NewGRF or NML documentation to confirm the "multi-tile industries are drawn per tile" convention in its own words. The bug tracker reference is indirect.
- I did not check OpenRA, Phaser isometric plugins or PixiJS iso projects.
- The centre-key proof is my own derivation. I found no source that states it, so check it in-game with a debug overlay that draws each object's footprint and key.

---

## 3. Units moving between tiles: computing the sort key, and units sitting on a tile boundary

### Takeaway
Compute the key from the unit's continuous ground position (its feet or shadow point), not from the tile it is registered in. Then a unit on a tile boundary is just another float, and there is no boundary case against static objects. Use a fixed tiebreak (entity id) so equal keys don't flicker. Airborne objects need one extra rule: when a flier's ground point is inside a building footprint and it is above the roof, sort it after that building, or draw it in a separate air pass.

### Cited Findings
- Bose's fix for objects moving between rows is "block sorting":
  - Sort the 2x2 "problem block" around the moving tile column-first, and everything else row-first. The moving object gets its order at the matching point inside the block.
  - For movement between floors, "we are considering two floors as a single floor and doing a block sort on that single floor."
  - Stated limits: one moving object only, no slopes, characters smaller than a tile.

  — [Juwal Bose, Tuts+ (2018)](https://code.tutsplus.com/isometric-depth-sorting-for-moving-platforms--cms-30226t)
- Flare builds unit keys from `floor(x)+floor(y)` plus a fractional sub-key, so entities interleave with the object-layer tiles in the order those tiles are drawn. While walking the object layer diagonally, it also tests whether an entity's bottom-left and bottom-right screen points fall inside the SW or NE neighbour tile's bounds. If so, it queues the entity into `render_behind_SW` or `render_behind_NE`, which are drawn before those tiles. A code comment admits this "does not account for entities that are made up of multiple Renderables" and that the hero-only fix "is incomplete." — [Flare MapRenderer.cpp](https://github.com/flareteam/flare-engine/blob/master/src/MapRenderer.cpp)
- Andrew Russell uses per-animation rather than per-frame bounds so ordering stays stable through looping animations and doesn't flicker. — [Andrew Russell](https://andrewrussell.net/2016/06/how-2-5d-sorting-works-in-river-city-ransom-underground/)
- Martin Pane: flying objects cause sorting problems, which "can be managed with careful placement or by changing the Sorting Layer" so they always render in front of (or behind) the main scene. — [Martin Pane, Game Developer](https://www.gamedeveloper.com/programming/what-i-learned-from-trying-to-make-an-isometric-game-in-unity)
- Lebron's z rule: if z ranges don't intersect, the higher box is in front. This is checked only after x and y fail to separate. — [Shaun Lebron](https://shaunlebron.github.io/IsometricBlocks/)
- Sprite-art guidance: "Feet should fit comfortably on one tile. But if the character's body covers three tiles, your depth sorting will be a nightmare." (search snippet) — [sprite-ai isometric pixel art guide](https://www.sprite-ai.art/guides/isometric-pixel-art)

### Inferences
**Sort keys by object type:**
- Ground units and low drones: `key = x + y` of the feet (shadow) point, kept as a float, never floored.
- Buildings and structures: `key = cx + cy` of the footprint centre.
- Tiebreak: `(key, kind, id)`. For example, at an exact tie draw static before mobile, then by id.
- `Array.prototype.sort` has been stable since ES2019, but an explicit id tiebreak is still safest. This is from general JS knowledge and was not verified this session.
- Flare's `floor()`-based key exists only because Flare interleaves units with object-layer tiles. With the floor in its own pass, continuous `x+y` is simpler and has no boundary discontinuity. Floor-sum and continuous sum disagree: `(0.9, 0.9)` has floor-sum 0, while `(1.1, 0)` has floor-sum 1.
- Don't sort from the tile-occupancy grid. Gameplay may reserve the destination tile while the sprite is still mid-transit. If you must bucket, for example to cull by diagonal, put a moving unit in the front-most tile its footprint square touches (`max` of the covered tile indices). This is a common heuristic; I found no source for it.

**Airborne drones and projectiles.** Model them as a ground point `(x, y)` plus altitude `z`. Draw at `screenY - z*Z_SCALE`. Rules:
1. If the ground point is over open ground, use the normal ground key. Altitude doesn't change the order: per Q2, x/y separation from a building holds regardless of z.
2. If the ground point is inside a building footprint and `z >= building.height`, set `key = building.x1 + building.y1 + ε` so it draws after that building (Lebron's z rule).
3. If it is inside a footprint and below the roof, that is a collision. Keep it out with gameplay rules.

The simpler alternative is an "air pass": draw everything above maximum roof height after the whole sorted list. This is Pane's sorting-layer trick. It is wrong only when a high flier passes behind a taller object, which can't happen if nothing is taller than the flight altitude.

**Jitter.** Interpolated positions change the key smoothly, so units cross each other's keys exactly once. Sudden order flips come only from discontinuous keys, such as tile-floor keys or switching anchors.

### Gaps
- I found no source that gives a complete, general solution for multiple moving objects with tile-bucketed sorting. Bose handles one object, and Flare's code calls its own approach a hack. The continuous-key recommendation is my inference.

---

## 4. Tall sprites: anchoring at the ground foot point, origin conventions, and overlap of tiles behind

### Takeaway
Store each sprite with an explicit anchor: the pixel offset from the image's top-left to the ground point used for sorting (the footprint centre, or the feet for units). Draw at `round(project(ground) - anchor)`. Tall art then extends upward over whatever is behind it. That is correct under painter's order, as long as the floor is drawn in an earlier pass and culling extends far enough to catch tall sprites whose base is off-screen.

### Cited Findings
- Clint Bellanger's formulas:
  - Map to screen: `screen.x = (map.x - map.y) * TILE_WIDTH_HALF; screen.y = (map.x + map.y) * TILE_HEIGHT_HALF`.
  - Screen to map: `map.x = (screen.x / TILE_WIDTH_HALF + screen.y / TILE_HEIGHT_HALF) / 2; map.y = (screen.y / TILE_HEIGHT_HALF - screen.x / TILE_WIDTH_HALF) / 2`.
  - The tile's origin is its top corner while sprites draw from top-left, so adjust, e.g. `screen.x -= TILE_WIDTH_HALF`.

  — [Clint Bellanger, Isometric Tiles Math](https://clintbellanger.net/articles/isometric_math/)
- Flare positions each tile sprite with a per-tile offset: `dest.x = tile_center.x - tile.offset.x; dest.y = tile_center.y - tile.offset.y`. It widens the visible tile range by the tileset's maximum sprite size (`2 * tset.max_size_x`, `max_size_y`), so tall tiles whose base is off-screen are still drawn. — [Flare MapRenderer.cpp](https://github.com/flareteam/flare-engine/blob/master/src/MapRenderer.cpp)
- OpenTTD gives each parent sprite a screen anchor and a separate world-space bounding box (`ParentSpriteToDraw`: min/max x, y, z plus screen x, y). The box runs "from the offset origin to the far corner, both corners included." — [OpenTTD ParentSpriteToDraw](https://docs.openttd.org/source/df/def/structParentSpriteToDraw); [OpenTTD changeset (search snippet)](https://code.theleruby.com/cpp/openttd-patchpack/source/changeset/3094aefd5bba6ff46f4b6d16df96ece8fb2969bb)
- Martin Pane defines the sort "floor" as "where the sprite would be touching the ground if it were a 3D object," an offset from the sprite's lower bounds. — [Martin Pane, Game Developer](https://www.gamedeveloper.com/programming/what-i-learned-from-trying-to-make-an-isometric-game-in-unity)
- LibGDX/Tiled practice: oversized tiles (32x64 on a 32x32 grid), ordered by "feet y." (search snippet) — [Tiled forum](https://discourse.mapeditor.org/t/libgdx-render-order-isometric-maps/1109)

### Inferences
**Sprite metadata to store per frame:** `{img, sx, sy, sw, sh, ax, ay, footprint:{w,h}, height}`.
- For a building, `(ax, ay)` is the footprint diamond's centre pixel within the image. For an n×n footprint this sits `n * TILE_H/2` below the footprint's top corner and horizontally centred.
- For units, `(ax, ay)` is the feet or shadow centre.
- Draw with: `dx = Math.round(originX + (gx - gy)*TW2 - ax); dy = Math.round(originY + (gx + gy)*TH2 - z*ZS - ay)`.

**Tall structures over tiles behind them.**
- Floor tiles are drawn in the ground pass, so a tall building covering the floor behind it is correct.
- Objects behind it are covered because they sort earlier.
- The only real problem is gameplay visibility: units, targets or hover highlights hidden behind the base. Fix that with fading or silhouettes (Q5, Q6), not with sorting.

**Culling.** Cull sprites by their screen rectangle (`dx, dy, sw, sh`), not by footprint tile, so tall art from below the viewport isn't dropped. Flare's `max_size` padding is the tile-loop equivalent.

**Art rule that keeps the Q2 centre key exact.** Nothing should stick out sideways past the footprint's left or right corner: no overhanging antennas, cranes or dishes. Overhangs need their own sorted child sprite, or must be kept small.

### Gaps
- I did not find OpenTTD's or Flare's sprite-sheet anchor conventions spelled out in their asset docs (OpenTTD x_offs/y_offs, Flare tileset definition files). The above comes from code reading.

---

## 5. Ground layers vs object layers: floor tiles, decals (pools, cables, range rings, shadows), and fog or lighting masks

### Takeaway
Use fixed passes. Draw the floor and anything flat on the ground unsorted, first. Then draw sorted objects, each with its attached child sprites. Then the air pass, then screen-space overlays (light and fog), then text and UI. OpenTTD and Flare both follow this shape. Fog that is applied as ground-tile diamonds after the objects will wrongly darken tall buildings that rise over fogged tiles behind them. Apply fog to the floor in the ground pass, and to objects per object.

### Cited Findings
- OpenTTD's viewport draw sequence (`ViewportDoDraw`):
  1. Collect the landscape and vehicles.
  2. `ViewportDrawTileSprites` draws ground or tile sprites in collection order, unsorted.
  3. `_vp_sprite_sorter(&parent_sprites_to_sort)` sorts, then `ViewportDrawParentSprites` draws each parent sprite immediately followed by its linked child sprites. Children are positioned relative to the parent and are not sorted independently.
  4. Optional bounding-box debug draw.
  5. The link-graph overlay.
  6. `ViewportDrawStrings`: signs and text.

  Sprites drawn on a foundation are attached to it as child sprites. — [OpenTTD viewport.cpp](https://github.com/OpenTTD/OpenTTD/blob/master/src/viewport.cpp)
- OpenTTD's `StartSpriteCombine` "begins a block of sprites that are combined into a single bounding box," which is a way to make a multi-part object sort as one unit. (search snippet of docs) — [OpenTTD viewport.cpp docs](https://docs.openttd.org/source/da/d7e/viewport_8cpp.html)
- Flare's `renderIso` sequence:
  1. Every layer below the object layer, as plain tile layers.
  2. `renderIsoBackObjects(r_dead)`: dead entities, drawn as a separately sorted list before the live object layer, so corpses act like floor decals.
  3. `renderIsoFrontObjects(r)`: object-layer tiles interleaved with live entities.
  4. Layers above the object layer. With `FogOfWar::TYPE_OVERLAY`, these include the `fow_dark` and `fow_fog` layers drawn with fog tilesets. With `FogOfWar::TYPE_TINT`, Flare instead sets each tile's `color_mod` from the fog state.
  5. Tooltips and dev HUD.

  — [Flare MapRenderer.cpp](https://github.com/flareteam/flare-engine/blob/master/src/MapRenderer.cpp)
- Pane parents decorations (windows, signs) to the building with a small z offset so they never sort against it, the same idea as OpenTTD child sprites. — [Martin Pane, Game Developer](https://www.gamedeveloper.com/programming/what-i-learned-from-trying-to-make-an-isometric-game-in-unity)
- Silhouette pass for occluded units: AoE3 DE "Obscured Unit Alpha"; 0 A.D. uses an extra pass drawn only where the unit is hidden. (search snippets) — [AoE forum](https://forums.ageofempires.com/t/please-add-option-to-disable-obscured-unit-alpha/194981); [0 A.D. ticket #144](https://trac.wildfiregames.com/ticket/144)
- MDN recommends splitting complex scenes into layers that update at different rates (static background, per-frame game layer, UI), using stacked canvases or offscreen canvases. — [MDN, Optimizing canvas](https://developer.mozilla.org/en-US/docs/Web/API/Canvas_API/Tutorial/Optimizing_canvas)

### Inferences
**Suggested pass list for this game.** Items marked [cache] go in an offscreen canvas that is rebuilt only when it changes.
1. **Floor** [cache]: floor diamonds, plus static flat decals such as pools, painted pads and ground-routed cable conduits.
2. **Ground fog** [cache, rebuilt when visibility changes]: darken or desaturate unexplored floor diamonds here, before objects, so tall objects are never darkened by fog belonging to the tiles behind them.
3. **Dynamic ground decals**, unsorted, drawn every frame:
   - Range rings: draw them as iso ellipses, i.e. `ctx.ellipse` with radiusY = radiusX × TILE_H/TILE_W.
   - Placement ghost footprint, selection and hover diamonds.
   - Drop shadows of drones and projectiles, drawn at their ground point.
   - Building cast shadows.

   Shadows fall on the floor but not on other objects. That is acceptable stylisation. If needed, darken a drone whose ground point is inside a shadow polygon.
4. **Sorted objects**: buildings, 1x1 structures, ground units and low drones, each followed immediately by its child sprites (turret head, blinking lights, damage sparks), as OpenTTD does.
   - Fog per object: skip enemies the player can't see, or draw explored-but-not-visible structures from a pre-tinted darker sprite variant. The 2D canvas has no per-draw colour modulation like Flare's `color_mod`, so pre-bake tinted variants on offscreen canvases at load.
5. **Air pass**: high drones and projectiles (or use the key override from Q3), plus overhead cables strung between rooftops. A long cable spans many tiles and can't take one sort key. Choose one of:
   - (a) Route cables on the ground (pass 1 or 3), which is always correct.
   - (b) Draw them in the air pass on top. This is wrong only if a building stands between the endpoints.
   - (c) Split them into per-tile segments, each sorted at its midpoint. This is more calls and has glitch risk, since OpenTTD warns that overlapping boxes cause more problems.
6. **Occluded-drone silhouettes** (optional): for each drone whose screen rect overlaps a building sorted after it, draw a pre-baked solid-colour silhouette at about 35% alpha on top.
   - Make the silhouette once per frame image with `globalCompositeOperation = 'source-in'` on an offscreen canvas.
   - Drawing it everywhere the drone overlaps the building, not only the hidden pixels, is a cheap approximation of the 0 A.D. effect.
7. **Lighting or darkness mask**: one viewport-sized offscreen canvas.
   - Each frame, fill it with `rgba(0,0,0,darkness)`, switch to `'destination-out'`, and draw radial gradients for each light source to cut holes.
   - Draw the mask onto the main canvas with `source-over`. Add coloured glows afterwards with `'lighter'`.
   - The mask is screen-space, so a tall building's roof is lit by whatever light sits behind it on screen. This is usually fine for stylised art.
   - The compositing modes come from general Canvas API knowledge and were not fetched this session.
8. **UI and text**: health bars, labels, cursor. Draw text last, as OpenTTD does with strings. Pre-render text if it is frequent, per MDN's "avoid text rendering."

### Gaps
- I did not fetch MDN's `globalCompositeOperation` page this session, so the compositing-mode details in pass 7 are unsourced here.
- I found no source that discusses the "fog overlay darkens tall buildings behind it" artefact directly. That is my inference from Flare's layer order.

---

## 6. Picking: which tile or object is under the mouse when tall sprites overlap tiles behind them

### Takeaway
Keep two separate queries. **Object pick:** walk the final draw list front to back (reverse draw order), test the sprite's screen rect, then its alpha at the cursor pixel; the first opaque hit wins. **Ground pick:** invert the projection to get the floor tile, ignoring sprites. Use the object pick for selecting and the ground pick for placing and targeting. When a tall sprite hides the floor tile you need, fade it or show the hover diamond on top.

### Cited Findings
- Screen-to-map inversion: `map.x = (screen.x / TILE_WIDTH_HALF + screen.y / TILE_HEIGHT_HALF) / 2; map.y = (screen.y / TILE_HEIGHT_HALF - screen.x / TILE_WIDTH_HALF) / 2`, applied after removing the camera and origin offset. — [Clint Bellanger](https://clintbellanger.net/articles/isometric_math/)
- Pixel-perfect sprite picking: do a cheap bounding-box test first, then check the sprite's pixel at the mouse position and accept only if it is not transparent. (search snippet) — [DEV Community, Pixel-Perfect Sprite Clicks](https://dev.to/joestrout/pixel-perfect-sprite-clicks-20dl)
- Picking the "top-most" tile per column breaks once terrain is extruded upward, because the wrong tile is picked. A suggested fix is a separate pass that computes top-tile positions outside the render loop. (search snippet) — [JVM-Gaming forum](https://jvm-gaming.org/t/picking-the-top-tile-in-isometric-games-images-included/53454)
- With tall tiles, a single tile can be overlapped by up to four others, and depth sorting alone doesn't resolve picking. (search snippet) — [GameDev.net, Advanced isometric engine](https://gamedev.net/forums/topic/620645-advanced-isometric-engine/4915574/)
- AoE players rely on unit outlines to select units hidden behind buildings, so picking hidden units is a real usability issue. (search snippet) — [AoE forum](https://forums.ageofempires.com/t/suggestion-unit-outlines-for-better-and-faster-identification-selection-management/159612)

### Inferences
**Implementation sketch:**
```js
// At load: build an alpha mask per sprite frame
// (getImageData once; store a Uint8Array with 1 byte per pixel).
function pickObject(mx, my, drawList) {          // drawList is back-to-front
  for (let i = drawList.length - 1; i >= 0; i--) {
    const o = drawList[i];                          // dx/dy as drawn (already rounded)
    const lx = mx - o.dx, ly = my - o.dy;
    if (lx < 0 || ly < 0 || lx >= o.sw || ly >= o.sh) continue;
    if (o.mask[(ly | 0) * o.sw + (lx | 0)] > 127) return o.entity;  // entity, not tile
  }
  return null;
}
function pickTile(mx, my) {   // ground only
  const sx = mx - originX, sy = my - originY;
  const fx = (sx / TW2 + sy / TH2) / 2, fy = (sy / TH2 - sx / TW2) / 2;
  return { x: Math.floor(fx), y: Math.floor(fy) };   // origin = top corner of tile (0,0)
}
```
- Convert mouse coordinates to canvas pixels first, accounting for CSS scale and `devicePixelRatio` (see Q7).
- Reuse the exact `dx/dy` rounding from drawing so the mask lines up.
- Exclude non-interactive items from the object pick: decals, projectiles, fog.
- Optionally give drones an enlarged hit radius, since small sprites are hard to click.

**Hidden tiles behind the 3x3 base.** In placement or targeting mode, draw the hover diamond and the placement ghost in an overlay pass, after objects, so they show through the building. Or temporarily draw buildings at reduced alpha when the cursor's ground tile sits behind them, Flare-style fading. Use `pickTile` for the action itself.

**Alternative: an ID colour buffer.** Draw each object as a flat unique colour into a hidden canvas, then read one pixel. This costs a second full draw and needs `imageSmoothingEnabled = false` and integer coordinates to avoid blended IDs. Reverse-order mask testing is cheaper for a small board. Neither technique is sourced in these notes.

### Gaps
- I found no canonical, authoritative write-up of front-to-back alpha-mask picking for isometric engines. The GameDev.net archive article "Object Selection" (article740) returned HTTP 403. The recommended approach is a synthesis of the snippets above.

---

## 7. Canvas 2D performance: caching static layers, integer pixel snapping, imageSmoothingEnabled=false, integer scaling

### Takeaway
Pre-render the floor and static decals to an offscreen canvas and blit it once per frame. Redraw only dynamic content on top. Snap every `drawImage` destination to whole pixels. Set `imageSmoothingEnabled = false`. Render at native pixel-art resolution and upscale by an integer factor, either via CSS with `image-rendering: pixelated` or via `drawImage` at integer multiples. Sorting a few hundred objects per frame is negligible. OpenTTD-scale O(n²) sorting only hurts at around 100k sprites.

### Cited Findings
- MDN, offscreen pre-rendering: "If you find yourself repeating some of the same drawing operations on each animation frame, consider offloading them to an offscreen canvas." — [MDN, Optimizing canvas](https://developer.mozilla.org/en-US/docs/Web/API/Canvas_API/Tutorial/Optimizing_canvas)
- MDN, integer coordinates: "Sub-pixel rendering occurs when you render objects on a canvas without whole values," which forces extra anti-aliasing work. Round `drawImage` coordinates, e.g. with `Math.floor()`. — [MDN, Optimizing canvas](https://developer.mozilla.org/en-US/docs/Web/API/Canvas_API/Tutorial/Optimizing_canvas)
- MDN, don't scale images in `drawImage`: cache sizes on an offscreen canvas at load. — [MDN, Optimizing canvas](https://developer.mozilla.org/en-US/docs/Web/API/Canvas_API/Tutorial/Optimizing_canvas)
- MDN's other tips:
  - Use layered canvases (static background, game, UI).
  - Use `getContext('2d', { alpha: false })` when no transparent backdrop is needed.
  - Batch calls and avoid unnecessary state changes.
  - Avoid `shadowBlur` and text rendering.
  - Render only screen differences, and use `requestAnimationFrame`.
  - CSS transforms scale on the GPU, and "the best case is to not scale the canvas, or have a smaller canvas and scale up."

  — [MDN, Optimizing canvas](https://developer.mozilla.org/en-US/docs/Web/API/Canvas_API/Tutorial/Optimizing_canvas)
- `imageSmoothingEnabled` "determines whether scaled images are smoothed (`true`, default) or not (`false`)"; "Set this property to `false` to retain the pixels' sharpness." — [MDN, imageSmoothingEnabled](https://developer.mozilla.org/en-US/docs/Web/API/CanvasRenderingContext2D/imageSmoothingEnabled)
- MDN's crisp pixel-art setup: give the canvas its native low-res `width`/`height` attributes, scale it up with CSS (e.g. 128 to 512px for 4x), and set `image-rendering: pixelated`. When drawing scaled, `dWidth` must equal `sWidth / xScale * n` for an integer `n`. A non-integer `devicePixelRatio` makes "certain pixels ... drawn larger than others," which "is not an easy problem to solve." — [MDN, Crisp pixel art look](https://developer.mozilla.org/en-US/docs/Games/Techniques/Crisp_pixel_art_look)
- OpenTTD's O(n²) sorter can take seconds at about 150k sprites. Its main sorter relies on nearly-sorted input from tile-scan order to keep moves cheap. — [OpenTTD viewport.cpp](https://github.com/OpenTTD/OpenTTD/blob/master/src/viewport.cpp); [OpenTTD SSE4 sorter (search snippet)](https://docs.openttd.org/source/d4/d7d/viewport__sprite__sorter__sse4_8cpp_source.html)
- Edge building for a topological sort is O(|V|²). — [Bannalia](https://bannalia.blogspot.com/2008/02/filmation-math.html)

### Inferences
**Frame loop for this game:**
1. `ctx.drawImage(floorCache, -camX|0, -camY|0)`. The cache holds floor, static decals and ground fog. Rebuild it when a building is placed or destroyed, or visibility changes.
2. Draw dynamic decals.
3. Build the object array: keep static structures in a persistent array and append drones and projectiles. Sort by `(key, id)`. Insertion sort on last frame's order is near O(n) because the order barely changes, the same nearly-sorted idea as OpenTTD. Plain `Array.sort` is also fine at this scale.
4. Draw objects, then the air pass, light mask and UI.

**Scaling strategy.**
- Pick a fixed internal resolution, e.g. 480×270 or 640×360.
- Size the canvas backing store to `internal × n`, where `n = floor(min(viewportCSSpx × devicePixelRatio / internal))`. Set its CSS size to `backing / devicePixelRatio`.
- `ctx.setTransform(n, 0, 0, n, 0, 0)` and `ctx.imageSmoothingEnabled = false`. Round positions in internal pixels before scaling.
- Or keep the backing store at internal resolution and let CSS `image-rendering: pixelated` scale it. That is simpler, but on non-integer DPR displays it suffers MDN's uneven-pixel issue.
- Letterbox the leftover space rather than using a fractional scale.

**Re-apply `imageSmoothingEnabled = false`, and the transform, after every canvas resize.** Setting `width`/`height` resets the 2D context state to defaults (my understanding of the HTML spec; not verified this session). The same applies to every offscreen cache context.

**Pixel snapping when the camera moves.** Round the camera offset once and add integer sprite offsets, rather than rounding each sprite's float position separately. This avoids 1-px shimmer between neighbouring sprites and between a building and its child sprites. Store sub-pixel world positions for movement, and round only at draw time.

**Glows and lights.** Pre-render glows, muzzle flashes and radial light sprites to offscreen canvases once. Avoid `shadowBlur` per frame, as MDN advises. Draw additive effects with `'lighter'`, grouped so you switch composite mode once per pass, not once per sprite.

**Static building caching.** Baking static buildings into the floor cache is unsafe once drones can pass behind them. Redraw buildings every frame; on a small board that is a few dozen `drawImage` calls. An optimisation I found no source for: draw a "static scene cache" and then, for each drone, redraw only the static sprites that sort after it and overlap its screen rect.

### Gaps
- I found no benchmark numbers for `drawImage` call budgets on modern browsers for this workload. The claims that "a few hundred sprites is negligible" and "200² comparisons < 1 ms" are engineering estimates, not measurements.
- The claim that resizing resets the canvas context state, including `imageSmoothingEnabled`, was not confirmed from a fetched source this session. The MDN `imageSmoothingEnabled` page does not mention resizing.
