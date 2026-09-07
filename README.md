# Quantum Hardware, Error Correction & Applications — Online Reader

A self-contained GitHub Pages site for **Quantum Hardware, Error Correction & Applications: Physical Qubits, Noise Mitigation & Quantum Computing Ecosystem** (Q.C. Series, Vol. III) by Dr. S. K. Jain.

This is **Volume III** of the same series as `quantum-computers-book-site` (Volume I) and `quantum-algorithms-book2-site` (Volume II). It deliberately uses the exact same fonts, box colors, heading styles, and reader UI as the earlier volumes, so all three feel like one consistent series.

## Design note: matched to the series on purpose

Volume III's source `.docx` uses its own color palette for pedagogical boxes (sampled directly from the document's table shading), which differs from Volumes I/II. **This site intentionally overrides those with the series' established colors and typography** rather than keeping Volume III's own, so the three books read as one series. If you'd ever prefer Volume III to use its own native palette instead, that's a small, isolated change (the color values are all in `assets/css/style.css`).

## New box types in this volume

Volume III's source uses a richer set of pedagogical boxes than Volumes I/II — 9 types instead of 6, plus inline Examples. Five reuse existing series classes:

- **Key Concept**, **Anecdote/Historical**, **Real World**, **Warning**, **Example** — same classes and colors as Volumes I/II
- **Mathematics** boxes (step-by-step derivations) now use the `box-math` class, previously defined but unused in Volume II
- Short highlighted equation call-outs use `box-equation` (previously defined but unused in Volume II)

Four are genuinely new, added to `assets/css/style.css` with colors chosen to stay harmonious with the existing palette:

- `box-learning-objectives` (blue-grey) — per-chapter learning objectives
- `box-definition` (indigo) — formal Definition/Theorem statements
- `box-tip` (amber) — practical tips
- `box-roadmap` (brown) — short orientation/roadmap notes, and the "RECAP" wayfinding notes that lead into each chapter's practice section

## Structural differences from Volumes I/II (handled automatically)

- **Chapter banners are table-based**, not a heading style: each chapter opens with a table whose first cell contains a literal `UNIT n | CHAPTER n` heading, the chapter title, and a keyword subtitle — all parsed out and re-emitted as `# CHAPTER n`, `# Title`, and an italic `*Unit n · keywords*` line.
- **Ten chapters grouped into five units.** Unit 1 has its own intro banner (lecture hours, figure/example/MCQ counts) rendered as a `box-roadmap` box at the very top of Chapter 1; Units 2–5 don't carry a separate banner in the source, so their chapters only show the "Unit n · keywords" subtitle line — this is a genuine asymmetry in the source document, not a conversion bug.
- **Real equations, not typed Unicode.** Unlike Volumes I/II (which typed math directly as Unicode text), this book's ~230 equations are genuine Word equation objects. They're converted to the same plain-Unicode style via a LaTeX→text pass, since no MathJax/KaTeX is loaded on this site (matching the rest of the series).
- **Figures carry real captions.** Volumes I/II's `<figcaption>` elements were left empty; this book's docx has genuine "Figure n.n: ..." captions, so `<figcaption>` is populated here.

## What's inside

- `index.html` — the reader shell (sidebar TOC, topbar controls, reading pane, on-page TOC)
- `assets/css/style.css` — dark/light theme (CSS variables, toggle persists via `localStorage`) — matched to the series, plus 4 new box classes
- `assets/js/app.js` — chapter loading & routing, on-page TOC generation, read-aloud, visitor counter, like button
- `content/*.md` — the book itself, one Markdown file per chapter, generated from your `.docx` source
- `content/manifest.json` — the chapter list that drives the sidebar (edit titles/order here)

## Features

- **Dark / light theme** — toggle in the top bar, remembers your choice
- **Read aloud** — Web Speech API, play/pause/stop, speed selector, auto-advances chapters
- **Clickable navigation** — every chapter, subsection, and Prev/Next button is deep-linkable
- **Filter box** in the sidebar to quickly jump to a chapter
- **Visitor counter** (sidebar footer) and **like button** (top bar, red heart)
- Responsive: collapses to a slide-out sidebar on mobile

