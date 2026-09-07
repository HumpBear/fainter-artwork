# Robert Fainter — Website Design Brief

## What this is

A portfolio site for the paintings, drawings and studies of **Robert Fainter**. 155 works across 9 series, photographed and prepared for web.

This repo is the asset source. Every image exists at two sizes and is indexed in `catalog.json`.

> **Fill in before building:** artist bio, dates/period of the work, medium and dimensions per piece, whether work is for sale, and contact/inquiry details. None of that is known from the files alone — everything below was read off the images themselves.

---

## Purpose — decided

**Portfolio now, sales later.**

Build it as a portfolio: series-first browsing, generous whitespace, no prices and no cart in v1. The emphasis is the body of work, not the transaction.

But structure it so commerce drops in without a rebuild. Concretely:

- **Every piece gets its own real URL** (`/work/:series/:id`) from day one — not just a lightbox. A lightbox-only gallery has nothing to attach a price, a description, or a share link to, and retrofitting URLs later means breaking every link that already exists.
- **Per-piece pages carry a metadata block** — medium, dimensions, year, status — rendered from the catalog. Ship it with the fields empty or hidden; the slot exists, so adding `price` and `available` later is a data change, not a layout change.
- **Model the data as if pieces were sellable.** Each work already has a stable ID. Add `title`, `year`, `medium`, `dimensions`, `status` to `catalog.json` as they become known.
- **An inquiry path, not a checkout.** A per-piece "Inquire about this work" that pre-fills the piece ID and emails you. This is how most fine art actually sells, it needs no payment infrastructure, and it's the natural seam where a real cart goes later.
- **Don't build**: cart, checkout, payment, inventory, shipping. Premature and they'd shape the design wrongly.

---

## The work

Nine series. Descriptions below are read from the images, not from the artist — correct anything that's wrong.

### 01 · Sketches (14)
Line and graphite studies. Reclining figures and nudes, plus a run of animal studies — fox, tapir, camel, gazelle, primate. Spare, fast, observational. Mostly landscape orientation.

### 02 · Large Oil Paintings (8)
Figurative oils on flat geometric grounds. Saturated skin tones set against blocked color fields — tiled blues, hard horizontals. Single figures and paired portraits. Several are near-square.

### 03 · Warrior Series (14)
The most kinetic work here. Fractured, high-energy figures in motion, limbs breaking into ribbon and plane. Cubist-adjacent, heavily saturated.

### 04 · Jockey Series (6)
Jockeys in silks — solo portraits and grouped figures. Flatter color, heraldic palette. A quieter cousin to the Warrior work; the two series share DNA and could be presented adjacently.

### 05 · Faces (29)
The largest body of work. Portrait heads built from loose line and wash, high chroma, expression pushed well past likeness. Reads as a sustained sequence rather than isolated pieces — worth presenting as a run.

### 06 · Floating Figures (40)
The largest series by count and the most conceptual. Figures suspended in minimal architectural space — doorways, blank walls, hard-edged voids. Largely grayscale with sparse color accents. Restrained, and tonally the opposite of Warrior/Faces.

### 07 · Miscellaneous (10)
Uncategorized. Interiors, a musician, beach scenes, portraits. Range rather than a through-line.

### 08 · Impressionist (17)
Atmospheric landscapes and figure groups. Layered washes, dissolved edges, weather and light doing the work.

### 09 · Mystical Figures (17)
Graphite and charcoal figure groups. Hats, ambiguous gatherings, narrative deliberately left open.

---

## Design direction

**The work is loud. The site should not be.**

The collection spans near-monochrome graphite (Floating Figures, Mystical Figures) and full-saturation color (Warrior, Faces, Jockey). A site with its own strong palette will fight half the collection. Recommendation:

- Near-neutral ground — warm off-white or a deep charcoal, committed to consistently. Not both.
- One restrained accent for interactive states only. Let each painting supply the color on its own page.
- Typography carries the personality: a serif with real character for titles, a clean neutral sans for everything else.
- Generous margins. Art needs air; a tight grid makes a portfolio look like a stock photo site.

**Mixed orientation is the central layout problem.** 70 portrait, 82 landscape, 3 square, unevenly distributed — Floating Figures is 65% portrait, Mystical Figures is 71% landscape. A fixed-aspect grid will crop badly or letterbox badly. Use a masonry or natural-aspect grid; `catalog.json` carries `width`, `height`, and `orientation` for every work so layout can be computed rather than guessed.

**Don't invent titles.** Files are sequence-numbered (`Fainter_Faces_007`), not titled. Either display sequence numbers honestly, show nothing, or get real titles from the artist. Fabricated titles on an artist's site are worse than no titles.

---

## Structure

```
/                    Landing — a single strong image, artist name, entry to the work
/work                All nine series as cards
/work/:series        One series, full grid
/work/:series/:id    Single piece, large, with prev/next within the series
/about               Bio, statement, portrait
/contact             Inquiry form
```

Per-piece URLs are required, not optional — see Purpose. A lightbox may sit on top of the grid for fast browsing, but it must deep-link to `/work/:series/:id` rather than replace it.

---

## Assets

| | |
|---|---|
| **Preview** | `images/<folder>/<filename>` — 800px long edge, ~70 KB. In this repo. Use for all grids and thumbnails. |
| **Full** | 2400px masters, ~88 MB — **not in this repo**, held privately and wired in at build time for detail/lightbox views. |
| **Index** | `catalog.json` — series, descriptions, per-work dimensions and orientation. Build from this, don't hardcode filenames. |
| **Provenance** | `catalog_manifest.csv` — maps every output file back to its source photograph. |

All images are sRGB JPEG with EXIF orientation already applied. Filenames are stable; treat them as permanent IDs.

**Performance:** the preview set is ~11 MB total for all 155 works and is what makes the site feel fast. Never load full-resolution masters into a grid.

**Copyright:** all artwork is © Robert Fainter, all rights reserved. See `COPYRIGHT.md`. Any prototype or published site must carry a visible copyright line.

---

## Known gaps

- **Faces 001–017 are low resolution** (~1100–1450px) — noticeably softer than the rest. They're AI-processed versions, not photographs of the paintings. If originals exist, re-shoot.
- **Large Oil Paintings are all ~2048px square exports**, not full-resolution originals. Fine at current display sizes, no room to zoom.
- **Mystical Figures 009–017** read as pencil figure studies — arguably belong in Sketches. Categorization is the artist's call.
- **Miscellaneous 010** appears to be a motion-blurred capture. Consider re-shooting or dropping.
- **No titles, dates, media, or dimensions exist** for any piece. This is the biggest content gap.
