# Overgrown Brutalist and Cold War Architecture in Small-Scale Isometric Pixel Art

Method note: Saint11 (Pedro Medeiros) publishes his tutorials as 512x512 GIF images with no page text. The Saint11 findings below come from reading the final frames of those images directly (Ruins, Isometric, Vegetation 1 and 2, Metals, City Backgrounds, Rock Formations, Illumination Techniques). Each one is cited by its image URL. "Inferences" sections hold pixel-level rules I derived for this project's target scale (3x3 tiles of a 2:1 grid, about 96–128px wide). No source states them directly.

---

## 1. Concrete: texturing at small scale without visual noise

### Takeaway
The sources agree: keep large concrete areas clean and imply texture with a few repeated, deliberate marks. Those marks are paired dark/light seam lines, small erosion points and tree-like cracks, placed where light and weathering logic put them. Never cover a face with uniform noise or dither. Orphan pixels and too many detail colors are the main causes of noise.

### Cited Findings
- **Imply texture and leave areas empty.** "A perfectly homogenous texture can appear unnatural and too busy." Put detail in select areas and leave others empty so the texture is implied. Drawing every brick is "noisy", and emphasizing brick outlines "can look very busy". For a rougher look, add an occasional shadow or highlight on a few select blocks only. — [Slynyrd, Pixelblog 2: Texture](https://www.slynyrd.com/blog/2018/2/15/pixelblog-2-texture)
- **Orphan pixels.** Single pixels not connected to a same-colored cluster are usually avoided. They are acceptable only when they "relate to a greater texture". — [Slynyrd, Pixelblog 2](https://www.slynyrd.com/blog/2018/2/15/pixelblog-2-texture)
- **Repeat a few clusters.** Use a few clusters and repeat them, varying their distribution. — [Slynyrd, Pixelblog 2](https://www.slynyrd.com/blog/2018/2/15/pixelblog-2-texture)
- **Concrete panel grooves.** For a concrete groove, use "one slightly darker shade of wall color, right next to a slightly lighter one", applied vertically and then horizontally. — [Tutsplus, Isometric Pixel Art Office Building](https://design.tutsplus.com/tutorials/create-an-isometric-pixel-art-office-building-in-adobe-photoshop--cms-23357)
- **Subtle wall texture.** The same tutorial makes a wall texture from "lines of 2 px alternating" at 20% opacity. It gives a rooftop concrete surface a "soft grainy texture" with softened edges. — [Tutsplus office building](https://design.tutsplus.com/tutorials/create-an-isometric-pixel-art-office-building-in-adobe-photoshop--cms-23357)
- **Saint11's step order for a weathered urban wall:** basic block shapes, then a light source and highlights, then "erosion points and small cracks — don't forget about the light direction", then "make the brick division deeper in some points", then "reduce contrast adding a lighter shade/light on the details". — [Saint11, City Backgrounds tutorial](https://saint11.art/img/pixel-tutorials/City.gif)
- **Cracks and bumps on ruins.** Cracks: "do NOT exaggerate", use a "tree-like pattern". Bumps: "small irregular shapes, a little noisy". Missing parts: "add BG and inside object", meaning show what is behind and inside the break. Also "uneven surfaces" and "break symmetry". — [Saint11, Ruins tutorial](https://saint11.art/img/pixel-tutorials/Ruins.gif)
- **A crack is more than a black line.** "Drawing a black line across a texture doesn't really suffice as a crack." Cracks need shape and shading. — [Superwalrusland pixel art guide](https://superwalrusland.com/ohr/issue26/pa/pixelart.html) (search excerpt)
- **Grime.** Strong value contrast plus "a little noise" can make wall tiles look "a little grimy & wrecked". — [gamedev.net, "Miles of Tiles" devlog](https://gamedev.net/blogs/entry/2262738-i-am-overburdened-miles-of-tiles) (search excerpt)
- **Hard-stone analog.** Saint11's tutorial on dense ("metamorphic") rock calls for "large clean areas", "stronger highlights", breaks "in larger chunks", and a warning: "avoid orphan pixels and adding too much noise / don't use too many colors on details". — [Saint11, Rock Formations](https://saint11.art/img/pixel-tutorials/Rock.gif)
- **Tile craft.** Draw the grout or seam outlines first, then fill, then add discoloration and damage. Make at least one variant (cracked or discolored) of any heavily used texture. Switching grout from light to dark changes the look of a tile dramatically. Grout is a 1px stroke. — [Slynyrd, Pixelblog 45: Bricks, Walls, Doors](https://slynyrd.com/blog/2023/7/21/pixelblog-45-bricks-walls-doors-and-more)
- **How the real material looks.** Raw concrete is often left with "shuttering" marks from the forms, which gives a rough, board-textured surface. Concrete in damp, cloudy climates can "become streaked with water stains", with moss and lichen growth. Rust stains come from steel reinforcing bars. — [Wikipedia, Brutalist architecture](https://en.wikipedia.org/wiki/Brutalist_architecture)
- **Where stains form.** Runoff can stain a wall, and rust from iron elements stains adjoining surfaces. Rain cleans exposed areas, while sheltered, poorly ventilated or saturated masonry can "darken and stain". White salt efflorescence often shows as "white curves". — [Designing Buildings Wiki, Pattern staining and soiling](https://www.designingbuildings.co.uk/wiki/Pattern_staining_and_soiling_in_stonework)
- **Streaks below windows.** Search excerpts from facade-weathering sources say water runs off sills, picks up dirt and streaks down the face. Glass sheds wind-driven rain onto the wall below, "especially at the corners" (sometimes called "mustaching"). Drip edges prevent this. — [PCI "Designer's Notebook: Weathering" (search excerpt; PDF returned 404 when fetched)](https://www.pci.org/PCI_Docs/Publications/Ascent%20Magazine/2013/Summer/Designer's%20Notebook_Weathering.pdf)

### Inferences
- **Value budget per face.** Each concrete face gets one base value and covers 70–85% of its area with it. Texture uses at most one step darker and one step lighter from the same ramp. Saint11's "don't use too many colors on details" and Slynyrd's "leave areas empty" both point this way.
- **Panel seams at 96–128px.** Draw a seam as a 1px line one step darker, with a 1px line one step lighter on the lit side (the Tutsplus groove). Use them only at module boundaries, every 8–16px along a face, not on every panel. Vertical seams stay perfectly vertical. Horizontal seams follow the 2:1 slope of the face.
- **Board-formed texture.** At this scale, only show it on large blank surfaces such as stair or service cores, blank end walls and the plinth. Use broken horizontal (2:1) 1px lines one ramp step apart, every 2px, with gaps of 3–6px so they read as grain, not stripes. This is the indexed-palette version of the Tutsplus "2px alternating lines at 20% opacity". If you have no intermediate color, leave the board texture out. It is the first detail to drop.
- **Below-window streaks.** Hang 1px-wide streaks one step darker from the corners of sills and spandrel ledges, 3–8px long. Break the bottom 1–3px into separated pixels so the streak fades. Use 1–3 streaks per facade module and stagger them, not one under every window. Add rust streaks in a warm brown only below spalls or exposed rebar.
- **Shelter darkening.** Darken the 2–4px directly under cantilevers, parapet overhangs and inside deep recesses by one step, sheltered areas being the ones that "darken". Keep the top 1px of every horizontal ledge in the lightest top-face tone so the ledge line reads crisply.
- **Cracks.** Draw a crack as a tree-like 1px path in the darkest face tone. Add a 1px lighter edge on the side facing the light. Keep it short: 4–10px on a 96–128px building, with at most 2–3 per visible face.
- **Spalls and missing chunks.** Notch the silhouette at a corner by 2–4px and show the darker interior and exposed rebar (1px rust-brown pixels) inside. This applies Saint11's "the silhouette should also look ruined" and "add BG and inside object".
- **Dither.** Avoid checkerboard dither on concrete faces at this scale, because it reads as noise and turns grey into mud. If you use dither, keep it to a 1–2px transition band, for example a fading stain, between two adjacent ramp steps.

### Gaps
- I found no pixel-art tutorial devoted to concrete or board-formed texture. The concrete rules above are adapted from brick, stone and wall tutorials.
- The full PCI weathering document could not be retrieved (404), so the "mustaching at window corners" detail rests on a search excerpt only.

---

## 2. Brutalist massing in iso: horizontal bands, deep recesses, cantilevers, repeated modules

### Takeaway
Brutalism reads through heavy, near-monochrome masses, repeated modules and deep-set openings. In 2:1 iso, build it by carving cuboids, then expressing modules with bright ledge-top lines and shadowed recesses rather than outlines. Show recess depth geometrically: a 1–2px jamb on the side away from the near corner, a 1px sill highlight and a shadowed top row.

### Cited Findings
- **Real brutalist form.** Forms are often "massive in character (even when not large)", the geometry is angular and the palette is "predominantly monochrome". Designs use "recurring modular elements representing specific functional zones". Stepped or terraced "ziggurat" forms break up bulk. Some buildings project portions of the facade to signal interior functions (Boston City Hall). — [Wikipedia, Brutalist architecture](https://en.wikipedia.org/wiki/Brutalist_architecture)
- **Exposed services.** Structure, pipes, conduits and water tanks are shown, not hidden. At Hunstanton the water tank sits in a "prominent, visible tower" and utilities run in "readily visible pipes and conduits". — [Wikipedia, Brutalist architecture](https://en.wikipedia.org/wiki/Brutalist_architecture)
- **A federal reference.** The FBI J. Edgar Hoover Building's design includes "exposed concrete, deep window recesses, strong, powerful massing and monumentality". — [GSA, FBI Building History](https://origin-www.gsa.gov/real-estate/gsa-properties/visiting-public-buildings/j-edgar-hoover-fbi-building/whats-inside/history) (search excerpt). Its exterior is buff-colored precast and cast-in-place concrete with "repetitive, square, bronze-tinted windows set deep in concrete frames". — [Wikipedia, J. Edgar Hoover Building](https://en.wikipedia.com/wiki/FBI_HQ) (search excerpt)
- **Deep-set windows in general.** Brutalist buildings feature "exposed concrete… stark geometric patterns, deep-set windows, simple lines and unpainted surfaces". — [Smithsonian Magazine](https://www.smithsonianmag.com/smart-news/washingtonians-love-to-hate-brutalist-architecture-but-what-if-we-could-fix-it-180984968/) (search excerpt)
- **Strip windows.** Harvard GSD describes a brutalist dormitory with "immense strip windows". — [Harvard GSD](https://www.gsd.harvard.edu/?p=2450339) (search excerpt)
- **Iso fundamentals.** Use a 2:1 ratio ("for every 2 horizontal pixels… move vertically by one pixel"). "Vertical lines are always straight." "Lines can be replaced by transitions and bright corners." "Use cuboids to create basic shapes and carve the desired shape out of it." "Apply texture after the basic shape is solved." Drawing flat and converting to iso is "a useful tool, but don't rely only on this." — [Saint11, Isometric Part 1](https://saint11.art/img/pixel-tutorials/Isometric.gif)
- **Grid discipline.** Use a 2:1 step line (about 26.5°), not a true 30° line, which looks jaggy. Keep a 2:1 ruler on its own layer, count pixels and "don't fully trust your eyes". Tiles are cuboids whose heights can vary as long as the top surface is uniform. Even dimensions are "your friend". Wide-corner cubes look better but must overlap to tile. Sharp-corner cubes tile perfectly. — [Slynyrd, Pixelblog 41: Isometric](https://www.slynyrd.com/blog/2022/11/28/pixelblog-41-isometric-pixel-art)
- **Base cube.** A 32x32px cube whose top diamond has 2px corners at left and right. Adjacent cubes must be layered in a consistent order so the top row of pixels overlaps flush. — [Slynyrd, Pixelblog 61: Isometric Mecha Tactics](https://www.slynyrd.com/blog/2026/4/1/pixelblog-61-isometric-mecha-tactics)
- **Light and edges.** Pick one light source. Strong directional light should give "clear contrast between the right-facing and left-facing sides". Bevel edges and use subtle outlining. — [Slynyrd, Pixelblog 54](https://www.slynyrd.com/blog/2025/1/23/pixelblog-54-isometric-pixel-art)
- **Face shading values.** Make the top lightest and the right side darkest, varying brightness by "8% to 15%". Lower contrast on inside corners. Add a line every 4px between levels as a guide, leave an extra line for a roof ledge, and let the base protrude slightly at the corners. "Cleaner can often be better than accurate in pixel art." — [Tutsplus, Isometric Pixel Art Factory](https://design.tutsplus.com/tutorials/create-an-isometric-pixel-art-factory-in-adobe-photoshop--cms-25351)
- **Corners and outlines.** "Make the peak corners a lighter shade than the walls." "Most corners don't need to be black." Borders touching walls or floors should be "a bit lighter than black". Duplicate floors as modules, and repeat only the concrete portion for a roof-access level. — [Tutsplus office building](https://design.tutsplus.com/tutorials/create-an-isometric-pixel-art-office-building-in-adobe-photoshop--cms-23357)
- **Breaking repetition.** "It's nice to let the texture breathe" so a facade isn't just a repeated window grid. — [Tutsplus, Isometric Apartment (search excerpt)](https://design.tutsplus.com/tutorials/crea-un-apartamento-de-arte-de-pixel-isometrico-en-adobe-photoshop--cms-23006)

### Inferences
- **Scale sheet for a 96–128px, 3x3-tile compound.** With 32x16 tiles, a 3x3 footprint is a 96x48 diamond. A storey of about 6px works: 2px window band, 1px reveal/shadow row, 3px spandrel. A 4-storey HQ block is then about 24px of facade, plus a 3–4px plinth and a 2px parapet, roughly 30px of wall. The full sprite lands around 96x80–100px before masts and the dish. At 42px tiles (128px wide), use a 7–8px storey. Keep storey height constant across all compound buildings so the modules read as one system.
- **Recess geometry in 2:1.** A recess d px deep on the **left** face shifts its back plane right d px and up d/2 px. On the **right** face it shifts left d px and up d/2 px. Practical rules:
  - **Left-face opening:** a jamb d px wide on the opening's left edge in the right-face (shadow-side) tone, a 1px sill-top line at the bottom in the top-face (lightest) tone when d=2, and a darkest 1px row along the top of the glass (lintel shadow).
  - **Right-face opening:** mirror it, with the jamb on the right edge in the left-face tone.
  - The visible reveal is always on the side of the opening farther from the building's front corner. With d=1, draw the jamb only at the ends of a ribbon band and rely on the 1px top shadow row.
  - This matches the Tutsplus factory's "L-shaped darker glass shade parallel to the far edges".
- **Horizontal bands.** Make spandrels continuous unbroken bands. Give each spandrel or ledge a 1px top-edge highlight in the top-face tone, Saint11's "bright corners" in place of lines. Put the brightest pixels on the vertical front corner, Tutsplus's "peak corners lighter".
- **Cantilevers.** Undersides are invisible from the iso camera. Show a cantilever by (a) stepping the silhouette out 2–4px at the corner and (b) a 2–4px shadow band on the wall directly below it, following the 2:1 slope on both faces. The shadow sells the overhang.
- **Modules and piers.** Break long ribbon bands every 6–10px with a 1–2px vertical pier in the lit concrete tone. Vary 1 in 4 or 1 in 5 modules (a blank panel, a stair-core slot, a projecting box) so the grid "breathes". Express one or two big functional volumes, such as a windowless auditorium box or a stair or elevator tower, as separate projecting cuboids, as Boston City Hall does.
- **Plinth and podium.** A heavier, darker, 1–2px protruding base (Tutsplus's protruding base) grounds the mass and gives moss a ledge to sit on.
- **Exposed services as detail budget.** Use visible roof plant, pipes and a tank or service tower as the compound's detail. This is architecturally faithful and gives the dish and masts a logical base.

### Gaps
- No pixel-art source states reveal depths in pixels. The recess rules above are derived from 2:1 projection geometry.
- The sources found did not document ribbon windows or cantilevers as defining brutalist traits beyond the Harvard GSD "strip windows" excerpt. One search-result page describing "deep-set ribbon glazing" with "vertical pier strips" (archivinci.com) reads like AI image-prompt text, so I excluded it as a source.
- I did not fetch the ArchDaily essay "Brutalism and Bureaucracy: An Architectural Language of Authority in the Postwar United States", which may help with the federal compound brief. ([ArchDaily](https://www.archdaily.com/1027169/brutalism-and-bureaucracy-an-architectural-language-of-authority-in-the-postwar-united-states))

---

## 3. Windows: ribbon bands, dark glass, sparse lit windows, reflections in 2:1 iso

### Takeaway
Treat glass as a nearly flat dark band. Its seams are only slightly darker than the glass, there is a darker zone under each overhang, and the lit corner is lighter. Add a small, focused sheen highlight and very few lit (emissive) segments. Do not try to render true reflections.

### Cited Findings
- **Glass seams and shadow zones.** Glass seams should be "just slightly darker than the surrounding colors". For darker glass zones, the tutorial offsets the floor-top selection down about 10px and paints the overlap a darker glass shade, which acts as the shadow of the slab or overhang. Make the front glass corner lighter. Frame example: "One pixel of grey was enough vertically. For the bottom line I left 3 px height." — [Tutsplus office building](https://design.tutsplus.com/tutorials/create-an-isometric-pixel-art-office-building-in-adobe-photoshop--cms-23357)
- **Glass color and shading.** Use a glass tone "that isn't too bright or saturated", an "L-shaped darker glass shade parallel to the far edges" and rails at regular intervals. Slope the bottom frame of long windows on a 1:1 diagonal. — [Tutsplus factory](https://design.tutsplus.com/tutorials/create-an-isometric-pixel-art-factory-in-adobe-photoshop--cms-25351)
- **Sheen.** Add "a reflective sheen to windows". — [Slynyrd, Pixelblog 54](https://www.slynyrd.com/blog/2025/1/23/pixelblog-54-isometric-pixel-art)
- **Simple glass.** Clear glass is hard because "you can't predict what will be behind the object". Most pixel art handles it through front reflection and internal reflection or refraction. A simpler route is "just add a bluish-greenish solid area for the glass". — [Pixel Joint forum WIP thread (search excerpt)](https://pixeljoint.com/forum/printer_friendly_posts.asp?TID=25956)
- **Highlights on glossy surfaces.** Glossy and reflective objects have "small and focused highlights". — [Pixel Grimoire, How to start making pixel art #4](https://medium.com/pixel-grimoire/how-to-start-making-pixel-art-4-f57f51dcfa02) (search excerpt)
- **Easy reflections.** "For easy reflections do multiple lines of light/shadow." "Reflections do not work like this, but if it looks cool who cares." — [Saint11, Metals](https://saint11.art/img/pixel-tutorials/Metal.gif)
- **Real-world tint.** The FBI HQ uses "bronze-tinted windows set deep in concrete frames". — [Wikipedia, J. Edgar Hoover Building](https://en.wikipedia.com/wiki/FBI_HQ) (search excerpt)
- **Emissive windows.** In emission-map workflows, "everything is black except the windows of the building, so only the windows emit a glow". — [Megacat Studios, Emission Map Guide](https://megacatstudios.com/blogs/game-development/emission-map-guide-for-artists) (search excerpt)

### Inferences
- **Ribbon band construction.** A ribbon band is a 2px-tall strip of dark glass following the face's 2:1 slope. Use two glass colors: a darkest row on top (the lintel or overhang shadow) and a slightly lighter glass row below. Add the reveal jamb from Section 2 at band ends. Put mullions every 4–6px as 1px vertical lines only one step lighter than the glass, or leave them out at 96px width.
- **Glass hue.** For a 1990s federal building, use dark bronze/brown-grey or dark blue-grey glass, not bright cyan. Bright glass fights the concrete for attention, and dark glass makes the band read as a deep horizontal slot from far away.
- **Reflection in 2:1.** Use one or two short highlight streaks per face: 1px lines or 2–3px runs in a pale cool tone on the glass, placed toward the lit end of each band. Use the same diagonal (for example, 1:1) across every window on that face. The single consistent direction makes it read as reflection instead of noise. Add a 1px brightest pixel at the building's front corner where the two glass bands meet.
- **Sparse lit windows.** Light only 5–15% of glass segments, in clusters of 1–2 adjacent segments (2–4px wide each), placed irregularly across floors. One fully lit floor segment can be a storytelling accent, such as a still-occupied ops room. In a ruined or overgrown state, use fewer lights: 0–3 per building plus one emergency red.
- **Glow without gradients.** A lit window is the warm emissive mid color, a 1px hot core at its center or top only on large windows, and an optional single 1px halo pixel in the darkest warm step on the concrete directly below the sill. Keep glow as an engine light or emission layer, not painted gradients, so day and night can share one sprite.

### Gaps
- No source gave a pixel-level method for reflections specifically on 2:1 iso glass. The reflection rules above are inference.
- Saint11's glass or "Shine" tutorial (Shine.gif) was not read. It may give more specific reflection rules ([Saint11 tutorial index](https://saint11.art/blog/pixel-art-tutorials/)).

---

## 4. Overgrowth: moss on top faces, vines on vertical faces, shrubs and trees, and how much green is too much

### Takeaway
Build overgrowth from small, consistent leaf clusters (for example 2x2px units), layered as shadow, base and highlight. Put the detail and contrast at the borders and keep interiors calm. Old growth should be lower-contrast with rounded edges. Vines should hang and trail with gravity from ledges and roofs. Green must never break the building's silhouette or its three-face value structure.

### Cited Findings
- **Leaf cluster unit.** Use "a small 2x2px square as its base geometric unit". Squares may touch but should not overlap. "Clusters consistent in shape, size and spacial distribution are key to achieving a pleasing overall foliage texture." "4 or 5 colors per bundle is plenty." Shade bundles like spheres: a darker crescent at the bottom-left and a lighter smaller circle at the upper-right, then hand-place highlights and clean stray pixels where bundles overlap. — [Slynyrd, Pixelblog 44: Top Down Trees](https://www.slynyrd.com/blog/2023/5/22/pixelblog-44-top-down-trees)
- **Abstraction.** "Embrace abstraction." Not every leaf needs to be drawn, because the viewer fills gaps. "Too much variation will create displeasing noise." — [Slynyrd, Pixelblog 15: Plant Life](https://www.slynyrd.com/blog/2019/3/7/pixelblog-15-plant-life)
- **Saint11 Vegetation Part 1:**
  - The silhouette should be recognizable "even without the colors".
  - "The light must show that the tree has volume."
  - "Keep the color count as low as possible."
  - Use "detailed borders, clear internal areas".
  - "I try to detail without adding too much noise / I avoid orphan pixels / Think in layers of foliage."
  - "Repeating patterns is a good way to avoid noise" and "Keep a leaf pattern for consistency".
  - "Hanging vines are always awesome."
  - Gradient: warm light and cold shadow, with "almost half of the gradient… light green", and "create resting areas for the eyes".
  — [Saint11, Vegetation Part 1](https://saint11.art/img/pixel-tutorials/Vegetation.gif)
- **Saint11 Vegetation Part 2:**
  - Think in layers: highlights, base, shadow and dirt, and "think in 3 dimensions".
  - "Hanging vines work here too", drawn on ledge overhangs.
  - Avoid too many colors, orphan pixels and noise.
  - "Different depths and patterns break monotony."
  - "Big empty areas give focus to other areas."
  - "Don't overdo the details" and "Each detail must be conscious; don't throw random dots around."
  - "Borders have more detail and contrast."
  — [Saint11, Vegetation Part 2](https://saint11.art/img/pixel-tutorials/Vegetation2.gif)
- **Saint11 Ruins:** "Old ruins might have plants growing on it; low contrast and rounded edges." "Hanging cables, wires, vines etc." "Ruins should always tell a story." "Repeat objects to show different levels of destruction." "Cobwebs: don't make them too bright." — [Saint11, Ruins](https://saint11.art/img/pixel-tutorials/Ruins.gif)
- **Iso trees.** The trunk should "strictly conform" to the 2:1 pattern. Root paths follow roughly 2:1 directions. The trunk is a tapered cylinder built from two ovals. Foliage bundles are roughly 3:2 ovals. Clean unsightly overlap clusters, add a cast shadow and "nestle trees in grass tufts". Use minor dithering to soften harsh clusters. — [Slynyrd, Pixelblog 54](https://www.slynyrd.com/blog/2025/1/23/pixelblog-54-isometric-pixel-art)
- **Start from geometry.** Start foliage and organic objects "with basic geometric shapes, then sculpt in details" to keep the iso look consistent. — [Slynyrd, Pixelblog 41](https://www.slynyrd.com/blog/2022/11/28/pixelblog-41-isometric-pixel-art)
- **Where growth goes.** Weathered concrete in damp climates gets water stains plus moss and lichen. — [Wikipedia, Brutalist architecture](https://en.wikipedia.org/wiki/Brutalist_architecture). Sheltered, poorly ventilated or oversaturated masonry darkens and stains. — [Designing Buildings Wiki](https://www.designingbuildings.co.uk/wiki/Pattern_staining_and_soiling_in_stonework)
- **Ivy method (non-pixel).** A Photoshop tutorial keeps ivy as a continuous string of leaves on one layer, with a separate shadow layer beneath it on the wall. — [Tutsplus, Classic Ivy Running Up a Building](https://design.tutsplus.com/tutorials/create-classic-ivy-running-up-a-building--psd-5148) (search excerpt)
- **Lush vines are a known difficulty.** Pixel artists in an OpenGameArt thread report trouble making "thick and lush" vines read correctly. — [OpenGameArt help/feedback thread](https://opengameart.org/forumtopic/helpfeedback-requested)

### Inferences
- **Moss on top faces.** Use clusters, not checker dither. Place 2x2 and 3x2 clumps of a desaturated moss mid-green on roof slabs, parapet tops, ledges, the helipad edge and the plinth. Concentrate them toward the back corners and along the bases of walls and parapets (the sheltered, wet spots) and thin them toward the exposed center. Let 1–2px of concrete top-face tone show between clumps. At the clump edge facing the light, add 1px highlight pixels. Moss values should sit between the top-face and side-face concrete values so the top still reads as the brightest plane.
- **Moss creep onto vertical faces.** Let moss drip 1–3px down from ledges as irregular 1px-wide fingers. It should be heaviest under drips and at the base splash zone (bottom 2–4px of walls).
- **Vines.** Draw a stem as a 1px darkest-green line hanging from a roof edge, parapet or cantilever. It follows gravity mostly vertically, with occasional 1px sideways jogs, and drapes over ledge lines. Add leaf clusters of 2x2 or 3x3 along the stem, larger and denser near the top (the source) and thinning to single tendrils at the bottom. Use 3 values on each cluster (shadow, base, highlight) and put 1–2 highlight pixels on the light-facing side. Add a 1px cast-shadow pixel on the concrete beside clusters on the side away from the light, from the ivy "shadow layer" idea. Climbing vines (from the ground up) should hug the front corners and the stair or service cores. They should spread in a fan, never as an even sheet.
- **Cables as vines.** Use sagging 1px cables between masts, the dish and building edges in a dark neutral. Saint11 groups "cables, wires, vines" together. Cables and vines can share drape logic so the compound reads as both technological and decayed.
- **Shrubs.** Use 6–12px bushes made of 3–5 bundles, each a 2:1-ish rounded mass. Give them sphere shading, a 1px ground shadow ellipse and a few grass tufts. Use them to hide the base line of fences and the gatehouse.
- **Trees.** Make a 16–28px-tall tree (a 2–4px tapered trunk plus 3–5 foliage bundles, each a 3:2 oval) at one or two corners of the compound. Use a tree to break the roofline once, not more.
- **How much green.** Keep overgrowth to about 20–35% of a building's visible surface. Keep at least these clean:
  - (a) the front vertical corner from roof to ground, or most of it
  - (b) the full silhouette of the roofline at every corner except one or two
  - (c) at least two complete ribbon bands per face, so the modular rhythm stays legible
  - (d) the top-face, left-face, right-face value order
  To test, view the sprite as a flat single-color silhouette (Saint11's "recognizable without colors") and in grayscale. If the block loses its corners or its window rhythm in either view, remove green from the corners and band lines first.
- **Decay states.** Make 2–3 overgrowth levels per building (light, medium, heavy) as separate overlay layers. This is Saint11's "repeat objects to show different levels of destruction", and it enables gameplay states.

### Gaps
- I found no tutorial devoted specifically to moss on iso top faces or vines on iso walls. The rules above are adapted from foliage, tree and ruins tutorials.
- Saint11's Vegetation Part 3 (Vegetation3.gif) was not read.

---

## 5. Small compound details at tiny sizes: chain-link, dishes, masts, helipad, flags, vehicles

### Takeaway
At 96–128px, most small details must be implied. Draw posts, rails and silhouettes on the 2:1 grid and suggest mesh, lattice and markings with sparse or low-contrast pixels. Use a few bright "signal" pixels (optics, warning lights, the flag color) to make each prop identifiable.

### Cited Findings
- **Iso circles and ellipses.** Make a grid from a skewed square with a cross through it, so the circle touches the square at the four cross endpoints. Or draw a regular circle, skew it with free transform, then clean it up. — [Slynyrd, Pixelblog 41](https://www.slynyrd.com/blog/2022/11/28/pixelblog-41-isometric-pixel-art). An arched opening is a circle the width of the window with a vertical skew of 26.5°. — [Tutsplus factory](https://design.tutsplus.com/tutorials/create-an-isometric-pixel-art-factory-in-adobe-photoshop--cms-25351)
- **Low-resolution readability:**
  - Keep only what matters: "taking away every bit of information that it's not extremely important."
  - Use solid shapes with strong contrast, and drop outlines when space is tight ("one of the first things to throw away").
  - Favor 90° corners. Use 45° diagonal pixels for sharp tips.
  - Avoid stray pixels around important areas.
  - One pixel can change posture or expression.
  - "Establish a symbol before reducing it", for example show the letter first so a grey square later reads as an envelope.
  — [Saint11, My Thoughts on Low Resolution](https://saint11.art/blog/thoughts_on_low_resolution)
- **Vehicles in iso.** Add fenders, lights, intakes, racing stripes, rear-view windows, a reflective sheen on windows, "rhythmic dots along angled edges" and extra shine on rims. Bevel edges at varying thicknesses. — [Slynyrd, Pixelblog 54](https://www.slynyrd.com/blog/2025/1/23/pixelblog-54-isometric-pixel-art)
- **Rooftop railing.** About "the character's waist height", with borders that touch walls lightened from black. — [Tutsplus office building](https://design.tutsplus.com/tutorials/create-an-isometric-pixel-art-office-building-in-adobe-photoshop--cms-23357)
- **Chain-link assets exist at small sizes,** for example a 12x12 chain-link tileset with 54 blocks ([slowdevelopment on itch.io](https://slowdevelopment.itch.io/chain-link-fence)) and an angled seamless chain-link tile ([Clip Studio Assets](https://assets.clip-studio.com/en-us/detail?id=1711691)). I found no written technique for them.

### Inferences
- **Chain-link fence (implied texture, not lattice).** At this scale a true diamond lattice (a 1px diagonal grid) aliases badly on 2:1 runs and fights the 2:1 lines. Build the fence as:
  - 1px vertical posts every 6–8px along the 2:1 ground line, 6–10px tall, in a dark metal tone with a 1px light top cap
  - a 1px top rail following the 2:1 slope
  - a mesh fill: either sparse alternating pixels (every other pixel on every other row) in a tone only slightly lighter than the ground behind, or simply no fill, letting rails and posts imply mesh
  - optional 3–4px "barbed" top: a 1px zigzag or alternating pixels above the rail
  - a 1px dark ground shadow line on the 2:1 slope
  Save the diamond pattern for close-up UI or a 2x zoom portrait, which is Saint11's "establish the symbol" idea.
- **Fence damage.** Draw sagging sections as a rail that dips 1–2px, a leaning post (drawn 1px off vertical across its height) and a gap with the mesh peeled back. Weave grass and vine clumps along the base to break the long line.
- **Satellite dish (8–14px).** Draw an iso ellipse (skewed circle) tilted up toward the sky:
  - a bright concave inner face (top-face tone or lighter)
  - a 1px darker rim on the shadow side
  - a 1px feed arm to a 1–2px feed horn
  - a 2–3px-wide pedestal or yoke on a small 2:1 base box
  The lit concave interior and dark back crescent are what make it read as a dish rather than a disc.
- **Radio masts (lattice towers).** Use a 1px or 2px vertical mast. Imply the lattice with alternating light and dark pixels up a 2px mast, or 1px cross-brace pixels every 3–4px on a 1px mast. Add 1–2 horizontal 1px arms for antennas, 1px guy wires (dark, low-contrast, optional) running down to the roof on 2:1-compatible slopes, and a 1px red aviation light on top with a 2-frame blink. Vertical lines stay perfectly straight (Saint11).
- **Helipad (roof top face).** Draw a circle as an iso ellipse (for example 16x8 or 20x10) in a pale paint tone one step below the brightest highlight. Inside, put an "H" built on the iso axes: the two uprights run along one 2:1 axis and the crossbar along the other. Make it no smaller than about 5x3px or it collapses. Weathered: break the ellipse with moss clumps and 1px gaps, and fade the paint one step toward the concrete tone.
- **Flagpole and flag.** Use a 1px pole 12–16px tall with a 1px finial highlight. Make the flag 4–6px by 3–4px, in 2–3 colors at most, with a 2–4 frame wave loop. A tattered state has a shorter, ragged flag with a missing corner.
- **Gatehouse.** A small 2:1 box (about one tile) with a darker window band, a 1px boom barrier in alternating paint stripes (hazard red and white, or yellow and black) and one emissive window pixel at night.
- **Vehicles (12–20px long).** Build a cuboid on the 2:1 grid with a darker windshield band, one sheen pixel on the glass, 1px wheel arches in the darkest tone and 1px headlight or taillight signal pixels. Military trucks and jeeps should be olive or desaturated green-grey. Keep them a different hue family from the concrete so they read at a glance.

### Gaps
- I found no sourced technique for chain-link fences, satellite dishes, antenna masts, helipad markings or flags specifically in 2:1 iso pixel art. Everything in this section's Inferences comes from general iso and low-res rules.

---

## 6. Military and industrial props at 24–48px: turrets, missile pods, pipes and refinery equipment

### Takeaway
Build props from primitives (cuboids, cylinders, spheres) on the 2:1 grid. Shade cylinders with a light stripe just inside one edge and a dark stripe at the other. Give each moving part (a turret, a pod) its own sprite layer. Use one or two bright optic or warning pixels plus a sparse accent stripe for identity.

### Cited Findings
- **Mech construction (about 32x32px).**
  - Build from simple primitives (cubes, spheres) and work outward from the body.
  - Shape the torso like "a tank's front", projecting past the shoulders.
  - Make the head low and sleek, with "one or two bright pixels for optics".
  - Add accent stripes last, only where areas look bare and the stripes won't add noise.
  - The artillery variant is "a giant cannon merged with the head, sharp blocky shapes".
  - A destructible gas tank on rooftops is suggested as an urban prop.
  — [Slynyrd, Pixelblog 61](https://www.slynyrd.com/blog/2026/4/1/pixelblog-61-isometric-mecha-tactics)
- **Industrial details:**
  - **Chimneys:** circles with a 1px outline, a cool grey for a concrete look, "one light stripe just inside the left edge and a dark stripe by the right edge", and "tiny serrated edges" to break straight stripes.
  - **Tanks:** cylinders with a wider top circle forming a conical cap and wedge-shaped shading. A horizontal tank is a cuboid plus end circles, raised above its shadow.
  - **Pipes:** a different color from the tank. "Enlarge the highlight at the elbow."
  - **Boxes:** add slits or vents to avoid plain surfaces.
  - **Metallic hue shift:** lighter tones move toward yellow, darker tones cooler.
  - **Smoke:** a near-white, slightly yellow plume at decreasing opacity (80/60/40/20%), mostly square with corners rounding as it rises.
  — [Tutsplus factory](https://design.tutsplus.com/tutorials/create-an-isometric-pixel-art-factory-in-adobe-photoshop--cms-25351)
- **Metal rendering order:** general shape, base light, base shadow, backlight and specular, reflection or interference. "Metal is very reflective so expect a strong back light." "Scratches and bruises tell a story about that object." "Metals can be more or less glossy depending on their use and conservation." "Metal is stiff; it doesn't stretch, but you can move its parts individually." Metal color ramp: light "usually colder, less saturated", dark "usually warmer, more saturated". — [Saint11, Metals](https://saint11.art/img/pixel-tutorials/Metal.gif)
- **Recent damage.** "If the damage is recent, objects will often have signals like fire or smoke." — [Saint11, Ruins](https://saint11.art/img/pixel-tutorials/Ruins.gif)
- **Separate turrets.** A free tank set draws the turret as its own sprite (base, turret and bullet files) so it can rotate. — [OpenGameArt, tank sprite](https://opengameart.org/content/tank-sprite). A WW2 iso tank pack ships both "turret merged" and "separated turret" variants at a 150x150 base size. — [Jim Hatama, German WW2 isometric pixel tanks](https://jimhatama.itch.io/german-ww2-pixel-tanks). PixVoxel's CC0 iso wargame sprites use 8 facing directions. — [OpenGameArt PixVoxel thread](https://lpc.opengameart.org/comment/32699)
- **3D as a base.** One workflow renders iso sprites from 3D with an orthographic camera and a pixelate node. — [J.S. Abbott, ArtStation tutorial](https://jsabbott.artstation.com/blog/YQaAw/tutorial-creating-2d-pixel-art-style-isometric-sprites-from-a-3d-model-in-blender). Saint11 also has an article on using 3D reference for pixel art. — [Saint11, Using 3D Reference](https://saint11.art/blog/3d-ref) (title only; not read)

### Inferences
- **Shared kit.** Make all military and industrial props from the same concrete ramp as the HQ, for plinths, revetments and bunkers, plus one metal ramp (olive or grey-green for military, rust-tinged grey for industrial). The shared base makes the props read as one compound.
- **Turret (24–32px).**
  - Base: a squat 2:1 cuboid or octagonal plinth, 4–6px tall, in concrete with a moss top.
  - Turret: a separate sprite in 8 directions if it rotates. A rounded box or half-sphere housing with 3-step shading.
  - Barrel(s): 1–2px wide. On 2:1 diagonals use clean 2:1 steps. On screen-horizontal or vertical facings use straight lines. Add a 1px highlight along the lit side and a 1px darker muzzle tip.
  - Signal pixels: one or two bright optic pixels (red or cyan) as the identity read.
- **Missile launcher (32–48px).** Draw a box-shaped pod (a cuboid tilted up 1:1 or 2:1) with a visible grid of missile tubes on the end face: 2x2 or 3x2 dark dots on a lighter face, each 1px with a 1px gap. Add 1px warning stripes (hazard yellow) on one edge only and a separate erector arm. A fired or empty state has tubes with no nose highlight.
- **Pipes and refinery.**
  - Pipes are 2–3px wide runs along the 2:1 axes, with vertical risers. Each has a 1px highlight stripe inside the lit edge and a 1px dark stripe on the shadow edge, plus an enlarged highlight at each elbow.
  - Flanges are 1px lighter rings every 6–10px.
  - Tanks and columns are cylinders with an iso-ellipse top in the top-face tone, the Tutsplus stripe shading and serrated edges.
  - A flare stack has a 2–4 frame flame of 2–4px in the emissive ramp, which doubles as a night light source.
  - Use rust streaks under joints and valves, the same streak rule as the concrete in Section 1.
- **Lamps.** A 1px pole, a 2–3px head and a 1px emissive pixel. At night, an optional engine-driven light pool. Overgrown state: a vine stem twisted up the pole and a broken lamp with a dark head and no emissive pixel.
- **Readability at 24px.** Saint11's low-res rules apply most here. Drop outlines, keep 90° and 2:1 edges crisp, put one high-contrast signal color per prop (optics, warning stripe or flame) and avoid scratches and scuffs at 24px. Reserve "scratches and bruises" for 40px and up.

### Gaps
- I found no pixel-art tutorial for iso missile launchers or refinery equipment at 24–48px. These rules extend the Tutsplus factory and Slynyrd mecha techniques.
- Saint11's Tech, FirearmDesign and Spaceships tutorials were not read and may hold relevant mechanical-detail rules ([index](https://saint11.art/blog/pixel-art-tutorials/)).

---

## 7. Palette: concrete greys with green overgrowth and warm night emissives, without muddy greys

### Takeaway
Hue-shift every ramp, including greys. A grey ramp that only changes brightness turns muddy. Keep concrete at low but non-zero saturation, separate faces by clear value steps (about 8–15% brightness), put the palette's saturation into the moss greens and the warm emissives, and use warm and cool contrast (warm lights against cool night concrete) to separate them.

### Cited Findings
- **Hue-shifted ramps:**
  - Use positive hue shift of about 20° per swatch ("20 is about as high as I go"). Positive shift warms colors as they brighten.
  - Avoid "straight ramps" that change only brightness and saturation, which "lack interest and [are] difficult to harmonize".
  - Peak saturation near the middle of the ramp. Darker colors generally carry more saturation, and saturation drops near the top.
  - Avoid combining high saturation with high brightness.
  - Make neutrals by duplicating the middle of the palette and desaturating it. Desaturated colors "work well as grays" next to vibrant ones.
  - The Mondo example uses 9 swatches per ramp.
  — [Slynyrd, Pixelblog 1: Color Palettes](https://www.slynyrd.com/blog/2018/1/10/pixelblog-1-color-palettes)
- **Why straight greys go muddy.** If you only make a color darker and lighter, "the shadows look grey and muddy" and highlights look washed out. — [Pixel Grimoire, How to start making pixel art #6](https://medium.com/pixel-grimoire/how-to-start-making-pixel-art-6-a74f562a4056) (search excerpt)
- **Ramps need hand-tuning.** Uniform saturation and brightness increments "doesn't necessarily produce satisfactory results". Neutrals can be made by duplicating mid swatches and desaturating them. — [gamedev.net, Color Palettes](https://gamedev.net/tutorials/visual-arts/color-palettes-r4964) (search excerpt)
- **Lighting and hue.** "Change the hue when changing the amount of light." "Use complementary light colors for a strong contrast." Colored lights from below suit "horror themes and tense settings". Volumetric light is visible only "if enough dust or smoke [is] present", and the cone is brighter in the middle. Background contrast: make the background dark on the bright side of the central object. — [Saint11, Illumination Techniques](https://saint11.art/img/pixel-tutorials/IlluminationTechniques.gif)
- **Foliage ramp.** Warm light, cold shadow, with almost half the gradient light green. "Keep the color count as low as possible." — [Saint11, Vegetation Part 1](https://saint11.art/img/pixel-tutorials/Vegetation.gif)
- **Metal ramp (reversed temperature).** Light "usually colder, less saturated", dark "usually warmer, more saturated". — [Saint11, Metals](https://saint11.art/img/pixel-tutorials/Metal.gif). This conflicts with the Tutsplus factory, where metal lights go toward yellow and darks go cooler ([Tutsplus factory](https://design.tutsplus.com/tutorials/create-an-isometric-pixel-art-factory-in-adobe-photoshop--cms-25351)). Both are presented as stylistic choices.
- **Warm grey alternative and face values.** The Tutsplus factory notes an alternative warm-grey chimney at about 40° hue and a face brightness spread of 8–15%. — [Tutsplus factory](https://design.tutsplus.com/tutorials/create-an-isometric-pixel-art-factory-in-adobe-photoshop--cms-25351)
- **Layered color counts.** Sky: "low saturation, low contrast". "2 colors for the sky", "3 colors for the far BG" and a "…ow color count for FG" (first letter obscured in the image, probably "low"). — [Saint11, City Backgrounds](https://saint11.art/img/pixel-tutorials/City.gif)
- **Small palettes.** Keep the palette small so each color has a clear reason to exist. Use complementary or high-contrast pairs to avoid "very boring images" from a single hue. — [Saint11, Low Resolution](https://saint11.art/blog/thoughts_on_low_resolution)
- **Night painting.** At night, streetlamps and windows glow more, edges blur and colors fade gently. — [Clip Studio Tips, Night Illustration](https://tips.clip-studio.com/en-us/articles/11478) (search excerpt)
- **Lighting pipelines in shipped games.** Eastward's team rebuilt asset parts in 3D and hand-painted bump maps "to ensure they have the correct lighting performance in-game", with shaders conveying weather and time of day. — [Game Developer, Eastward interview](https://www.gamedeveloper.com/art/eastward-s-creators-share-insights-on-making-pixel-art-adventures)

### Inferences
- **Concrete ramp (5–6 colors).** Keep saturation low but never 0%: about 5–15% in mids and highlights and 15–25% in the darkest shadow. Choose one of two strategies and use it everywhere:
  - **(A) Cool-to-warm through violet:** shadow is blue-violet, mids are violet-grey or neutral, highlights are warm buff. This follows Slynyrd's positive shift and suits buff precast concrete like the FBI HQ.
  - **(B) Narrow cool ramp:** blue-grey throughout, with only the top highlight warmed.
  At saturation below about 10%, hue jumps larger than 20° per step are barely visible, so (A) can cross more of the wheel than Slynyrd's 20° cap implies. Check it in-engine.
- **Face separation.** Map top, lit side and shadow side to three distinct ramp steps, about 8–15% brightness apart, not adjacent near-duplicates. Muddiness usually comes from too many greys within 5% brightness of each other.
- **Moss and foliage ramp (4–5 colors).** Darkest is a deep teal or blue-green, which can be shared with the darkest concrete shadow to unify the two. Then a cool mid-green, a yellow-green base and a pale warm-green highlight. Moss on concrete uses the lower-saturation middle two. Living trees and shrubs get the more saturated top two, following Saint11's "old ruins… low contrast".
- **Don't mix green into grey with dither.** Mixing green and grey at the pixel level produces olive mud. Keep moss as clean clusters on top of clean concrete.
- **Glass (2–3 colors).** Dark bronze/brown-grey or dark blue-grey. The darkest glass can equal the darkest concrete shadow.
- **Emissive ramp (3 colors).** A pale yellow hot core, a warm amber main and a dim orange-brown falloff. The falloff color doubles as the rust streak. Add one red for aviation and warning lights and one optional cyan for monitors or optics.
- **Night strategy.** Prefer engine-side lighting (a night tint, emission or light layers, light pools) over separate night-painted sprites, as Eastward and Sea of Stars build lighting into their pipelines. If night sprites are hand-painted, shift every concrete step one notch cooler and darker and leave the emissive ramp unchanged. The complementary warm-versus-cool contrast (Saint11) then makes lit windows pop.
- **Budget.** About 24–32 colors for the whole compound and prop set:
  - concrete: 6
  - glass: 2–3
  - moss and foliage: 5
  - metal (military olive and industrial): 3–4
  - emissive: 3
  - red and cyan signal: 2
  - paint (off-white and hazard yellow): 2
  - ground or dirt: 2–3
  - near-black: 1
  Lospec has hue-shifted palettes to start from ([Lospec, "hue shift" tag](https://lospec.com/palette-list/tag/hue%20shift)). Build your own so the look stays original.

### Gaps
- No source gave tested HSB values for a concrete-plus-moss-plus-emissive palette. The numbers above are starting points to validate in-engine.
- Saint11's Darkness and Shading tutorials were not read and may add night-palette guidance.

---

## 8. Examples to study: what makes modern iso and ruined pixel cities read well (without copying any one look)

### Takeaway
The work that reads well shares a few traits: clean geometric blockouts with texture added last, strict value hierarchy between planes, low-contrast backgrounds and high-contrast foregrounds, readable silhouettes, and lighting systems designed in from the start. Study these principles across several sources rather than any single game's palette or shapes.

### Cited Findings
- **Saint11, City Backgrounds.** Process: "block out your idea", then "choose light source and get the shapes right", then "block out structural details and objects", then "add object details and textures", and only then "add vegetation and any environmental stuff like mud, rain, snow". The sky is low saturation and low contrast. Background layers use very few colors. — [Saint11, City Backgrounds](https://saint11.art/img/pixel-tutorials/City.gif)
- **Saint11, Ruins.** Every ruin should "tell a story". Ask "why is this object ruined?" The silhouette itself must look ruined. — [Saint11, Ruins](https://saint11.art/img/pixel-tutorials/Ruins.gif)
- **Into the Breach (Subset Games).** Reviewers describe "attractive but elementary pixel graphics [that] clearly communicate the layout of any given battle". — [Game Informer review](https://gameinformer.com/games/into_the_breach/b/pc/archive/2018/02/26/game-informer-review-into-the-breach). One analysis argues the team deliberately kept a simple style so art could "match mechanics… in a clean and cohesive manner". — [GameTextures on Medium](https://medium.com/gametextures/subset-games-create-near-perfection-94688f04847) (search excerpt)
- **Eastward (Pixpil).**
  - The setting began with sketches "reminiscent of the Kowloon Walled City".
  - Assets are divided "into different parts according to their natural structure", rebuilt in 3D and hand-painted on bump maps for lighting.
  - A fixed camera "forces you to adjust the world so the player is never unintentionally obscured behind the environment", and verticality and elevation make areas feel dense.
  - Worlds should feel larger than the player, with a sense of history.
  — [Game Developer, Eastward's creators share insights](https://www.gamedeveloper.com/art/eastward-s-creators-share-insights-on-making-pixel-art-adventures)
- **Sea of Stars (Sabotage Studio).** Dynamic lighting was developed from the start (2018), and the asset pipeline was shaped around it. Creative director Thierry Boulanger said it is "doing things that 2D sprite games shouldn't do with lighting". — [Nintendo Life hands-on (2020)](https://www.nintendolife.com/news/2020/03/hands_on_first_impressions_of_sea_of_stars_the_next_project_from_the_messenger_dev_sabotage) (search excerpt); [PC Gamer](https://www.pcgamer.com/sea-of-stars-is-a-gorgeous-retro-rpg-from-the-makers-of-the-messenger) (search excerpt). Note that Sea of Stars is not isometric. Its relevance is the lighting approach, not the projection.
- **eBoy.** Founded in 1997 by Kai Vermehr, Steffen Sauerteig and Svend Smital. Known for dense isometric "Pixoramas" of cities such as Tokyo, Paris, Rio, Berlin and London. — [Wikiwand, eBoy](https://mobileapps.wikiwand.com/en/EBoy). Make's "Complete Guide to Isometric Pixel Art" uses eBoy's work as its examples. — [Make](https://makezine.com/article/craft/the-complete-guide-to-iso)
- **Other post-apocalyptic iso work.** *Beautiful Desolation* is a post-apocalyptic isometric adventure but is rendered and painted, not pixel art. — [The Brotherhood](https://thebrotherhoodgames.com/desolation/). *Beneath a Dead City* is tagged isometric pixel-art city builder. — [itch.io](https://joshuagalecki.itch.io/beneath-a-dead-city)
- **Pixel Joint study material.** The tutorial index includes Pixelblink's "Making a Cube" and "Building a Building", Zi's isometric roof tutorial, isometric circles and rock and stone textures. — [Pixel Joint tutorials](https://pixeljoint.com/pixels/tutorials.asp?id=2254)

### Inferences
- **What to take from each, as principles rather than looks:**
  - **Into the Breach:** at strategy-map scale, the layout and state of each tile (intact, damaged, burning) must read before any texture does.
  - **Eastward:** layered, part-separated assets let lighting and occlusion work, and density comes from verticality and stacking, not from noise.
  - **Sea of Stars:** decide the lighting model (day/night, emissives, light pools) before finalizing sprite palettes.
  - **eBoy-style cities:** many small, simple, repeated modules plus a few high-contrast "story" props per block (a vehicle, a sign, a person) keep large dense scenes legible.
  - **Saint11's ruins and city guidance:** overgrowth and grime are the last layer, applied with a narrative reason for where they occur.
- **Study method.** When studying a piece, check:
  - (1) a 1-color silhouette (is the building identifiable?)
  - (2) grayscale (are the top, left and right planes three clearly different values?)
  - (3) a count of the colors used per material
  - (4) where the highest contrast is, which should be at silhouette borders and focal props
  - (5) how much of the surface is plain base color
  Compare several artists' answers to each check to find your own rules rather than adopting one game's palette or shapes.

### Gaps
- I did not retrieve specific Pixel Joint gallery pieces, r/PixelArt threads, ArtStation breakdowns or X/Twitter breakdowns of overgrown iso cities. Search mostly returned asset stores and AI-generated "pixel art" listings (for example pixie.haus pages and an AI-generated itch.io sprite pack), which I excluded.
- I found no eBoy interview describing their pixel-level technique. A 2014 Verge feature ("Pixel perfect: the story of eBoy") was mentioned in search results but not retrieved.
- I found no Sea of Stars art-team breakdown of environment pixel technique. Available sources cover only the lighting system at a high level.
- I found no artist commentary on how Into the Breach's buildings are drawn. The game's art credits were not confirmed.
