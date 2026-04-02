# Pretext Demo Suite for The Guardian

Five self-contained HTML demos showcasing [Pretext](https://github.com/chenglou/pretext) — a JS library that measures and lays out multiline text without triggering DOM reflows.

## How to Run

Each demo is a single HTML file. You need a local server because they use ES module imports:

```bash
cd "/Users/cjmoran/Desktop/Claude projects/Pretext demos"
python3 -m http.server 8765
```

Then open any demo at `http://localhost:8765/`:

- [Demo 1: Pull Quote Balancer](http://localhost:8765/demo-1-pull-quote-balancer.html)
- [Demo 2: Fluid Cutout](http://localhost:8765/demo-2-fluid-cutout.html)
- [Demo 3: Live Blog Scroller](http://localhost:8765/demo-3-zero-jank-live-blog.html)
- [Demo 4: Canvas Data Journalism](http://localhost:8765/demo-4-canvas-data-journalism.html)
- [Demo 5: Stress Test](http://localhost:8765/demo-5-stress-test.html)

## The Demos

### Demo 1: Print-Perfect Pull Quote Balancer
A Guardian article with a pull quote floated inside the body copy. The article text reflows around the quote in real-time as you drag the width slider. Toggle between CSS mode (fixed `max-width` with red waste hatching) and Pretext mode (binary-search shrink-wrap — the quote tightens and the body copy gains breathing room). Metrics show pixel waste, space saved, and layout time.

**Pretext APIs:** `prepareWithSegments()`, `layout()`, `layoutWithLines()`, `layoutNextLine()`

### Demo 2: Fluid Cutout Article Layout
A long-read article with a draggable silhouette cutout. Text reflows around it in real-time — each line gets a different available width depending on where the image overlaps. Lines are individually positioned `<span>` elements. Sliders control image size and margin.

**Pretext APIs:** `prepareWithSegments()`, `layoutNextLine()` (variable widths per line)

### Demo 3: "Breaking Wire" Live Blog
An immersive dark-themed live blog (Middle East crisis scenario). New updates arrive with a three-phase animation: (1) a slot opens to the exact pre-measured pixel height with a shimmer gradient, (2) the headline types in character-by-character, (3) the body text fades in. BREAKING updates trigger a red ribbon. Updates tagged by importance (BREAKING/UPDATE/ANALYSIS). Zero-jank scroll compensation when reading older entries. Speed controls, "Burst 20" button, "Show heights" toggle, and "How it works" panel explaining the Pretext APIs.

**Pretext APIs:** `prepare()`, `layout()`, `prepareWithSegments()`, `layoutWithLines()`

### Demo 4: Canvas Data Journalism
Full-screen Canvas climate visualization (temperature anomaly 1880-2025) with animated particles. Hover annotated data points to see Pretext-wrapped callout text rendered directly on Canvas — no DOM overlays. Callouts adapt width near screen edges.

**Pretext APIs:** `prepareWithSegments()`, `layoutWithLines()`

### Demo 5: The 500-Block Stress Test
A grid of 500 Guardian article cards. Drag the width slider to trigger recalculation across every card. Left panel: DOM reads `offsetHeight` per card (layout thrashing). Right panel: Pretext calls `layout()` using cached `prepare()` results. Live timing shows the speedup (typically 100-200x). Card count toggle: 100/250/500/1000.

**Pretext APIs:** `prepare()`, `layout()`

## Design

All demos use Guardian-authentic styling: Georgia for body text, Guardian blue (#052962), accent yellow (#FFE500), news red (#C70000), and 1300px max-width layouts with pillar top-borders.

Each file imports Pretext from `https://esm.sh/@chenglou/pretext@0.0.3` and waits for `document.fonts.ready` before measuring.