## Cross-linking the series

The sidebar's "More in this series" links to Lab Manual I, Volume I, and Volume II (the `SERIES_LINKS` constant near the top of the counter/like-button section in `assets/js/app.js`). If you add more volumes later, add more entries to this same array.

```js
const SERIES_LINKS = [
  { label: "Laboratory Manual I — Hands-on Qiskit Experiments", url: "https://skjaindr.github.io/Quantum-Computing.labmanual-open-1/" },
  { label: "Volume I — Quantum Computers", url: "https://skjaindr.github.io/Quantum-Computing.book-open-1" },
  { label: "Volume II — Quantum Algorithms & Complexity", url: "https://skjaindr.github.io/Quantum-Computing.book-open-2" },
];
```

**Remember to also update Volume I's and Lab Manual I's own `SERIES_LINKS`** to add this book, so cross-linking works in both directions:

```js
{ label: "Volume III — Quantum Hardware, Error Correction & Applications", url: "https://skjaindr.github.io/Quantum-Computing.book-open-3" },
```

## Visitor counter & like button

Backed by [Abacus](https://abacus.jasoncameron.dev), a free counting API needing no signup or key. Volume III uses its own counter namespace (`qc-series-vol3-skjain`, set in `assets/js/app.js` as `COUNTER_NAMESPACE`) so its visitor/like counts are tracked independently from every other book/manual in the series — none of them share a total.

- **Visitor counter**: increments once per page load, shown in the sidebar footer.
- **Like button**: click once to like — it turns solid red and the count increments, remembered via `localStorage` so it can't be clicked repeatedly.

**Verify this actually works once deployed** — click the like button on your deployed site and refresh to confirm the count persists.

## Publishing to GitHub Pages

1. Create a new GitHub repository (e.g. `Quantum-Computing.book-open-3`, to sit alongside your Volume I/II and Lab Manual repos).
2. Copy everything in this folder into the repo root and push:
   ```bash
   git init
   git add .
   git commit -m "Quantum Hardware, Error Correction & Applications — online reader"
   git branch -M main
   git remote add origin https://github.com/<your-username>/<repo-name>.git
   git push -u origin main
   ```
3. In the repo on GitHub: **Settings → Pages → Source → Deploy from a branch → `main` / `(root)`** → Save.
4. Your book will be live at `https://<your-username>.github.io/<repo-name>/` within a minute or two.

### Testing locally before you push

```bash
python3 -m http.server 8000
# then open http://localhost:8000
```

## About the text conversion — please proofread

Converted from your `.docx` using the same pipeline built for Volumes I/II, adapted for this book's table-based chapter banners and real equation objects: 75 figures extracted and placed with their original captions, 9 pedagogical box types detected from the document's table structure and mapped onto the series' box vocabulary (4 new classes added), ~230 equations converted from native Word math to plain-Unicode text, and genuine data tables (comparison tables, MCQ answer keys, the Figure/Table/Symbol indexes) rendered as real HTML tables.

A few things worth a skim:

- **Front-matter Table of Contents** was rebuilt from the document's auto-generated TOC field, filtered down to chapter + two-level-section entries (matching the granularity of Volumes I/II) — the original TOC includes every three-level subsection and isn't reproduced in full, but the sidebar and on-page TOC still expose every heading.
- **The A–Z glossary** (back matter) is dense and formula-heavy in this book; each letter's entries are rendered inside an equation-style box rather than as plain list items, since most entries are short formula summaries rather than prose.
- **One cover-page line reads "*First Edition*"** — the source docx literally just contains the single word "First" at that spot (likely a truncated "First Edition, 2026" in the original draft); rendered as-is rather than guessing at the missing word.
- **Units 2–5 don't get an intro banner box** the way Unit 1 does — see the structural-differences note above.

None of this is destructive — the `.md` files are plain text you can hand-edit directly, same as the rest of the series.
