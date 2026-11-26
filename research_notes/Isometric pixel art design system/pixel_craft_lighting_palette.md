# Pixel-Art Craft Rules for Isometric Sprites: Lines, Lighting, Shading, Palettes, Outlines, Dithering

Scope: craft rules for a cohesive set of 2:1 isometric sprites (base, turrets, missile launchers, refineries, lamps, resource pools, enemy drones) in a small, dark-themed, lamp-lit browser tower-defense game. Sources are practitioner tutorials: Slynyrd/Pixelblog, Derek Yu, Cure (Pixel Joint), Pixel Parmesan, Wikibooks, and forums. Bullets under "Inferences" are my own synthesis or general practitioner knowledge that I did not verify in this session. They are labelled so the report writer can weigh them.

## 1. Iso line rules (2:1 steps, clean vs broken lines, 1:1/4:1, verticals, jaggies/doubles, circles and ellipses)

### Takeaway
Every ground-plane edge uses the 2:1 "two-step" line: 2 px across, 1 px up, about 26.565°, not the true 30°. Vertical edges are straight 1 px columns. A line is clean only when every run in it has the same length. Iso circles are ellipses drawn inside a 2:1 iso square, built from their vertices outward. Count pixels and keep a ruler layer rather than trusting your eye.

### Cited Findings
- True isometric is 30°, but "a true 30º degree line in pixel art results in a jaggy rhythm." Pixel art uses 2:1 "2-step lines" (about 26.5°), and the whole composition should align to a grid of 2-step lines. [Slynyrd Pixelblog 41](https://www.slynyrd.com/blog/2022/11/28/pixelblog-41-isometric-pixel-art)
- "The 2:1 (2x,1y) line is the foundation of isometric pixel art." You may adjust angles for patterns that are "less jagged and easier to work with (like 1:2 pixel line.)" [Pixel Parmesan: Fundamentals of Isometric Pixel Art](https://pixelparmesan.com/fundamentals-of-isometric-pixel-art/)
- "It's possible to use a different projection angle, but two-step lines create the most realistic perspective." Isometric "rotates the building 45 degrees" compared with 3/4 top-down. Use a layer with a long 2:1 line in a bright colour as a guide. [Slynyrd Pixelblog 4](https://slynyrd.com/blog/2018/4/12/pixelblog-4-graphical-projection-part-2)
- The "Golden Rule": "for every 2 pixels in the Z axis (front to back) there is one pixel for the X or Y axis." Line lengths should be multiples ("2 x 2 = 4; 2 x 3 = 6"). A cube's lines "must be of equal length for it to be a real cube." [Wikibooks: Isometric Pixel Art](https://en.wikibooks.org/wiki/Isometric_Pixel_Art/Printable_version)
- Cube corner rule: the front/top corner "should be three pixels wide, as opposed to two like the rest of the box". That is a 3 px run where two 2:1 lines meet, so the corner does not form a sharp single-pixel spike or a doubled pixel. [Wikibooks](https://en.wikibooks.org/wiki/Isometric_Pixel_Art/Printable_version)
- Other ratios are acceptable for specific forms. Wikibooks draws a car "by 3s" (3:1) "as it fits the slope of the car better." [Wikibooks](https://en.wikibooks.org/wiki/Isometric_Pixel_Art/Printable_version)
- Cube-style trade-off: wide-corner cubes (A1) look better but "need overlapping to tile well". The sharp-corner cube D1 "adheres more to the 2:1 grid and tiles perfectly." [Slynyrd Pixelblog 41](https://www.slynyrd.com/blog/2022/11/28/pixelblog-41-isometric-pixel-art)
- Tiles: use cuboids. "So long as the top surface is uniform the height of the tiles can vary." Texture must loop on all four edges of each visible face. The example tile is 36x36 px, and "Whatever size you go with, even dimensions are your friend." [Slynyrd Pixelblog 41](https://www.slynyrd.com/blog/2022/11/28/pixelblog-41-isometric-pixel-art)
- Workflow: draw a wireframe iso cuboid around each sprite, keep one 2:1 line on its own layer as a ruler, and "count pixels" instead of relying on your eyes. [Slynyrd Pixelblog 41](https://www.slynyrd.com/blog/2022/11/28/pixelblog-41-isometric-pixel-art). In Aseprite, Shift with the pencil gives a live-preview 2:1 line. Map the full outline, including hidden lines, before rendering. [Slynyrd Pixelblog 54](https://www.slynyrd.com/blog/2025/1/23/pixelblog-54-isometric-pixel-art)
- The width, depth and height of a cube "are all equal measurements visually." [Pixel Parmesan](https://pixelparmesan.com/fundamentals-of-isometric-pixel-art/)
- Jaggies: "Jaggies occur when a pixel or group of pixels are out of place, interrupting the flow of a line." The fix is "a more uniform length of pixels." Unless a line is horizontal, vertical or 45°, "the edges of your line segments are naturally going to be a little jagged." [Cure's tutorial, Pixel Joint](https://pixeljoint.com/forum/forum_posts.asp?TID=11299)
- Curves: "jaggies crop up when the length of the line segments don't grow or shrink in a consistent manner." Reduce outlines to 1 px thickness. [Derek Yu, Pixel Art Tutorial](https://www.derekyu.com/makegames/pixelart.html)
- Iso circles, method 1: build a skewed (iso) square with a cross through it. The circle touches the square at the four endpoints of the cross. Alternatively, draw a normal circle and skew it (Photoshop Free Transform), then clean up by hand. [Slynyrd Pixelblog 41](https://www.slynyrd.com/blog/2022/11/28/pixelblog-41-isometric-pixel-art)
- Iso circles, method 2: "the majority of information about an ellipse is contained on the ends, also known as the vertices." Contain each ellipse in an iso square box and make sure it hits the correct grid points. [Pixel Parmesan](https://pixelparmesan.com/fundamentals-of-isometric-pixel-art/)
- Cylinders: a tapered cylinder is "One flat oval at the base, and another slightly smaller one placed above," joined at the sides along the vertical axis. It should conform to the 2:1 grid. [Slynyrd Pixelblog 54](https://www.slynyrd.com/blog/2025/1/23/pixelblog-54-isometric-pixel-art)
- Side-wall ellipses (community advice): draw an ellipse 1.61 times as wide as it is tall, rotate it about 63.4°, then clean up. Pixel iso shows less of the top face, so top ellipses end up thinner than side ellipses. [Pixel Joint forum TID=6033](https://pixeljoint.com/forum/forum_posts.asp?TID=6033) (forum opinion, not a standard)
- A roof tutorial in the search results recommends only two jag-free angle combinations: a 1:1 diagonal with a straight horizontal, and 1:2 with 1:1. I could not confirm which page this came from. It most likely matches [Clip Studio Tips: Isometric View – Pixel Art Style](https://tips.clip-studio.com/fr-fr/articles/4969?org=1), so treat it as low confidence.

### Inferences
- Clean vs broken lines (synthesis of the Cure and Derek Yu run-length rules). The clean slopes are those whose runs repeat identically: 1:1 (1-1-1), 2:1 (2-2-2), 3:1 (3-3-3), 4:1 (4-4-4), plus pure horizontal and vertical. Mixed runs such as 2-1-2-1 (about 3:2) or 3-2-3-2 read as broken or jaggy. Use those only inside a curve, where runs grow or shrink monotonically.
- When to use which slope in this game:
  - 2:1 for every edge that lies on the ground plane: tile edges, building footprints, roof edges, the base of walls, pool rims.
  - Vertical 1 px columns for every wall corner and tower side. Never lean them.
  - 1:1 for diagonal roof facets, braces, missile fins and launcher rails, where 45° is the physical angle. 1:1 lines need no AA.
  - 4:1 for very shallow shapes: the low arc of a dish rim, the near-flat ends of a large ellipse, ramp lips.
  - Keep the number of non-2:1 slopes on screen small so the grid stays legible.
- Doubles: in a 1 px line, a doubled pixel is an extra pixel that turns a diagonal step into an "L". These should be removed, which is what Aseprite's "pixel-perfect" stroke mode does. The 3 px corner rule above is the sanctioned exception, where two 2:1 lines meet at a vertex.
- Tile convention, from general practice and not verified here: diamond tiles are usually 2:1 sized (32x16, 64x32). Many artists use a 2 px wide flat top and bottom vertex, and some use 32x17 or 64x31 variants to stop adjacent tiles doubling their shared edge. Pick one convention and test-tile it before drawing any sprite. Slynyrd's "even dimensions" rule supports using 32 or 64 widths.
- Geometry, derived from the projection: 2:1 iso is "rotate 45°, then halve the height". A circle lying flat on the ground therefore projects to an ellipse whose width:height is exactly 2:1. A pool or tower footprint that fills a W-wide tile is a W x W/2 ellipse, e.g. 32x16 or 24x12. A round tower is that 2:1 top ellipse, two straight vertical sides at its left and right vertices, and the front half of an identical bottom ellipse. A dish tilted toward the viewer becomes a fatter ellipse, between 2:1 and 1:1.
- Pixel-perfect ellipse recipe (synthesis): start at the four vertices. Use long runs at the left/right ends, e.g. 4-3-2-1 for a 2:1 ellipse, shrinking monotonically toward the top and bottom, where you finish with long horizontal runs. Mirror one quadrant to get the other three so the shape stays symmetrical.

### Gaps
- No source explicitly codified when to use 1:1 or 4:1 in iso. That guidance above is inference.
- The Northlake "Complete Guide to Isometric Pixel Art" chapters on lines would likely cover broken lines and corner joins, but they could not be fetched because of an expired TLS certificate.
- No authoritative source on tile-size conventions (32x16 vs 32x17) was retrieved.

## 2. Lighting: standard light direction, top/left/right face values, rim light vs ambient occlusion

### Takeaway
Pick one light, from top-left or top-right, and never change it across the set. The top face is brightest, the side turned toward the light is the mid tone, and the side turned away is darkest, so each of the three faces gets a distinct value. No source gives numeric percentages. The steps proposed below are inference. Interior or lit edges are drawn lighter than any face, and contact with the ground is controlled by lightening or darkening the outline.

### Cited Findings
- "A light source which hits the top face mainly as well as one side face partially is most effective," so each face has a different value. A cast shadow "running horizontally to the right or left" fits the iso grid. "A light higher in the sky creates a shorter shadow." Construct shadows by projecting lines from the bottom edges at the same angle and length along the ground, then connecting the endpoints. [Pixel Parmesan](https://pixelparmesan.com/fundamentals-of-isometric-pixel-art/)
- Use at least three colours, "each getting gradually lighter", with a fourth optional. The face turned away from the light gets the darkest colour, the next face the next darkest, and the third face the lightest. If you move the light, swap the colours and mirror the box. Shadows are "usually less saturated than the other colours." "If you leave off light sources, the whole picture will be off." [Wikibooks](https://en.wikibooks.org/wiki/Isometric_Pixel_Art/Printable_version)
- Top-left light example: the top face is the brightest (the base colour), the left face is a dimmer mid tone, and the right face is darkest because it "gets the least amount of light". Interior edges are filled with a tone "brighter than all the others", while the exterior outline stays black. Shading "doesn't have to be completely accurate... as long as you're generally consistent." [Kethsong: Isometric Pixel Art Tutorial II](https://kethsong.blogspot.com/2017/07/isometric-pixel-art-tutorial-ii-shading.html)
- Top-right light example: strong directional light should create "considerable contrast" between the right-facing and left-facing sides. Values follow "the light source, and luminous quality of the building materials." Warm light glows on brick, roofs get a touch of shine, rims get shine, and windows get a reflective sheen. Bevels "slice the edges with varying thickness." [Slynyrd Pixelblog 54](https://www.slynyrd.com/blog/2025/1/23/pixelblog-54-isometric-pixel-art)
- The light source is most often top-right or top-left because it is usually treated as the sun. [Wikibooks](https://en.wikibooks.org/wiki/Isometric_Pixel_Art/Printable_version) (via search summary)
- General sprite convention: "there is a single light source above the orc and slightly in front of it." Surfaces facing the light are bright, and shadows go on the bottom and back. [Derek Yu](https://www.derekyu.com/makegames/pixelart.html)
- Ground contact: lighter or removed outlines where an object meets the ground make it look flush with the surface. Darker outlines make it look "more solid or interactable." Outlines can themselves be shaded by the light source. [Pixel Parmesan](https://pixelparmesan.com/fundamentals-of-isometric-pixel-art/)
- Pixel artists favour sharp light-to-shadow transitions to avoid banding. [Pedro Medeiros (Saint11), How to start making pixel art #4](https://medium.com/pixel-grimoire/how-to-start-making-pixel-art-4-f57f51dcfa02) (via search snippet; the page returned 403)
- Rim light, forum advice: for objects that cast heavy shadows, an opposing light source that doesn't cast its own shadow helps brighten dark edges and keep them readable. [itch.io post](https://itch.io/post/9656067) (forum opinion)
- Standard shading vocabulary includes highlight, light centre, halftone, midtone, reflected light, shadow, cast shadow and ambient occlusion. [Skillshare: Pixel Art Mastery Part 3](https://www.skillshare.com/en/classes/Pixel-Art-Master-Course-Beginner-to-ExpertFreelance-Textures-and-Shading-Part-3/622494873)

### Inferences
- Proposed global light for this game: top-left. The top face is brightest, the left face is the mid tone and the right face is darkest. It matches the Kethsong and Derek Yu convention, and it leaves the right-hand faces, which point toward the screen, dark enough for lamp spill to show up on them. Top-right is equally valid. What matters is that every sprite uses the same direction.
- Face value steps (my suggestion; no source gave numbers). On a 5- to 7-swatch ramp, use the same three indices for every material:
  - Top face: index n.
  - Lit side: n−1.
  - Shadow side: n−2, or n−3 for strong light.
  - In HSV brightness terms, roughly top 100%, lit side about 75–85%, shadow side about 50–60% of the top value.
  - Highlighted edges or bevels: n+1.
  - AO or contact line: n−3, or the darkest ramp colour.
  - Keep at least one full ramp step between faces so they still read when the art is shrunk to 1x.
- Ambient occlusion vs rim light in a dark, lamp-lit scene:
  - AO: a 1 px darkest-colour line where a wall meets the ground, and inside concave corners such as the turret-to-base seam. This anchors sprites to tiles, which is Pixel Parmesan's "darker outline means solid" rule.
  - Rim light: a 1 px edge in a lamp-tinted colour on the silhouette side facing the nearest light. If lamps are dynamic, this belongs in the engine (a tinted overlay or second sprite layer) rather than baked into the art. Use it on enemies and drones so they separate from the dark floor.
- Mixed lighting rule (inference): bake only the global "moon or ambient" key light into sprites, using the top-left convention. Treat lamp light as additive, either engine-side or as separate glow sprites. That way a turret looks correct whether or not it sits near a lamp.

### Gaps
- No source found gives numeric percentages or value steps for top, left and right faces.
- No pixel-specific source on ambient occlusion technique was retrieved beyond vocabulary lists.

## 3. Colour ramps, hue shifting, shades per material, palette size, known palettes, dark-mode readability

### Takeaway
Build ramps whose brightness rises steadily, whose saturation peaks in the middle, and whose hue shifts by at most about 20° per step. Ramps are usually 4–9 swatches, and practitioners commonly use 16- or 32-colour palettes. DB16, DB32, PICO-8 and Endesga 32 are established starting points. For a dark game, keep the environment low in value and low in saturation, and reserve the top values and saturations for emissives and threats.

### Cited Findings
- Slynyrd uses a positive hue shift of 20° between swatches in a ramp ("20 is about as high as I go"). Successive ramps are 45° apart around the 360° wheel, giving 8 ramps. A positive shift gives natural colours "warming as they become brighter." [Slynyrd Pixelblog 1: Color Palettes](https://www.slynyrd.com/blog/2018/1/10/pixelblog-1-color-palettes)
- Brightness increases steadily across the ramp, usually not starting at 0 unless you want black. Saturation peaks around the middle and never quite reaches 100% or 0%. Saturation takes bigger steps at the ends and smaller steps in the middle, and brightness takes smaller steps near 100%. As brightness rises, reduce saturation, since dark colours are generally more saturated. Exact values come down to "trusting the eyeballs." [Slynyrd Pixelblog 1](https://www.slynyrd.com/blog/2018/1/10/pixelblog-1-color-palettes)
- Slynyrd's Mondo palette is 9 swatches per ramp × 8 ramps plus neutrals, 128 colours in total. Thyrian Defenders used 160 colours. Neutrals are made by duplicating the middle of the ramps, dropping the darkest and lightest columns, flipping and desaturating. You can also "Start with trying to make a small ramp." [Slynyrd Pixelblog 1](https://www.slynyrd.com/blog/2018/1/10/pixelblog-1-color-palettes)
- Hue shifting: bend highlights "toward a certain color" and darks "toward a second color". Straight ramps change only luminosity. A ramp is "a group of colors that can be used together, arranged according to luminosity," and ramps often share their darkest or lightest colour. Use "a full range of values." "The smaller the palette, the easier it is to manage," "the palette ties the piece together," and "a high amount of colors can indicate the use of dirty tools." [Cure's tutorial](https://pixeljoint.com/forum/forum_posts.asp?TID=11299)
- Common palette sizes are 32 and 16 colours. Constrain colours, but don't let palette choice delay starting, because palettes are easy to swap. [Derek Yu](https://www.derekyu.com/makegames/pixelart.html)
- "When colors are too similar, pixels begin to blend together and get lost." Give each colour a distinct role. Avoid "naive" pure local colours, because reflected light mixes colours. [Derek Yu, Common Mistakes](https://www.derekyu.com/makegames/pixelart2.html)
- Conflict on shadow saturation: Wikibooks says shadows are "usually less saturated" ([Wikibooks](https://en.wikibooks.org/wiki/Isometric_Pixel_Art/Printable_version)), while Slynyrd says dark colours generally carry more saturation ([Slynyrd Pixelblog 1](https://www.slynyrd.com/blog/2018/1/10/pixelblog-1-color-palettes)). Slynyrd's view, with saturation peaking in the mid-darks and falling off at the very darkest and lightest ends, is the more widely taught modern approach. That last judgement is mine.
- DawnBringer 32 (DB32): 32 colours, starting #000000 and #222034. It was first released as "DawnBringer's 32 Col Palette V1.0" on the Pixel Joint forums. [Lospec: DawnBringer 32](https://lospec.com/palette-list/dawnbringer-32); [Lospec dawnbringer tag](https://lospec.com/palette-list/tag/dawnbringer)
- Endesga 32: created by ENDESGA, originally for NYKRA, "honed over years of palette creation, refined for materialistic pixelart and design". A successor palette is billed as higher contrast, range and saturation. [Lospec endesga tag](https://lospec.com/palette-list/tag/endesga)
- PICO-8 is Lexaloffle's fantasy console with a fixed 16-colour palette. PICO-DB follows PICO-8's structure "in the style of Dawnbringer 16." [Lospec: PICO-DB](https://lospec.com/palette-list/pico-db)
- Check shading by flipping the art horizontally and by desaturating it. Use a squint test: the art should still show "a few large clusters of light and dark." [Derek Yu](https://www.derekyu.com/makegames/pixelart.html)

### Inferences
- Shades per material (synthesis), with ramps drawn from a shared 32-colour master palette:
  - Hard-surface metal or concrete: 4–5 shades. That covers 3 face values, 1 edge highlight and 1 AO.
  - Organic or moss: 3–4 shades.
  - Emissive: 3 shades plus near-white. That covers a dark coloured halo, a saturated mid, a light core and a white-hot pixel.
- Hue-shift direction for this game: darks lean toward blue or violet (cool), highlights toward yellow or orange (warm), with a step of 10–20° per swatch, following Slynyrd. Lamps throw warm light, so the warm highlight bias reads as "lit by lamps". Lean shadows toward the background's blue-black so sprites sit in the scene.
- Palette size: a 32-colour master, e.g. based on DB32 or Endesga 32, is a practical ceiling for a small team. 16 colours, as in DB16 or PICO-8, is tight but very cohesive. 64-colour palettes such as Endesga 64 or Resurrect 64 exist for more material variety. DB16 and the 64-colour palettes are general knowledge and were not verified this session.
- Dark-mode readability, as a value budget:
  - Floor and terrain: roughly the bottom 0–35% of value, low saturation.
  - Structures: 20–70% of value.
  - Player emissives (lamps, refinery pools, turret muzzles): 70–100% of value with high saturation.
  - Enemy accents (drone eyes, thrusters): high value in a hue family no friendly object uses, e.g. friendly cyan or amber vs enemy magenta or red.
  - Test by desaturating the whole screen, Derek Yu's check. Threats and interactables should still pop on value alone.
- Black is not an outline colour on a dark background. Use the palette's darkest blue-black (DB32's #222034 is an example) for outlines and deepest shadow, and keep true #000000, if present, for empty void.

### Gaps
- No source quantified "shadows cooler" in degrees. Slynyrd only gives the +20° per step maximum.
- I did not fetch the individual Lospec pages for DB16, Endesga 64 or PICO-8, so their exact hex counts and creation dates are unverified.
- No source specifically on keeping dark-themed pixel games readable was found.

## 4. Outlines: full black vs selective (sel-out) vs none; interaction with iso tiles; interior vs exterior lines

### Takeaway
Pure black outlines are cartoony and harsh. Sel-out, a coloured outline lightened where it meets the light or the background, is the polished standard, but it is only safe when the background is known, and here it is: consistently dark. Iso terrain tiles should have no outlines, because outlined blocks create tangents when tiled. Interior lines should be shadow colours, not black, and lit interior edges should be lighter than any face.

### Cited Findings
- A pure black outline "can look cartoony" and gives harsh segmentation. Sel-out replaces much of it with lighter colours, and where the sprite meets negative space the outline may be removed. Interior lines use "darker shadow colors instead of pure black." At small sizes, "color plays a larger role in defining the character than outlines." Keep outlines 1 px. [Derek Yu](https://www.derekyu.com/makegames/pixelart.html)
- "Sel-out... is anti-aliasing an outline to a background color." It "works if it is created for specific scenarios, such as in a game where you know the background will be consistently dark." A full outline with light variation causes fewer jaggies than a broken one. Avoid "the (easy) mistake of doing external AA." [Cure's tutorial](https://pixeljoint.com/forum/forum_posts.asp?TID=11299)
- "Iso blocks without outlines will tile the most cleanly." Outlined blocks "will almost always create some sort of pixel tangent in certain situations." Visible inner lines "often ruin the illusion of dimensionality." Lighter or removed outlines at the ground make an object look flush, and darker ones make it look solid or interactable. Outlines can be shaded by the light source. [Pixel Parmesan](https://pixelparmesan.com/fundamentals-of-isometric-pixel-art/)
- Classic iso-box style: the exterior outline stays black, and interior edge lines are recoloured lighter than any face. In the Wikibooks version, the optional fourth colour fills black lines "that ARE NOT touching the edge." [Kethsong](https://kethsong.blogspot.com/2017/07/isometric-pixel-art-tutorial-ii-shading.html); [Wikibooks](https://en.wikibooks.org/wiki/Isometric_Pixel_Art/Printable_version)
- In Slynyrd's finished iso pieces, "subtle outlining is applied to make the sprite pop." [Slynyrd Pixelblog 54](https://www.slynyrd.com/blog/2025/1/23/pixelblog-54-isometric-pixel-art)

### Inferences
- Proposed outline policy for the game:
  - Terrain and floor tiles and resource-pool basins set into the floor: no outline.
  - Standalone objects (base, turrets, launchers, refineries, lamps): 1 px exterior outline in the darkest palette colour, sel-out.
    - Top and lit edges: the outline lightens to the face's shadow colour, or is replaced by a 1 px highlight edge.
    - Bottom and contact edge: kept darkest, which gives AO and reads as solid.
  - Interior edges between faces: no dark line. The value change does the work, and the top-face/side-face seam may carry a highlight pixel row.
  - Enemy drones: a fully closed outline in an enemy-tinted dark colour, so they read against any tile while moving.
- Because the background is dark, an exterior outline in near-black merges with the floor. Separation comes from the sprite's lit faces and any rim light, not from the outline. That makes the top-face value and the emissive accents more important than outline weight.

### Gaps
- No source discussed how outlines on tall iso sprites should overlap the tiles behind them (depth-sorted overlap).

## 5. Dithering: when it helps texture, when it hurts; checkerboard vs ordered vs noise

### Takeaway
Use dithering sparingly. It earns its place on large, static, low-contrast areas: concrete slabs, moss patches, sky or fog gradients, and big pool surfaces. It hurts on small or animated sprites and on hard edges, and noise dither usually looks sloppy. Checkerboard (50%) is the default pattern. If dithering covers half a sprite, add a colour instead.

### Cited Findings
- The checkerboard is "most common for simple pixel blending". Noise dithering is "not a 'pattern'" and can look "a bit sloppy." [Pixel Parmesan: Dithering for Pixel Artists](https://pixelparmesan.com/dithering-for-pixel-artists)
- There are two uses of dithering. Fill dithering makes an extra apparent colour across a form and is "most useful for 1-bit or otherwise low color count pieces". Transitional dithering smooths a blend or edge and is "most useful for relatively higher res pixel art with a more painterly aesthetic." [Pixel Parmesan](https://pixelparmesan.com/dithering-for-pixel-artists)
- Some sprites are "simply too small to dither relative to the amount of detail they can possibly contain." "I would not recommend dithering for most character sprites, especially those that will need to be animated." "Extraneous dithering often creates visual noise or the unwanted illusion of texture." Dithering "softens edge definition and may disrupt the integrity of forms." The pattern becomes more obvious as the image is scaled up. [Pixel Parmesan](https://pixelparmesan.com/dithering-for-pixel-artists)
- "The greater the contrast between the two colors you are blending, the more dithering steps you will need." Instead of many patterns between two colours, add intermediate colours and reuse a few patterns. On older hardware dithering "was often a necessity", and CRT blur made patterns "less visible." [Pixel Parmesan](https://pixelparmesan.com/dithering-for-pixel-artists)
- "The most common form of dithering you'll see is a 50/50 dither, also known as a 50% dither or a checkerboard pattern." "If dithering is covering half your sprite, it'd probably just be better if you added a new color to the palette." "The lower the contrast is between the two... the less harsh the dithering will be." Random dithering "isn't generally advised." [Cure's tutorial](https://pixeljoint.com/forum/forum_posts.asp?TID=11299)
- Dithering bridges two shades without adding a new one. It "works best on large areas of a single color or on rough, bumpy surfaces," and some artists avoid it entirely. [Derek Yu](https://www.derekyu.com/makegames/pixelart.html)
- In finished iso work, "minor dithering smooths harsh cluster connections." [Slynyrd Pixelblog 54](https://www.slynyrd.com/blog/2025/1/23/pixelblog-54-isometric-pixel-art)
- Low-confidence claims from search snippets, with attribution among these guides uncertain:
  - Checkerboard is the coarsest ordered dither and gives a very visible grid.
  - Blue-noise dithering avoids both clumping and regular patterns.
  - NES-style work dithers heavily, while modern pixel art uses dithering sparingly.
  - Sources: [Bitmapped dithering guide](https://martink-rsa.github.io/bitmapped/guides/dithering); [Pixnote dithering](https://pixnote.net/en/learn/dithering); [Divoom blog](https://divoom.com/blogs/setup-ideas/pixel-art-dithering-when-to-use-and-stop)

### Inferences
- Dither rules for this game:
  - Allowed: on floor tiles (concrete wear, moss creeping onto concrete), dithering only between adjacent ramp steps (n and n±1), using checkerboard or sparse 25% patterns. These are 2x2 or 4x4 ordered (Bayer-style) patterns, where 25% means 1 of 4 pixels.
  - Allowed: across the falloff of large static glow pools on the floor, to suggest light without extra colours.
  - Forbidden: on drones, turret heads, projectiles or any animated part, because dither shimmers as it moves.
  - Forbidden: on silhouettes and edges.
  - Forbidden: noise dither everywhere.
- Ordered patterns tile seamlessly on iso tiles only if the pattern period divides the tile dimensions. With 2x2 or 4x4 patterns, use tile widths that are multiples of 4, consistent with Slynyrd's "even dimensions" rule.

### Gaps
- No source specifically addressed dithering under dynamic lighting or engine-side colour tinting.

## 6. Anti-aliasing at small sizes, banding, pillow shading

### Takeaway
At small sprite sizes, use AA minimally and only inside the sprite, never on the outer edge against an unknown background. Avoid banding, where adjacent clusters or lines run parallel and line up. Avoid pillow shading, which darkens from the outline inward. Shade according to the global light and volume instead. Avoid 1 px-thick features.

### Cited Findings
- AA means adding in-between colours "at the corners where two line segments meet". Longer segments need longer AA runs, and AA must be redone each time a new shade is added. "Don't anti-alias the outside of a game sprite," since the background is unknown. [Derek Yu](https://www.derekyu.com/makegames/pixelart.html)
- "Manual AA means smoothing the jagged areas by hand-placing pixels of a different color to ease the transition." "You only want to use as much AA as is necessary to smooth the edge." Use longer lines of AA pixels for gradual transitions. [Cure's tutorial](https://pixeljoint.com/forum/forum_posts.asp?TID=11299)
- Banding is "when pixels line up," which exposes the grid. Named variants are AA banding (AA runs that line up with the line they buffer), fat pixels, skip-one banding and 45-degree banding. Outlines and their interior shapes should not line up. [Cure's tutorial](https://pixeljoint.com/forum/forum_posts.asp?TID=11299)
- Banding makes the eye "focus on the lines where the colors meet instead of the colors themselves." [Derek Yu](https://www.derekyu.com/makegames/pixelart.html)
- Pillow shading is "Shading by surrounding a central area with increasingly darker bands." It's wrong because it "follows flat shapes rather than focuses on how the three-dimensional forms are lit." [Cure's tutorial](https://pixeljoint.com/forum/forum_posts.asp?TID=11299). Pillow shading "almost never occurs naturally in real life" and looks blurry, so treat objects as basic volumes. [Derek Yu, Common Mistakes](https://www.derekyu.com/makegames/pixelart2.html)
- "Chunky Pixels Rule": avoid any element only 1 px thick, because thin protrusions look flat and flimsy. [Derek Yu, Common Mistakes](https://www.derekyu.com/makegames/pixelart2.html)
- Applied example: "Anti-aliasing is applied to smooth harsh edges" in iso tree and house sprites. [Slynyrd Pixelblog 54](https://www.slynyrd.com/blog/2025/1/23/pixelblog-54-isometric-pixel-art)

### Inferences
- In iso, the three-face lighting is the cure for pillow shading. Each face is flat-lit by one rule, so there is never any "darken toward the outline" logic. Round forms (towers, pools, domes) get a single terminator line placed by the global light, not concentric rings.
- 2:1 lines produce regular stair-steps. At 1x, a single AA pixel of an intermediate colour at each step's inner corner is usually enough, and only where the line separates two interior colours.
- Minimum feature widths for small sprites (inference from the Chunky Pixels rule): barrels and antennae at least 2 px, missile bodies at least 2–3 px. A 1 px element is acceptable only as a deliberate glint or emissive line.

### Gaps
- No source gave a numeric minimum sprite size below which AA should be skipped entirely.

## 7. Emissive light and glow: lamps, beacons, energy pools, sparkles, animation frame counts

### Takeaway
Pixel glow comes mainly from colour and value contrast against a dark scene: a near-white core, a saturated mid, and a darker tinted fringe or 1–2 px outline. Soft halos should be used sparingly. Engine-side emission or 2D lights can add true glow on top, without breaking the pixel grid. No sourced animation frame counts were found.

### Cited Findings
- Forum advice: things can be made to glow "without any halo effect" through careful use of colours, optionally with a subtle 1–2 px outline, and halos should be used sparingly. [Pixel Joint](https://pixeljoint.com/pixelart/103437.htm) (from a search summary; exact page attribution uncertain)
- Layered glow method: a small soft halo around each light, then a larger, softer halo, then blurred coloured light spilling across the scene. The author warns that this softer look may not match a pixel-art style. [Eevee Expo resource](https://eeveeexpo.com/resources/710)
- Engine emission: an emission colour multiplied by an intensity makes a sprite glow on its own, in areas marked by an emission map, separately from Light 2D lighting. Test lights as sprites move near them, because light can appear in the wrong place. [Unity Learn: Add Emission Shaders](https://learn.unity.com/tutorial/add-emission-shaders); [Unity Learn: 2D Lighting for Pixel Art](https://learn.unity.com/course/2d-lighting-for-pixel-art)
- Night-scene method: darken or tint the whole scene first to make room for contrast. Start with one main light and add secondary lights afterwards, not all at once. [Pixel Beef devlog: 4 things we did to add light and depth](https://pixel-beef.itch.io/xdasher/devlog/192949/4-things-we-did-to-add-light-and-depth-to-our-pixel-art-game); [Clip Studio: Dynamic Lights Technique](https://tips.clip-studio.com/de-de/articles/1924?org=1) (via search summary)
- Lit materials: warm light glows on brick, there is "a touch of shine" on roofs, shine on rims, and a reflective sheen on windows. [Slynyrd Pixelblog 54](https://www.slynyrd.com/blog/2025/1/23/pixelblog-54-isometric-pixel-art)

### Inferences
- Glow construction for lamps, beacons and energy pools (synthesis):
  1. Core: 1–4 px of near-white, tinted slightly toward the light's hue rather than pure #FFFFFF.
  2. Body: the saturated hue at high value.
  3. Fringe: a 1 px ring in a darker, more saturated version of the hue, not grey, where the glow meets the dark scene.
  4. Spill: the lit hue on adjacent floor tiles and on nearby object faces turned toward the light, done as a separate additive sprite or engine light so it can flicker independently.
  - Keep halos to a few discrete ring steps (e.g. 2–3 rings with dithered or stepped falloff). Avoid smooth blurs, which break the pixel grid unless drawn at the same pixel scale.
- Energy and resource pools: a 2:1 floor ellipse with a darker rim, brighter towards the centre (emissive pools defy normal top-left shading), and 1–2 px bright "caustic" glints that move.
- Animation frame counts. None of these came from a source; they are common practice to validate in-engine:
  - Lamp flicker: 2–4 frames, irregular timing.
  - Beacon pulse: 4–8 frames, ping-pong.
  - Sparkle or glint: 4–6 frames (dot → plus → cross/star → plus → dot).
  - Pool shimmer: 4–8 frames, looping.
  - Drone thruster: 2–4 frames.
  - Muzzle flash: 1–3 frames.
  - Typical sprite playback is 8–12 fps.
  - Tie all loop lengths to a shared timing base, e.g. multiples of 100 ms, so ambient pulses across the map don't look chaotic.
- Readability: brightness should signal meaning. Only interactable or active things (powered lamps, turret charge, refinery output, enemy eyes and thrusters) are allowed to reach near-white.

### Gaps
- No sourced frame counts for glow or sparkle animations were found. Saint11's tutorials, the likely best source for animated FX, are image-based, and the site redirect (saint11.org to saint11.art) and the Medium 403 blocked retrieval.
- No art-direction postmortem (Hyper Light Drifter, Eastward, Sea of Stars) on emissive lighting was retrieved.

## 8. Consistency: style guide, shared palette, shared light, detail density, one pixel scale (no mixels)

### Takeaway
Cohesion comes from fixing a small set of invariants and checking every sprite against them: one palette, one light direction, one projection grid, one pixel scale, one outline policy and one detail density. Mixing pixel sizes (mixels) is the most cited cohesion-breaker. Clarity comes before decoration, as Into the Breach shows.

### Cited Findings
- "For example, color palette, light source, scale, proportions and other style properties should always be consistent." Common iso clusters repeat, so copy-paste reuse is recommended. [Slynyrd Pixelblog 4](https://slynyrd.com/blog/2018/4/12/pixelblog-4-graphical-projection-part-2)
- A shared palette "ties the piece together, unifying the work." [Cure's tutorial](https://pixeljoint.com/forum/forum_posts.asp?TID=11299)
- Stick to the projection's guidelines: "it's best practice to stick to the foundational guide lines of the desired projection." Build each component on a separate layer. [Slynyrd Pixelblog 54](https://www.slynyrd.com/blog/2025/1/23/pixelblog-54-isometric-pixel-art)
- Mixels are pixels of different sizes combined together, within one image or between on-screen elements that clash even when each sprite is internally consistent. [Yari: My fascination with mixels](https://yari-pixels.github.io/Articles/mixels.html) (via search summary)
- Forum rule: the only acceptable mixel use is a background layer, and even there the pixel size may differ between layers but not within one. [Flowlab: Stop using mixels](https://community.flowlab.io/t/mini-rant-stop-using-mixels/38287) (forum opinion)
- Always scale by integer multiples (2x, 3x, 4x) with nearest-neighbour, because non-integer scaling like 1.5x causes uneven pixels. [Pixnote: Pixel Art Resolution Guide](https://pixnote.net/en/learn/resolution) (via search summary)
- Some modern games use mixed resolution successfully, but sparingly and usually scaled in-engine. [Guard Duty dev diary #4](https://www.moddb.com/features/guard-duty-a-development-retrospective-dev-diary-4)
- Into the Breach: "we would sacrifice cool ideas for the sake of clarity every time" (Justin Ma). "The game clearly communicates exactly what every unit on the map is going to do before it does it." Showing an animation of an effect is "a thousand times more effective" than text. [Game Developer: Into the Breach UI design](https://gamedeveloper.com/design/-i-into-the-breach-i-dev-on-ui-design-sacrifice-cool-ideas-for-the-sake-of-clarity-every-time-). Subset kept a "relatively basic pixel art style" and drew the pixel art in Photoshop. [Medium: Subset Games Create Near Perfection](https://medium.com/gametextures/subset-games-create-near-perfection-94688f04847)
- Give each colour a role, and don't let similar colours blur together. [Derek Yu, Common Mistakes](https://www.derekyu.com/makegames/pixelart2.html)

### Inferences
- Proposed one-page style guide (invariants to pin before drawing):
  1. Projection: 2:1 iso. Choose the tile size (e.g. 32x16 footprint) and the corner convention once. Keep the ruler layer and wireframe cuboid in every source file.
  2. Scale: one art pixel = one canvas pixel. Display at an integer scale only (e.g. 2x or 3x) with nearest-neighbour scaling, which in a browser means CSS `image-rendering: pixelated` and integer canvas scaling. Never rotate or scale sprites at runtime by non-integer amounts. Draw rotated turret heads as pre-drawn frames, e.g. 8 or 16 directions, rather than rotating them in code.
  3. Palette: one master palette of 16–32 colours. Each material gets a fixed ramp with fixed indices for top, lit, shadow, edge and AO.
  4. Light: top-left key for all sprites. Lamp light is additive and engine-side.
  5. Outline: none on terrain, dark sel-out on objects, closed outline on enemies.
  6. Detail density: a rule such as "no detail cluster smaller than 2x2 except deliberate glints", with similar numbers of detail clusters per tile footprint across building types. A small building must not be noisier than a big one.
  7. Footprint and height classes: 1x1 lamp or turret, 2x2 refinery or launcher, 3x3 base. Height should signal importance.
  8. Value and hue roles: friendly emissive hue, enemy hue, neutral environment. Verify with a greyscale and squint test on a full mock screen, not on isolated sprites.
- Review process: put every new sprite into a shared "lineup" sheet next to existing sprites, on a representative dark tile with and without lamp light, at 1x and at display scale, before accepting it.

### Gaps
- No studio style-guide document (e.g. from Into the Breach, Hyper Light Drifter, Celeste, Eastward, Kingdom, Sea of Stars or Octopath Traveler) was retrieved with concrete art-direction rules. The Into the Breach material found covers UI and design clarity, not pixel-art specifications.
- Michael Azzi's Pixel Logic and the Brandon James Greer, AdamCYounis and MortMort videos were not accessed. Video content wasn't retrievable as text within this session's limits.
