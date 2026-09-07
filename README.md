# Robert Fainter — Artwork Catalog

Web-preview image assets and catalog for the Robert Fainter portfolio site. **155 works across 9 series.**

> **© Robert Fainter. All rights reserved.** Images here are 800px web previews published for website development only — not licensed for reuse, redistribution, or model training. See [`COPYRIGHT.md`](COPYRIGHT.md). Full-resolution masters are not in this repository.

Read [`DESIGN_BRIEF.md`](DESIGN_BRIEF.md) first — it covers the work, the design direction, and the open questions.

## Layout

```
images/<series>/         800px long edge · ~70 KB each · 11 MB total
catalog.json             series metadata + per-work dimensions and orientation
catalog_manifest.csv     provenance — every file mapped to its source photograph
DESIGN_BRIEF.md          design brief
COPYRIGHT.md             usage terms
```

Series folders: `01_Sketches`, `02_Large_Oil_Paintings`, `03_Warrior_Series`, `04_Jockey_Series`, `05_Faces`, `06_Floating_Figures`, `07_Miscellaneous`, `08_Impressionist`, `09_Mystical_Figures`

## Using `catalog.json`

Build from the catalog rather than hardcoding filenames.

```js
const catalog = await fetch('catalog.json').then(r => r.json());

catalog.series.forEach(series => {
  console.log(series.name, series.count, series.description);
  series.works.forEach(w => {
    // w.id, w.file, w.width, w.height, w.orientation
    const src = `images/${series.folder}/${w.file}`;
  });
});
```

Every work carries `width`, `height` and `orientation` (`portrait` | `landscape` | `square`) so a masonry grid can reserve correct space before images load — no layout shift.

**Note:** `width`/`height` in `catalog.json` describe the 2400px master. The preview files in `images/` are the same aspect ratio scaled to 800px long edge. Use the values for ratio, not for pixel dimensions.

## Image URLs

Raw GitHub, for prototyping:

```
https://raw.githubusercontent.com/<owner>/<repo>/main/images/05_Faces/Fainter_Faces_001.jpg
```

jsDelivr CDN, for anything with real traffic — same paths, cached globally:

```
https://cdn.jsdelivr.net/gh/<owner>/<repo>@main/images/05_Faces/Fainter_Faces_001.jpg
```

## Image specs

sRGB JPEG, progressive, q82, EXIF orientation baked in and metadata stripped. Filenames are stable and safe to use as permanent identifiers.

Full-resolution masters (2400px long edge, ~88 MB) exist outside this repository and are wired in at build time for detail and lightbox views. `catalog_manifest.csv` maps every file back to its source photograph, including which sources were already under 2400px and left at native resolution.
