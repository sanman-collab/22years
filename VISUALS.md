# VISUALS.md — image-generation prompt pack for *Twenty-Two Years*

28 images across 9 decisions. Written for an operator who will generate these
himself and drop them into cards referenced by `decision.visual` in
`content.json`.

Nothing here edits `content.json`. Two fields this pack needs do not exist in
SCHEMA v6 (`visual_caption`, `visual_alt`) — see
[Labels and translation](#labels-and-translation). Per the schema rule I am
flagging them rather than inventing them.

---

## 1. Why this style

Testers asked for pictures in four of five sessions, and both direct quotes are
about *comprehension*, not appeal: "hard to pick blind", "especially tank covers
and water-dispersal systems". The images have one job — make a construction
option unambiguous before the reader commits. That rules out three obvious
alternatives:

- **Photographs.** A real photo of a septic tank is a photo of a specific tank,
  on a specific site, in a specific state of repair. Readers will read dirt,
  poverty and locality into it. It also cannot show a cutaway, which is the
  whole point for `tanktype`, `openings` and `depth`.
- **Illustrated/painted scenes.** Warm illustration flatters the subject. People
  die in these tanks. The register has to stay flat.
- **3D renders.** Perspective, shadow and material shine eat the 420 px budget
  and make two similar options look different for reasons that are not the
  decision.

So: **flat orthographic technical cutaway, two-tone plus one ochre accent, heavy
line weight, zero text.** It is the drawing language a maistry and an engineer
already share, it survives downscaling to a phone card, it reads as an
instrument rather than an opinion, and it costs nothing in neutrality — a
section drawing has no mood.

### Neutrality rules baked into the style

Option order is randomised. An image that pre-judges corrupts the choice and the
research data. These constraints are therefore not aesthetic:

1. **No valence coding.** No ticks, crosses, red, green, warning triangles,
   faces, stars, glows, cracks, rust, leaks, flies, or "clean vs dirty"
   contrast. No element may be drawn as damaged, failing, tidy or desirable.
2. **Locked camera per decision.** Every option of one decision uses the *same*
   view, scale, frame, ground line, line weight and surrounding context. The
   configuration is the only thing that differs. This is a legibility win and a
   neutrality win at once.
3. **Ochre marks the element present in every option of that decision** — never
   the element that varies in presence. Otherwise an option that lacks a feature
   (e.g. `vent-none`) shows no accent and reads as deficient. The per-decision
   ochre target is stated in each section below.
4. **No numbers on dimension lines.** Plain arrowheads only. Comparison comes
   from the locked scale across siblings. Magnitudes belong in the caption.
5. **No people inside, entering, or reaching into a tank. Ever.** Where human
   scale is needed, a plain featureless dark silhouette stands on the ground
   *outside* the tank. No tools in hand except a hose where the prompt says so,
   no uniform, no headload, no bucket, no rag, no face, no clothing detail. The
   silhouette is a ruler, not a character. This is a hard rule and it is why the
   tool exists.
6. **Suction hoses are drawn reaching the tank floor in every `depth` option.**
   Drawing a hose stopping short in the deep option would announce the answer.
   Depth is conveyed by the dimension line against the silhouette.

---

## 2. STYLE BLOCK

Append this verbatim after every per-image prompt. Do not edit it between
images — that is what holds the set together.

```
STYLE: Flat technical cutaway diagram in the manner of a hand-drafted
architectural section. No photorealism, no 3D render, no perspective, no
vanishing points, no cast shadows, no gradients, no gloss, no texture
photography. Strict orthographic projection, flat vector look, crisp clean
edges. Palette limited to exactly four values and nothing else: warm off-white
background #FDFBF7, near-black ink #1A1614 for every outline, the same ink at
about 15 percent opacity for earth and masonry hatching, and a single ochre
#C8871B used only on the element named above. No other colour anywhere, no
blue water, no green plants, no red, no grey steel. Heavy uniform line weight,
roughly 4 pixels at 1536 pixels wide, consistent throughout, no hairlines, no
sketchy double strokes, no fine detail, no small features. Few elements,
generous empty margin on all four sides, centred composition, the whole drawing
readable when shrunk to 420 pixels wide. Indian peri-urban residential
construction: coursed brick masonry, cement plaster, precast concrete rings,
cast RCC slab, plastered block compound wall, bare earth and cement yard
surface. Not an American or European septic system, no moulded plastic tanks,
no lawn, no timber-framed house, no pickup truck. Absolutely no text of any
kind: no letters, no words, no numerals, no units, no labels, no legend, no
key, no callouts, no watermark, no signature, no title block, no scale bar with
writing. No icons, no ticks, no crosses, no exclamation marks, no arrows with
writing, no smiley or sad faces, no emoji. Neutral and non-judgemental: nothing
drawn as broken, dirty, leaking, cracked, dangerous, appealing, premium or
recommended. No people, no figures, no hands, no faces, except where the prompt
explicitly names a plain featureless solid dark silhouette standing on the
ground outside the tank. Aspect ratio 3:2 landscape.
```

**Negative prompt** (for models that take one separately):

```
text, letters, words, numbers, numerals, labels, legend, watermark, signature,
title block, captions, annotations with writing, typography, tick marks, check
marks, crosses, warning signs, emoji, faces, smiley, people, person, worker,
hands, arms, figure inside tank, person in manhole, crowd, photorealistic,
photograph, 3D render, isometric perspective, vanishing point, drop shadow,
gradient, bokeh, lens flare, texture overlay, grunge, rust, cracks, sludge
splatter, flies, green, red, blue, teal, purple, saturated colour, rainbow,
lawn, white picket fence, suburban American house, plastic septic tank, pickup
truck, cartoon, cute, mascot, infographic icon set, thin hairlines, busy
detail, cluttered composition
```

### Generation workflow that actually holds a set together

1. Generate **`openings-covers.png` first**. It is the project's pivot image and
   the densest test of the style.
2. Iterate until it reads at 420 px. Then freeze it as the **style reference**
   and pass it as a reference/image-to-image input for all 27 others. Prompt
   text alone will not hold line weight across 28 generations.
3. Generate the options of one decision **in the same session, back to back**,
   with the first sibling also passed as a reference. This is what enforces the
   locked camera.
4. Regenerate the whole decision, not one option, if any sibling drifts. Three
   consistent images beat one perfect one.

---

## 3. Size and aspect ratio

| | Value |
|---|---|
| Aspect ratio | **3:2 landscape**, all 28, no exceptions |
| Generate at | **1536 × 1024** |
| Ship at | **840 × 560** (2× a 420 × 280 card slot) |
| Format | PNG, 8-bit, no alpha |
| Weight target | under 80 KB each after `pngquant`/`oxipng`; the set is 28 files on a phone over mobile data |

One ratio for all 28 even though plan views (`location`, `truck_access`) would
prefer 4:3. Mixed ratios make a scrolling thread look broken, and a thread is
exactly what v6 is. Compose within 3:2 instead — for plan views, rotate the plot
so the road runs along the long bottom edge.

Do not bake in rounded corners or a drop shadow. The card does that in CSS.

**Background must stay opaque #FDFBF7.** A transparent PNG with #1A1614 lines
disappears in dark mode. An opaque warm panel reads as a drawing pinned to the
card in both themes. Pair it in CSS with a 1 px border at `rgba(26,22,20,.12)`
and 12 px of internal padding inside the image itself so the drawing never
touches the panel edge.

---

## 4. Labels and translation

The app ships in Tamil, Hindi, Odia and Assamese. **Text baked into an image
cannot be translated, cannot be read by a screen reader, and cannot be fixed
without regenerating the asset.** Image models also produce malformed glyphs in
Indic scripts, so a baked Tamil label would be worse than no label.

**Rule: all 28 images contain zero text. No letters, no numerals, no units, not
even on dimension lines.** The STYLE BLOCK and negative prompt enforce this. An
image that comes back with text, however small or decorative, is rejected and
regenerated — do not crop or paint it out, because partial glyph residue looks
like damage.

Where a magnitude genuinely must be communicated — `openings` (2 × 2 ft vs 1½
ft), `depth` (~2 m suction limit), `truck_access` (3 m width, 30 m hose),
`sizing` (8 × 4 × 5 ft) — it goes in a **translatable caption below the image,
not inside it**. Those four decisions are exactly the ones where a dimension
arrow is drawn with no number, so the caption is load-bearing, not decorative.

This needs two fields that SCHEMA v6 does not define. **Flagging, not adding:**

```jsonc
"decisions": {
  "openings": {
    "visual": "openings",          // exists in v6
    "visual_caption": "...",       // PROPOSED. Translatable. One line under
                                   // the image. Carries every number.
    "visual_alt": "..."            // PROPOSED. Translatable. Screen-reader
                                   // description. Required for accessibility;
                                   // a decision card that is only an image is
                                   // unusable without it.
  }
}
```

Per-option images need per-option captions, so in practice these belong on
`options[]` as well as on the decision. Decide centrally before any image is
wired in; the images themselves do not change either way.

---

## 5. The prompts

Each entry: filename, the prompt to paste (then the STYLE BLOCK), and what a
reader must be able to tell at a glance.

---

### 5.1 `location` — 3 images

**Locked camera:** identical top-down site plan in all three. Same plot outline,
same compound wall, same gate position, same house footprint, same road. Only
the tank position and the route line move.
**Ochre:** the tank rectangle and the route line (present in all three).

---

**`location-vastu.png`**

```
Flat top-down architectural site plan of a small rectangular Indian residential
plot, roughly 30 by 40 feet, drawn orthographically from directly above. A
public road runs as a plain band along the entire bottom edge of the frame. The
plot is enclosed on all four sides by a plastered compound wall drawn as a thick
double line, with one single gate opening in the bottom wall, offset to the
right. Inside the plot, the house footprint is a simple rectangle with thick
hatched walls, set back from the road, filling the centre and the left of the
plot, leaving an open yard strip along the rear and right. A small plain
triangular north arrow with no letter sits in the top right corner of the frame,
pointing up. A small rectangle representing the underground septic tank sits in
the top left corner of the plot, in the far corner diagonally opposite the gate,
with the house standing between it and the gate. A dashed route line runs from a
point on the road, in through the gate, around the right side of the house,
along the rear yard and to the tank, following the open ground, with a plain
arrowhead at the tank end. Keep the plot outline, compound wall, gate position,
house footprint, road band and north arrow exactly as described so this drawing
can be compared side by side with its siblings. Ochre: the septic tank rectangle
and the dashed route line. Everything else in ink.
```

*At a glance:* the tank is in the far corner, with the house between it and the
gate, and the route to it is long and bent.

---

**`location-behind.png`**

```
Flat top-down architectural site plan of a small rectangular Indian residential
plot, roughly 30 by 40 feet, drawn orthographically from directly above. A
public road runs as a plain band along the entire bottom edge of the frame. The
plot is enclosed on all four sides by a plastered compound wall drawn as a thick
double line, with one single gate opening in the bottom wall, offset to the
right. Inside the plot, the house footprint is a simple rectangle with thick
hatched walls, set back from the road, filling the centre and the left of the
plot, leaving an open yard strip along the rear and right. A small plain
triangular north arrow with no letter sits in the top right corner of the frame,
pointing up. A small rectangle representing the underground septic tank sits in
the narrow rear yard strip, centred directly behind the house along the top
wall, tight against the back of the house, with the full depth of the house
between it and the gate. A dashed route line runs from a point on the road, in
through the gate, up the narrow right-hand gap between the house and the
compound wall, then left along the rear strip to the tank, with a plain
arrowhead at the tank end. Keep the plot outline, compound wall, gate position,
house footprint, road band and north arrow exactly as described so this drawing
can be compared side by side with its siblings. Ochre: the septic tank rectangle
and the dashed route line. Everything else in ink.
```

*At a glance:* the tank is directly behind the house, reachable only by a narrow
side gap.

---

**`location-gate.png`**

```
Flat top-down architectural site plan of a small rectangular Indian residential
plot, roughly 30 by 40 feet, drawn orthographically from directly above. A
public road runs as a plain band along the entire bottom edge of the frame. The
plot is enclosed on all four sides by a plastered compound wall drawn as a thick
double line, with one single gate opening in the bottom wall, offset to the
right. Inside the plot, the house footprint is a simple rectangle with thick
hatched walls, set back from the road, filling the centre and the left of the
plot, leaving an open yard strip along the rear and right. A small plain
triangular north arrow with no letter sits in the top right corner of the frame,
pointing up. A small rectangle representing the underground septic tank sits in
the open front yard just inside the compound wall, a short distance to the left
of the gate opening, between the house and the road, with clear open ground
between it and the gate. A short dashed route line runs from a point on the
road, in through the gate, and straight across to the tank, with a plain
arrowhead at the tank end. Keep the plot outline, compound wall, gate position,
house footprint, road band and north arrow exactly as described so this drawing
can be compared side by side with its siblings. Ochre: the septic tank rectangle
and the dashed route line. Everything else in ink.
```

*At a glance:* the tank is just inside the gate with nothing between it and the
road.

---

### 5.2 `tanktype` — 4 images

**Locked camera:** identical vertical cutaway section, same frame, same ground
line, same overall outer height and width of the tank, same inlet and outlet
heights, same liquid line, same sludge band. Only the wall construction and the
internal arrangement differ.
**Ochre:** the tank's structural wall material (present in all four).
**Jargon note:** `precast`, `baffle wall` were both flagged as blocking terms.
These four images are the definition; the labels should lean on them.

---

**`tanktype-rings.png`**

```
Flat technical cutaway vertical section through an underground household septic
tank on an Indian house plot. A ground line runs across the full width of the
frame with light 15 percent ink hatching for earth below it. The tank is cut
open so its hollow interior is visible. An inlet pipe enters through the wall
near the top left; an outlet pipe leaves through the wall near the top right. A
cast RCC cover slab spans the top of the tank at ground level. Inside, the
liquid level is one flat horizontal line about three quarters up, and a solid
dark sludge band lies along the floor. The tank is circular in plan so it is
drawn in section as a vertical cylindrical shaft, built from three stacked
precast concrete rings of equal height: draw the two horizontal joints between
the rings as thick unbroken lines right across both wall thicknesses, and keep
the wall faces plain smooth concrete with no brick coursing and no plaster. The
internal bore of the top ring sets the width of a single narrow circular opening
in the cover slab directly above it. Same frame, same ground line and same
overall tank height and width as its siblings. Ochre: the concrete ring walls
and floor. Everything else in ink.
```

*At a glance:* a round shaft of stacked concrete rings, one compartment, and the
ring bore sets how wide the top opening can be.

---

**`tanktype-well.png`**

```
Flat technical cutaway vertical section through an underground household septic
tank on an Indian house plot. A ground line runs across the full width of the
frame with light 15 percent ink hatching for earth below it. The tank is cut
open so its hollow interior is visible. An inlet pipe enters through the wall
near the top left; an outlet pipe leaves through the wall near the top right. A
cast RCC cover slab spans the top of the tank at ground level with a single
opening in it. Inside, the liquid level is one flat horizontal line about three
quarters up, and a solid dark sludge band lies along the floor. The tank is a
circular shaft lined with irregular rubble stone in the manner of an old
step-well: draw the stones as roughly rectangular and polygonal blocks of
varying size stacked in uneven courses, with clearly visible open gaps between
them instead of continuous mortar, and the shaft floor left as bare earth rather
than a cast slab. Draw four or five simple thin root lines passing out of the
earth hatching, through the open stone joints, into the tank. Same frame, same
ground line and same overall tank height and width as its siblings. Ochre: the
rubble stone wall courses. Everything else in ink.
```

*At a glance:* a stone-lined shaft with open joints and an earth floor — water
and roots pass both ways.

---

**`tanktype-brick.png`**

```
Flat technical cutaway vertical section through an underground household septic
tank on an Indian house plot. A ground line runs across the full width of the
frame with light 15 percent ink hatching for earth below it. The tank is cut
open so its hollow interior is visible. An inlet pipe enters through the wall
near the top left; an outlet pipe leaves through the wall near the top right. A
cast RCC cover slab spans the top of the tank at ground level with a single
opening in it. Inside, the liquid level is one flat horizontal line about three
quarters up, and a solid dark sludge band lies along the floor. The tank is
rectangular in section, one single undivided chamber with no internal walls, its
side walls built of regularly coursed brick drawn as a simple even running-bond
pattern of small rectangles, standing on a cast concrete floor slab. On the
inside face of both walls and across the floor, draw one continuous unbroken
thick line hugging the masonry to represent a smooth cement plaster skin, with
rounded junctions at the wall-to-floor corners. Same frame, same ground line and
same overall tank height and width as its siblings. Ochre: the brick walls,
floor slab and the plaster skin line. Everything else in ink.
```

*At a glance:* one rectangular brick box, smooth-lined inside, still a single
compartment.

---

**`tanktype-twochamber.png`**

```
Flat technical cutaway vertical section through an underground household septic
tank on an Indian house plot. A ground line runs across the full width of the
frame with light 15 percent ink hatching for earth below it. The tank is cut
open so its hollow interior is visible. A cast RCC cover slab spans the top of
the tank at ground level. Inside, the liquid level is one flat horizontal line
about three quarters up, and a solid dark sludge band lies along the floor. The
tank is rectangular in section with regularly coursed brick walls drawn as an
even running-bond pattern, a cast concrete floor slab, and a continuous plaster
line on all inner faces. It is divided into two unequal chambers by a single
vertical brick baffle wall: the left chamber is about twice as wide as the right
one. The baffle wall stops clearly short of the floor, leaving an open gap
underneath, and clearly short of the cover slab, leaving an open gap above, so
liquid can pass both under and over it. An inlet pipe enters the top left of the
left chamber and turns down as a short vertical T-piece. An outlet pipe leaves
the top right of the right chamber, also with a short vertical T-piece dipping
below the liquid line. The cover slab has one square opening above each chamber,
two in total. Same frame, same ground line and same overall tank height and
width as its siblings. Ochre: the brick walls, floor slab and the baffle wall.
Everything else in ink.
```

*At a glance:* two chambers split by an internal wall that water passes under
and over, with an opening above each.

---

### 5.3 `openings` — 3 images

**Locked camera:** identical section, cropped tighter than `tanktype` so the top
of the tank occupies the upper half of the frame. Same tank body, same yard
surface, same slab thickness in all three. Only the top treatment differs.
**Ochre:** the RCC cover slab (present in all three).
**This is the project's pivot image set.** Generate `openings-covers.png` first
and use it as the style reference for the whole pack.
**Jargon note:** `flush slab` was one of two terms that actually blocked
comprehension. This image is the definition.

---

**`openings-flush.png`**

```
Flat technical cutaway vertical section through the top of an underground
household septic tank on an Indian house plot, cropped so the cover slab and the
upper half of the tank fill the frame. A ground line runs across the full width
as a cement yard surface with a simple paved finish; light 15 percent ink
hatching for earth below it on both sides. The tank below is cut open: coursed
brick walls, a plaster line inside, and a flat liquid level line near the bottom
of the frame, the hollow air space above the liquid left empty. A cast RCC cover
slab spans the full width of the tank and sits flush with the yard surface. The
slab is completely continuous and unbroken: no opening of any kind anywhere in
it, no hatch, no lid, no frame, no joint, no pipe passing through it, and a
cement plaster screed carried smoothly across its whole top face and onto the
surrounding yard surface so the slab and the yard read as one continuous
surface. Nothing protrudes above ground level anywhere in the drawing. Same
frame, same ground line, same tank body and same slab thickness as its siblings.
Ochre: the RCC cover slab. Everything else in ink.
```

*At a glance:* the top is one unbroken slab, plastered over, with no way in.

---

**`openings-small.png`**

```
Flat technical cutaway vertical section through the top of an underground
household septic tank on an Indian house plot, cropped so the cover slab and the
upper half of the tank fill the frame. A ground line runs across the full width
as a cement yard surface with a simple paved finish; light 15 percent ink
hatching for earth below it on both sides. The tank below is cut open: coursed
brick walls, a plaster line inside, and a flat liquid level line near the bottom
of the frame, the hollow air space above the liquid left empty. A cast RCC cover
slab spans the full width of the tank and sits flush with the yard surface. In
the centre of the slab there is exactly one single small opening, its width
about one quarter of the tank's width, closed by a plain thin circular precast
concrete disc lid resting in it with no frame and no lifting handle. A thin
cement plaster skim is carried over the edge of the disc where it meets the
slab, so the joint almost disappears. There is no second opening. Nothing
protrudes above ground level. A short horizontal ink dimension line with plain
arrowheads at both ends spans the width of the opening, directly above the slab;
it carries no number and no text of any kind. Same frame, same ground line, same
tank body and same slab thickness as its siblings. Ochre: the RCC cover slab.
Everything else in ink.
```

*At a glance:* one small round hole in the middle of the slab, part-skimmed
over, narrow relative to the tank.

---

**`openings-covers.png`**

```
Flat technical cutaway vertical section through the top of an underground
household septic tank on an Indian house plot, cropped so the cover slab and the
upper half of the tank fill the frame. A ground line runs across the full width
as a cement yard surface with a simple paved finish; light 15 percent ink
hatching for earth below it on both sides. The tank below is cut open: coursed
brick walls, a plaster line inside, and a flat liquid level line near the bottom
of the frame, the hollow air space above the liquid left empty. A cast RCC cover
slab spans the full width of the tank and sits flush with the yard surface. The
slab has exactly two square openings in it, side by side and evenly spaced, each
one about one quarter of the tank's width. Each opening has a rectangular cast
frame around its rim, drawn in section as a small L-shaped rebate, carrying a
square removable concrete cover that sits exactly level with the yard surface,
with a simple recessed lifting ring in its top face. The surrounding plaster
stops cleanly at each frame rather than running over it. Nothing protrudes above
ground level. A short horizontal ink dimension line with plain arrowheads at
both ends spans the width of each opening, directly above the slab; they carry
no numbers and no text of any kind. Same frame, same ground line, same tank body
and same slab thickness as its siblings. Ochre: the RCC cover slab. Everything
else in ink.
```

*At a glance:* two square framed covers sitting level with the ground, each
clearly wider than a hole an arm would fit through.

---

### 5.4 `slab_call` — 3 images

Same physical object as `openings`, at a different moment: the maistry's view on
pour day, before the concrete sets. That difference in moment is the whole point
of the decision — it is reversible for one more hour.

**Locked camera:** identical three-quarter cutaway of the tank under
construction, drawn flat without perspective distortion, slightly above and to
one side so the deck surface is visible as a foreshortened rectangle. Same tank
walls, same formwork, same props, same crop in all three.
**Ochre:** the steel reinforcement grid across the deck (present in all three).
**Hard rule check:** the tank below is open on pour day. No figure anywhere near
the opening. The only permitted figure is a silhouette standing on firm ground
at the edge of the frame, and only in `slab_call-ask`.

---

**`slab_call-flush.png`**

```
Flat technical three-quarter cutaway drawing of an underground household septic
tank on an Indian building site on the day the top slab is cast, seen from
slightly above and to one side but drawn flat and orthographic with no
perspective distortion and no vanishing point. The tank's brick walls are built
up to ground level and cut open at the near corner so the dark empty void inside
is visible as a plain hollow. Across the top of the walls, rough timber
shuttering planks are laid as a deck, supported from inside the tank by three
plain vertical timber props standing on the tank floor. Over the deck lies a
steel reinforcement grid drawn as evenly spaced straight bars crossing at right
angles, with a few bars bent up at the ends. The reinforcement grid runs
completely continuously across the entire deck from edge to edge, with no gaps,
no boxes, no voids and no marked-out areas anywhere in it. Beside the tank,
bare site ground with a simple heap of sand and a stack of four or five bricks.
No people anywhere in the drawing. Same frame, same tank, same formwork and
same props as its siblings. Ochre: the steel reinforcement grid. Everything
else in ink.
```

*At a glance:* the steel runs unbroken across the whole deck — once this is
poured there is no opening.

---

**`slab_call-ask.png`**

```
Flat technical three-quarter cutaway drawing of an underground household septic
tank on an Indian building site on the day the top slab is cast, seen from
slightly above and to one side but drawn flat and orthographic with no
perspective distortion and no vanishing point. The tank's brick walls are built
up to ground level and cut open at the near corner so the dark empty void inside
is visible as a plain hollow. Across the top of the walls, rough timber
shuttering planks are laid as a deck, supported from inside the tank by three
plain vertical timber props standing on the tank floor. Over the deck lies a
steel reinforcement grid drawn as evenly spaced straight bars crossing at right
angles. The grid is still continuous, but two square areas on it, side by side,
are marked out with dashed outlines only: no boxes have been built yet and no
bars have been cut. On the finished brick wall edge in the foreground lie two
plain objects: a rolled-up paper drawing tied with a single band, and a simple
rectangular mobile phone, both drawn as flat blank outlines with no screen
content and no writing. Standing on firm bare ground at the far left edge of the
frame, well away from the open tank, is one plain featureless solid dark
silhouette of a standing adult, no face, no clothing detail, holding nothing.
Beside the tank, bare site ground with a simple heap of sand and a stack of
four or five bricks. Same frame, same tank, same formwork and same props as its
siblings. Ochre: the steel reinforcement grid and the two dashed square
outlines on it. Everything else in ink.
```

*At a glance:* the two openings are only marked out, not built — the call has
not been made and the phone is right there.

---

**`slab_call-covers.png`**

```
Flat technical three-quarter cutaway drawing of an underground household septic
tank on an Indian building site on the day the top slab is cast, seen from
slightly above and to one side but drawn flat and orthographic with no
perspective distortion and no vanishing point. The tank's brick walls are built
up to ground level and cut open at the near corner so the dark empty void inside
is visible as a plain hollow. Across the top of the walls, rough timber
shuttering planks are laid as a deck, supported from inside the tank by three
plain vertical timber props standing on the tank floor. Over the deck lies a
steel reinforcement grid drawn as evenly spaced straight bars crossing at right
angles. Set into the deck are two square open-topped box-outs, side by side and
evenly spaced, each built of four rough timber planks forming a small square
well that stands slightly proud of the deck surface, each one about one quarter
of the tank's width. The reinforcement bars are cut at each box and turned up
along its sides, with the cut ends clearly visible. Beside the tank, bare site
ground with a simple heap of sand, a stack of four or five bricks, and two
spare square timber box frames lying flat on the ground. No people anywhere in
the drawing. Same frame, same tank, same formwork and same props as its
siblings. Ochre: the steel reinforcement grid. Everything else in ink.
```

*At a glance:* two timber boxes are built into the deck, so the poured slab will
come out with two openings in it.

---

### 5.5 `vent` — 3 images

**Locked camera:** identical combined elevation and section. Same house, same
parapet height, same roof line, same tank, same crop in all three. Only the pipe
differs.
**Ochre:** the tank's RCC cover slab (present in all three), so the option with
no pipe still carries an accent and does not read as deficient. The vent pipe is
drawn in heavy ink.
**Jargon note:** `parapet` and `cowl` were both flagged. These images define
them.

---

**`vent-none.png`**

```
Flat technical drawing combining an elevation and a section, orthographic, no
perspective. Occupying the left two thirds of the frame: a simple single-storey
Indian house in flat front elevation, plastered masonry walls, one door and one
window drawn as plain rectangles with no glazing detail, a flat concrete roof,
and a low parapet wall standing along the front edge of the roof, drawn as a
plain band. Occupying the right third: a ground line continuing out from the
house across the frame, light 15 percent ink earth hatching below it, and an
underground household septic tank drawn in cutaway section below ground, with
coursed brick walls, a flat liquid level line, a sludge band on the floor, an
inlet pipe running underground from the house into its top left, and an outlet
pipe leaving its top right. A cast RCC cover slab spans the top of the tank at
ground level. There is no vent pipe anywhere in the drawing: the cover slab is
unbroken by any vertical pipe, nothing rises from the tank, nothing runs up the
house wall, and nothing stands above the roof or the parapet. The air space
between the liquid line and the underside of the slab is drawn as a plain sealed
empty void. Same house, same parapet height, same roof line, same tank and same
frame as its siblings. Ochre: the RCC cover slab. Everything else in ink.
```

*At a glance:* nothing rises from the tank — the air space under the slab is
sealed shut.

---

**`vent-short.png`**

```
Flat technical drawing combining an elevation and a section, orthographic, no
perspective. Occupying the left two thirds of the frame: a simple single-storey
Indian house in flat front elevation, plastered masonry walls, one door and one
window drawn as plain rectangles with no glazing detail, a flat concrete roof,
and a low parapet wall standing along the front edge of the roof, drawn as a
plain band. Occupying the right third: a ground line continuing out from the
house across the frame, light 15 percent ink earth hatching below it, and an
underground household septic tank drawn in cutaway section below ground, with
coursed brick walls, a flat liquid level line, a sludge band on the floor, an
inlet pipe running underground from the house into its top left, and an outlet
pipe leaving its top right. A cast RCC cover slab spans the top of the tank at
ground level. One short straight vertical PVC vent pipe rises from the cover
slab, passing through it into the tank's air space, runs up the outside of the
house wall held by two simple pipe clamps, and stops well below the top of the
roof parapet, so the parapet wall stands clearly taller than the pipe. The pipe
ends in a plain open cut end with no cap, no hood and no cowl. Same house, same
parapet height, same roof line, same tank and same frame as its siblings.
Ochre: the RCC cover slab. Everything else in ink.
```

*At a glance:* a stub of pipe that stops below the parapet, open-topped, with
the parapet standing over it.

---

**`vent-proper.png`**

```
Flat technical drawing combining an elevation and a section, orthographic, no
perspective. Occupying the left two thirds of the frame: a simple single-storey
Indian house in flat front elevation, plastered masonry walls, one door and one
window drawn as plain rectangles with no glazing detail, a flat concrete roof,
and a low parapet wall standing along the front edge of the roof, drawn as a
plain band. Occupying the right third: a ground line continuing out from the
house across the frame, light 15 percent ink earth hatching below it, and an
underground household septic tank drawn in cutaway section below ground, with
coursed brick walls, a flat liquid level line, a sludge band on the floor, an
inlet pipe running underground from the house into its top left, and an outlet
pipe leaving its top right. A cast RCC cover slab spans the top of the tank at
ground level. One straight vertical PVC vent pipe rises from the cover slab,
passing through it into the tank's air space, runs the full height of the house
wall held by four simple pipe clamps, continues past the roof slab and well
above the top of the parapet, ending roughly a person's height above the roof
level and clear of the window below. The top of the pipe is fitted with a cowl
drawn as a small inverted cup hood sitting a short gap above the open pipe
mouth on two thin legs, so the opening faces downward and sideways rather than
straight up. Same house, same parapet height, same roof line, same tank and
same frame as its siblings. Ochre: the RCC cover slab. Everything else in ink.
```

*At a glance:* a pipe running the full height of the wall, finishing well above
the roof and parapet, with a hood over its open end.

---

### 5.6 `effluent` — 3 images

Explicitly asked for by testers ("water-dispersal systems"). `effluent`,
`soak pit` and `dispersion trench` were all flagged as jargon; these are the
definitions.

**Locked camera:** identical wide shallow section. Same tank on the left at the
same size, same outlet pipe at the same height, same ground line, same compound
wall and road band at the right edge. Only what happens after the outlet
differs.
**Ochre:** the outlet pipe and everything it discharges into (present in all
three).

---

**`effluent-storm.png`**

```
Flat technical cutaway section, wide and shallow, orthographic, no perspective,
showing where the water leaving a household septic tank goes. A ground line runs
across the whole frame with light 15 percent ink earth hatching below it. At the
left, an underground septic tank in cutaway section: coursed brick walls, cast
cover slab at ground level, flat liquid level line, sludge band on the floor,
and an outlet pipe leaving its upper right wall just below the liquid line. At
the right edge of the frame, a plastered compound wall rising from the ground
line, and beyond it a plain band of public road. The outlet pipe runs almost
horizontally from the tank straight to the right, with a very slight fall,
passes underneath the compound wall, and discharges through the side of an open
street storm drain that runs along the edge of the road. The storm drain is
drawn in section as a wide rectangular masonry channel open to the sky, with a
shallow flat liquid line in the bottom and two loose precast slab covers resting
across only part of its width, leaving the rest open. No water leaves the
drawing into the earth hatching; the earth below the pipe run is undisturbed
hatch. Same tank, same outlet height, same ground line, same compound wall and
same road band as its siblings. Ochre: the outlet pipe and the storm drain
channel. Everything else in ink.
```

*At a glance:* the outlet runs straight out under the wall into an open street
channel.

---

**`effluent-soak.png`**

```
Flat technical cutaway section, wide and shallow, orthographic, no perspective,
showing where the water leaving a household septic tank goes. A ground line runs
across the whole frame with light 15 percent ink earth hatching below it. At the
left, an underground septic tank in cutaway section: coursed brick walls, cast
cover slab at ground level, flat liquid level line, sludge band on the floor,
and an outlet pipe leaving its upper right wall just below the liquid line. At
the right edge of the frame, a plastered compound wall rising from the ground
line, and beyond it a plain band of public road. The outlet pipe runs only a
short distance to the right, no further than its own length from the tank wall,
and discharges into a soak pit built immediately beside the tank: a deep
cylindrical pit drawn in section, its sides lined with a ring of brick laid with
open gaps between the bricks, filled from floor to near the top with graded
stone drawn as coarse rounded pebbles of two sizes, a small plain precast cover
slab over its mouth at ground level, and bare earth at its base. Short dashed
lines spread outward and downward from the pit's open brick sides and its base
into the surrounding earth hatching. The remaining width of the frame between
the soak pit and the compound wall is plain undisturbed earth hatch. Same tank,
same outlet height, same ground line, same compound wall and same road band as
its siblings. Ochre: the outlet pipe and the soak pit. Everything else in ink.
```

*At a glance:* a single deep stone-filled pit right next to the tank, water
seeping out of its sides and bottom.

---

**`effluent-dispersion.png`**

```
Flat technical cutaway section, wide and shallow, orthographic, no perspective,
showing where the water leaving a household septic tank goes. A ground line runs
across the whole frame with light 15 percent ink earth hatching below it. At the
left, an underground septic tank in cutaway section: coursed brick walls, cast
cover slab at ground level, flat liquid level line, sludge band on the floor,
and an outlet pipe leaving its upper right wall just below the liquid line. At
the right edge of the frame, a plastered compound wall rising from the ground
line, and beyond it a plain band of public road. The outlet pipe runs a short
distance to the right into a small rectangular masonry distribution chamber set
just below ground with its own small cover. From that chamber, two long
horizontal perforated pipes run away to the right at slightly different depths,
each laid in its own shallow trench, continuing almost to the compound wall so
the trenches take up most of the frame's width. Each trench is drawn in section
as a wide shallow band filled with coarse angular gravel, with the perforated
pipe shown as a tube with small regular holes along its underside, and one thin
continuous line laid across the top of the gravel to represent a geotextile
sheet, with earth backfill hatch above that up to the ground line. Many short
dashed lines spread downward from the full length of both trenches into the
earth hatching below. Same tank, same outlet height, same ground line, same
compound wall and same road band as its siblings. Ochre: the outlet pipe, the
distribution chamber and the two trench pipes. Everything else in ink.
```

*At a glance:* the water is spread along two long shallow gravel trenches across
the whole plot, not dumped in one place.

---

### 5.7 `depth` — 3 images

**Locked camera:** identical section, identical ground line position, identical
silhouette height, identical suction hose, identical frame. The tank's enclosed
area should look roughly the same in all three so the reader compares *shape*,
not capacity.
**Ochre:** the tank outline and the vertical depth dimension line (present in
all three).
**Pre-judging guard:** the hose is drawn reaching the floor in all three. Do not
draw it stopping short, kinked, or failing — that announces the answer before
the reader chooses. The ~2 m suction limit belongs in the caption and the reveal
text.

---

**`depth-deep.png`**

```
Flat technical cutaway vertical section through an underground household septic
tank on an Indian house plot, orthographic, no perspective. A ground line runs
across the full width of the frame near the top, with light 15 percent ink earth
hatching filling everything below it. The tank is cut open: coursed brick walls,
a cast concrete floor slab, a cast RCC cover slab at ground level with one
single square opening in it, a flat liquid level line about three quarters up,
and a solid dark sludge band lying along the floor. A flexible corrugated
suction hose enters through the square opening from above, curving over the slab
edge, and runs straight down the inside of the tank to rest on the floor in the
sludge band. Standing on the yard surface at the far left of the frame, on firm
ground well clear of the tank and not touching it, is one plain featureless
solid dark silhouette of a standing adult: no face, no clothing detail, holding
nothing, both feet on the ground. At the right of the tank, outside it in the
earth hatching, a vertical dimension line with plain arrowheads at both ends
runs from the ground line down to the tank floor level, with thin extension
lines; it carries no number, no unit and no text. In this drawing the tank is
narrow and tall: its internal depth is about one and a half times the standing
silhouette's height, and its internal width is roughly equal to its depth. Same
frame, same ground line position, same silhouette height and same hose as its
siblings. Ochre: the tank outline and the vertical dimension line. Everything
else in ink.
```

*At a glance:* the tank floor is far below the figure's own height — a tall
narrow shaft.

---

**`depth-standard.png`**

```
Flat technical cutaway vertical section through an underground household septic
tank on an Indian house plot, orthographic, no perspective. A ground line runs
across the full width of the frame near the top, with light 15 percent ink earth
hatching filling everything below it. The tank is cut open: coursed brick walls,
a cast concrete floor slab, a cast RCC cover slab at ground level with one
single square opening in it, a flat liquid level line about three quarters up,
and a solid dark sludge band lying along the floor. A flexible corrugated
suction hose enters through the square opening from above, curving over the slab
edge, and runs straight down the inside of the tank to rest on the floor in the
sludge band. Standing on the yard surface at the far left of the frame, on firm
ground well clear of the tank and not touching it, is one plain featureless
solid dark silhouette of a standing adult: no face, no clothing detail, holding
nothing, both feet on the ground. At the right of the tank, outside it in the
earth hatching, a vertical dimension line with plain arrowheads at both ends
runs from the ground line down to the tank floor level, with thin extension
lines; it carries no number, no unit and no text. In this drawing the tank's
internal depth is about the same as the standing silhouette's height, and its
internal width is about twice its depth. Same frame, same ground line position,
same silhouette height and same hose as its siblings. Ochre: the tank outline
and the vertical dimension line. Everything else in ink.
```

*At a glance:* the tank is about as deep as the figure is tall, and twice as
wide as it is deep.

---

**`depth-shallow.png`**

```
Flat technical cutaway vertical section through an underground household septic
tank on an Indian house plot, orthographic, no perspective. A ground line runs
across the full width of the frame near the top, with light 15 percent ink earth
hatching filling everything below it. The tank is cut open: coursed brick walls,
a cast concrete floor slab, a cast RCC cover slab at ground level with one
single square opening in it, a flat liquid level line about three quarters up,
and a solid dark sludge band lying along the floor. A flexible corrugated
suction hose enters through the square opening from above, curving over the slab
edge, and runs down the inside of the tank to rest on the floor in the sludge
band. Standing on the yard surface at the far left of the frame, on firm ground
well clear of the tank and not touching it, is one plain featureless solid dark
silhouette of a standing adult: no face, no clothing detail, holding nothing,
both feet on the ground. At the right of the tank, outside it in the earth
hatching, a vertical dimension line with plain arrowheads at both ends runs from
the ground line down to the tank floor level, with thin extension lines; it
carries no number, no unit and no text. In this drawing the tank is shallow and
very wide: its internal depth is about half the standing silhouette's height,
and its internal width is about four times its depth, so the tank stretches
across most of the frame. Same frame, same ground line position, same silhouette
height and same hose as its siblings. Ochre: the tank outline and the vertical
dimension line. Everything else in ink.
```

*At a glance:* a wide shallow pan whose floor is only waist-deep relative to the
figure.

---

### 5.8 `truck_access` — 3 images

**Honest caveat first.** This decision is epistemic, not spatial: the options
are "assume", "measure", "measure and check the hose reach". Options two and
three describe the same site with different knowledge about it. Three images
risk reading as "more annotation equals better answer", which is exactly the
pre-judging the brief forbids. Mitigation below: **all three carry the same two
ink dimension arrows** (gate width, road width), and the geometry shown differs
instead. See [Section 7](#7-budget-if-only-ten) — if anything gets cut, this
goes first and is replaced by one shared diagram on the decision card.

**Locked camera:** identical top-down plan, same plot, compound wall, gate
position and house footprint as the `location` drawings.
**Ochre:** the desludging truck (present in all three). Hose route and arcs in
ink, so no option gains accent weight from extra annotation.

---

**`truck_access-assume.png`**

```
Flat top-down site plan, orthographic from directly above, of a small
rectangular Indian residential plot roughly 30 by 40 feet. A public road runs as
a plain band along the entire bottom edge of the frame. The plot is enclosed by a
plastered compound wall drawn as a thick double line, with one gate opening in
the bottom wall, offset to the right, closed, drawn as two leaves. Inside, the
house footprint is a simple rectangle with thick hatched walls set back from the
road, and a small rectangle representing the underground septic tank sits in the
open front yard to the left of the gate. On the road directly outside the gate
stands a desludging vacuum tanker truck drawn in plain top-down outline: a
rectangular chassis, a cab at the front, a large horizontal cylindrical vacuum
tank occupying most of its length, and wheels drawn as small rectangles along
both sides. The truck is parked parallel to the compound wall, nose to the
right, taking up much of the road width. Two smaller plain vehicle outlines are
parked on the road, one close in front of the truck and one close behind it,
leaving almost no clear space at either end. Two ink dimension lines with plain
arrowheads at both ends are drawn: one across the gate opening, and one across
the full width of the road; neither carries any number, unit or text. There is
no route line, no hose, and no arc anywhere in the drawing. Keep the plot
outline, compound wall, gate position and house footprint identical to its
siblings. Ochre: the tanker truck. Everything else in ink.
```

*At a glance:* a long truck boxed in on a road, parked outside a closed gate
narrower than the truck.

---

**`truck_access-measure.png`**

```
Flat top-down site plan, orthographic from directly above, of a small
rectangular Indian residential plot roughly 30 by 40 feet. A public road runs as
a plain band along the entire bottom edge of the frame. The plot is enclosed by a
plastered compound wall drawn as a thick double line, with one gate opening in
the bottom wall, offset to the right, standing open, drawn as two leaves swung
back. Inside, the house footprint is a simple rectangle with thick hatched walls
set back from the road, and a small rectangle representing the underground
septic tank sits in the open front yard to the left of the gate. A desludging
vacuum tanker truck drawn in plain top-down outline, with a cab, a large
horizontal cylindrical vacuum tank and wheels as small rectangles, is shown
angled from the road toward the gate opening, part-way through a turn, its nose
pointing into the gate and its tail still on the road. One large thin ink arc,
drawn as a long shallow curve, sweeps from the outer front corner of the truck
across the road to show the path its front corner traces through the turn, with
a thin radius line and a plain arrowhead from the turn centre to the arc. Three
ink dimension lines with plain arrowheads at both ends are drawn: one across the
gate opening, one across the full width of the road, and one across the truck's
own body width; none carries any number, unit or text. There is no hose anywhere
in the drawing. Keep the plot outline, compound wall, gate position and house
footprint identical to its siblings. Ochre: the tanker truck. Everything else in
ink.
```

*At a glance:* the truck mid-turn into the gate, with the curve its nose sweeps
drawn out and the gate, road and truck all dimensioned.

---

**`truck_access-hose.png`**

```
Flat top-down site plan, orthographic from directly above, of a small
rectangular Indian residential plot roughly 30 by 40 feet. A public road runs as
a plain band along the entire bottom edge of the frame. The plot is enclosed by a
plastered compound wall drawn as a thick double line, with one gate opening in
the bottom wall, offset to the right, standing open. Inside, the house footprint
is a simple rectangle with thick hatched walls set back from the road, and a
small rectangle representing the underground septic tank sits in the open front
yard to the left of the gate. A desludging vacuum tanker truck drawn in plain
top-down outline, with a cab, a large horizontal cylindrical vacuum tank and
wheels as small rectangles, stands on the road outside the gate, parallel to the
compound wall. One large thin ink arc is drawn as a wide quarter circle centred
on the rear of the truck, sweeping up across the plot and passing beyond the
septic tank, with a thin straight radius line from the truck's rear to the arc
carrying a plain arrowhead at each end. The septic tank rectangle sits inside
the arc. A dashed ink line runs from the rear of the truck, in through the open
gate, and across the yard to the tank, following the open ground. Two ink
dimension lines with plain arrowheads at both ends are drawn: one across the
gate opening and one across the full width of the road; neither carries any
number, unit or text. Keep the plot outline, compound wall, gate position and
house footprint identical to its siblings. Ochre: the tanker truck. Everything
else in ink.
```

*At a glance:* a reach arc swung from the back of the truck, with the tank
sitting inside it and the hose route traced through the gate.

---

### 5.9 `sizing` — 3 images

**Locked camera:** identical frame, identical ground line, identical row of five
silhouettes at identical size above ground. Only the tank's proportions and
internal subdivision differ.
**Ochre:** the row of silhouettes and the tank's outer outline (present in all
three).

---

**`sizing-thumb.png`**

```
Flat technical drawing, orthographic, no perspective, combining a scale row
above ground with a cutaway section below. In the upper band of the frame, above
a ground line that runs the full width, a row of five identical plain featureless
solid dark silhouettes of standing adults stand evenly spaced on the yard
surface: no faces, no clothing detail, holding nothing, all exactly the same
height, all standing on the ground and well clear of anything else. Below the
ground line, light 15 percent ink earth hatching, and an underground household
septic tank drawn in cutaway section: coursed brick walls, a cast concrete floor
slab, a cast RCC cover slab at ground level with one square opening in it, a
flat liquid level line, and a sludge band on the floor. No internal dividing
walls and no other internal lines. One horizontal ink dimension line with plain
arrowheads at both ends spans the tank's internal width, and one vertical ink
dimension line with plain arrowheads at both ends spans its internal depth;
neither carries any number, unit or text. In this drawing the tank is a plain
simple box whose internal width is about twice its internal depth, and whose
depth is a little less than one silhouette's height, so it sits under roughly
the middle three silhouettes. Same frame, same ground line and same silhouette
size as its siblings. Ochre: the five silhouettes and the tank's outer outline.
Everything else in ink.
```

*At a glance:* a plain box of ordinary proportions under a household of five,
with nothing inside it worked out.

---

**`sizing-oversize.png`**

```
Flat technical drawing, orthographic, no perspective, combining a scale row
above ground with a cutaway section below. In the upper band of the frame, above
a ground line that runs the full width, a row of five identical plain featureless
solid dark silhouettes of standing adults stand evenly spaced on the yard
surface: no faces, no clothing detail, holding nothing, all exactly the same
height, all standing on the ground and well clear of anything else. Below the
ground line, light 15 percent ink earth hatching, and an underground household
septic tank drawn in cutaway section: coursed brick walls, a cast concrete floor
slab, a cast RCC cover slab at ground level with one square opening in it, a
flat liquid level line, and a sludge band on the floor. No internal dividing
walls and no other internal lines. One horizontal ink dimension line with plain
arrowheads at both ends spans the tank's internal width, and one vertical ink
dimension line with plain arrowheads at both ends spans its internal depth;
neither carries any number, unit or text. In this drawing the tank is much
larger than in its siblings and fills most of the space below the ground line:
its internal width is about three times one silhouette's height, spanning the
whole row of five, and its internal depth is about one and a half times a
silhouette's height. Same frame, same ground line and same silhouette size as
its siblings. Ochre: the five silhouettes and the tank's outer outline.
Everything else in ink.
```

*At a glance:* a tank far wider and deeper than the household row above it,
reaching well below one person's height.

---

**`sizing-std.png`**

```
Flat technical drawing, orthographic, no perspective, combining a scale row
above ground with a cutaway section below. In the upper band of the frame, above
a ground line that runs the full width, a row of five identical plain featureless
solid dark silhouettes of standing adults stand evenly spaced on the yard
surface: no faces, no clothing detail, holding nothing, all exactly the same
height, all standing on the ground and well clear of anything else. Below the
ground line, light 15 percent ink earth hatching, and an underground household
septic tank drawn in cutaway section: coursed brick walls, a cast concrete floor
slab, a cast RCC cover slab at ground level with one square opening in it, and a
sludge band on the floor. In this drawing the tank's internal width is about
twice its internal depth and its depth is a little less than one silhouette's
height, the same overall size as the plainer sibling. Its interior is divided
by two thin horizontal ink lines into three clearly distinct stacked bands: a
shallow empty band directly under the cover slab, a tall middle band holding the
liquid, and a shallow band at the floor holding the sludge. Beside the tank on
the left, inside the earth hatching, three small vertical bracket dimension
lines with plain arrowheads sit one above the other, each one spanning exactly
one of the three bands; a fourth vertical dimension line spans the tank's full
internal depth, and one horizontal dimension line spans its internal width. No
dimension line carries any number, unit or text. Same frame, same ground line
and same silhouette size as its siblings. Ochre: the five silhouettes and the
tank's outer outline. Everything else in ink.
```

*At a glance:* the same tank, but its depth is broken into three measured
layers — air, liquid, sludge — each one bracketed off.

---

## 6. How to check a generated image is usable

Run every image through all seven. Any failure means regenerate, not retouch.

1. **The 420 px test.** Shrink to 420 px wide and look at it on a phone at arm's
   length. Can you still name the configuration without reading the card text?
   If the difference from its sibling has vanished, the drawing is too detailed
   or the difference is too subtle. Fix by removing elements, not by adding
   emphasis.
2. **The sibling test.** Put all options of one decision side by side. Do the
   frame, scale, ground line, line weight and context match exactly? Is the only
   difference the configuration? If one sibling is zoomed differently, the set is
   unusable even if each image is good alone.
3. **The pre-judging test.** Hand all siblings to someone who has not played,
   with no text. Ask which one they think the tool recommends. If they can
   answer confidently and correctly, the set is biased and the research data is
   compromised. The right outcome is a shrug.
4. **The text test.** Zoom to 100 % and scan for any glyph, numeral, unit,
   watermark or signature, including inside hatching and along dimension lines.
   Reject on any hit. Partially-cropped text looks like damage and cannot be
   translated.
5. **The hard-rule test.** No figure inside, entering, leaning into, or standing
   on the rim of a tank. No hands, no arms, no heads above an opening. Any
   silhouette has both feet on firm ground, outside, holding at most a hose. Any
   violation is an outright reject regardless of quality.
6. **The dual-theme test.** View on a #FDFBF7 card and on a near-black card. The
   opaque warm panel should read as a pinned drawing in both, with the 1 px
   border visible and no element touching the panel edge. If the image looks
   like a hole punched in a dark screen, add internal padding.
7. **The context test.** Does it look like an Indian peri-urban site — brick,
   plaster, concrete rings, compound wall, bare earth? Reject moulded plastic
   tanks, lawns, timber houses and pickup trucks. Readers were testing whether
   outsiders can relate to this; getting the construction context wrong loses
   insiders instead.

---

## 7. Budget: if only ten

Honest ranking. **Need an image** means a picture resolves a genuine spatial
ambiguity that words cannot. **Decoration** means the text already does the job.

| Decision | Images | Verdict |
|---|---|---|
| `openings` | 3 | **Essential.** Its own reveal says "this is the decision", the three options are purely geometric, and `flush slab` was one of two terms that actually blocked comprehension. If one decision gets images, it is this one. |
| `effluent` | 3 | **Essential.** Directly named by a tester ("water-dispersal systems"), and all three option labels are jargon. Nobody picks between a soak pit and a dispersion trench from a label. |
| `tanktype` | 4 | **Essential.** Four constructions, four unfamiliar words, and the one decision whose cutaway genuinely cannot be written. `baffle wall` only makes sense as a picture. |
| `depth` | 3 | **Strong.** Pure proportion, and the life-safety core of the content. Weakened slightly because the hose must be drawn reaching in all three, so the image shows shape and leaves the consequence to text. |
| `vent` | 3 | **Strong.** Defines `parapet` and `cowl` and makes "full height" concrete. A reader who has never looked at a roof parapet cannot picture "hidden behind the parapet". |
| `location` | 3 | **Moderate.** The plan view is genuinely clarifying, but the labels are already plain Indian spatial language that readers picture unaided. |
| `sizing` | 3 | **Weak.** This is arithmetic. Three boxes of different proportions against five silhouettes tells a reader less than the sentence already does. A numeric comparison in the card text beats it. |
| `slab_call` | 3 | **Weak as new art.** Two of three options are the same object as `openings`, one moment earlier. The pour-day framing is dramatically right but informationally redundant. |
| `truck_access` | 3 | **Weakest.** An epistemic choice, not a spatial one. Options two and three are the same site with different knowledge, and three images invite the "more annotation is better" reading the brief forbids. |

**The ten I would build:** `openings` × 3, `effluent` × 3, `tanktype` × 4.
Reason: those are the three decisions where testers asked by name, and the three
whose option labels are unreadable without a picture. Ten images, three complete
sets, nothing half-covered.

**The dissent, stated plainly.** If the owner will trade, I would rather have
`openings` × 3 + `effluent` × 3 + `depth` × 3 + one single shared `tanktype`
comparison drawing (all four constructions as four small sections in one 3:2
frame) = **ten images, four decisions covered**. That gets depth — which is the
actual mechanism by which a hose job becomes a body job — into the visual set,
and `tanktype`'s four options are the ones most tolerant of being shown as a
small comparison grid rather than four full cards.

**What to do with the cuts, for free:**
- `slab_call`: reuse `openings-flush.png` and `openings-covers.png` on two of
  its three options. Generate only `slab_call-ask.png` if the budget stretches
  to eleven. Losing the pour-day framing costs some drama, costs no
  comprehension.
- `truck_access`: drop the per-option images and put **one** neutral diagram on
  the decision card itself — truck on the road, gate, turning arc, reach arc —
  with the numbers in a translatable caption. One image, no ranking risk.
- `sizing` and `location`: no images. Put the numbers in the card text.

---

## 8. Raster or SVG — the honest answer

**SVG is better for most of this set, and for several images it is not close.**

Everything in this pack is flat line work in four flat values with no
photographic content. That is SVG's native output and an image model's worst
case. Concretely:

- **Precision is the content.** `openings` turns on an opening being one quarter
  of the tank's width versus one eighth. `depth` turns on a ratio against a
  silhouette. An image model will approximate those ratios and the approximation
  is the thing the reader is supposed to compare. In SVG the ratio is a number
  you set once.
- **The locked camera is free.** Siblings sharing a frame, ground line, scale and
  line weight is a hard prompting problem and a trivial SVG problem — one base
  file, one `<g>` swapped per option. Most of Section 5's "same frame as its
  siblings" boilerplate exists only to fight the generator.
- **The no-text rule inverts.** The reason text is banned is that baked text
  cannot be translated. SVG `<text>` **can** be translated: swap the string per
  locale from the i18n bundle, or render the label as HTML positioned over the
  SVG. The constraint that forces these images to be wordless disappears, and
  with it the need for the proposed `visual_caption` field on exactly the
  decisions where a number matters.
- **Dark mode becomes trivial.** `stroke="currentColor"` and a CSS variable for
  the ochre. No opaque panel, no dual-theme compromise, no "drawing pinned to a
  card" workaround.
- **Weight and sharpness.** 28 PNGs at 840 × 560 is a few hundred KB of mobile
  data. The same set as SVG is a fraction of that, inlineable, and sharp at any
  density. This app is for phones on Indian mobile data.
- **Accessibility.** SVG takes `<title>`, `<desc>` and `role="img"` natively,
  and those strings go through the same translation pipeline as everything else.

**Where raster still earns its place:** `location`, `truck_access` and
`effluent-storm` carry context — a house footprint, a tanker truck, a street
drain — that is tedious to author by hand and where approximate geometry costs
nothing. Generate those.

**Recommended split:**

| Build as SVG | Build as raster |
|---|---|
| `openings` × 3, `slab_call` × 3, `depth` × 3, `vent` × 3, `sizing` × 3, `tanktype` × 4 — **19 images.** All are dimensioned, counted, or depend on exact ratios between siblings. | `location` × 3, `truck_access` × 3, `effluent` × 3 — **9 images.** Context-heavy, no load-bearing dimensions. |

**Practical route if the owner wants to use an image model anyway:** use it as a
look-development tool, not an asset pipeline. Generate `openings-covers.png`,
settle the line weight, hatch density and ochre balance, then author the 19
precision images as SVG against that reference. The prompts in Section 5 double
as SVG specifications — every ratio, every element and every ochre target is
already stated numerically.
