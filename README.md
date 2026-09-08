# Quantum Hardware, Error Correction & Applications — Online Reader

A self-contained GitHub Pages site for **Quantum Hardware, Error Correction & Applications: Physical Qubits, Noise Mitigation & Quantum Computing** (Q.C. Series, Vol. III) by Dr. S. K. Jain.

This is **Volume III** of the same series as Volumes I and II. It uses the exact same fonts, box colors, heading styles, and reader UI as the other volumes, so all three feel like one consistent series.

## Structural differences from Volumes I & II (handled automatically)

Volume III's source document uses yet another chapter-banner convention, different again from both prior volumes:

- **Chapter banners use a "UNIT n | CHAPTER m" marker** (Heading 1, no colon or title on the same line), with the subtitle following as a separate paragraph. Neither Volume I's colon-joined format nor Volume II's bare "CHAPTER n" format applies here — the converter detects this book's specific marker text directly.
- **Section-heading styles are inconsistent between chapters**, the same underlying issue as Volume II: chapters 5 and 6 tag their major sections ("5.1", "6.1") as Heading 1, while every other chapter tags the equivalent sections as Heading 2. The same order-of-first-appearance normalization used for Volume II handles this automatically, plus a pattern override for recurring end-of-chapter headers (Solved Examples, MCQ Answers, Chapter Summary, References, Assignments) so they sit as peers to the numbered sections regardless of their raw style.
- **New pedagogical box icons** not seen in Volumes I/II — 📋 Learning Objectives, 💡 Tip, ▶ Definition/Theorem, ℹ Roadmap/Protocol, 📝 Problem, among others — are mapped onto the series' existing box types (Key Concept, Math, Real World, Solved Problem respectively) rather than inventing new colors, keeping the visual language consistent across all three books.
- **Multi-panel figures**: several figures in this book embed two images side by side under one caption (e.g. "Left: ... Right: ..."). These are laid out as a responsive row rather than stacked, matching how they read in the original.

## What's inside

- `index.html` — the reader shell (sidebar TOC, topbar controls, reading pane, on-page TOC)
- `assets/css/style.css` — dark/light theme (CSS variables, toggle persists via `localStorage`) — matched to Volumes I & II
- `assets/js/app.js` — chapter loading & routing, on-page TOC generation, read-aloud, visitor counter, like button
- `content/*.md` — the book itself, one Markdown file per chapter, generated from your `.docx` source
- `content/manifest.json` — the chapter list that drives the sidebar (edit titles/order here)

## Features

- **Dark / light theme** — toggle in the top bar, remembers your choice (light is default — it's the book's actual printed appearance)
- **Read aloud** — uses the browser's built-in Web Speech API. Play/pause, stop, and a speed selector (0.8×–1.75×). **Click any paragraph or heading to set it as the starting point** — a gold left-border marks the chosen spot until you pick a different one or navigate to another chapter. The paragraph being read is highlighted and auto-scrolled, advancing to the next chapter automatically.
- **Clickable navigation** — every chapter link, on-page TOC entry, and Prev/Next chapter button is deep-linkable
- **Filter box** in the sidebar to quickly jump to a chapter
- **Visitor counter** (sidebar footer) and **like button** (top bar, red heart)
- **More in this series** — links to Volumes I and II in the sidebar (see "Cross-linking the series" below)
- Responsive: collapses to a slide-out sidebar on mobile

## Cross-linking the series

The sidebar's "More in this series" section links to all five titles in the series — the other two textbook volumes plus both laboratory manuals. These now point to the real, live GitHub Pages URLs:

```js
const SERIES_LINKS = [
  { label: "Volume I — Quantum Computers (Textbook)", url: "https://skjaindr.github.io/Quantum-Computing.book-open-1/" },
  { label: "Volume II — Quantum Algorithms & Complexity (Textbook)", url: "https://skjaindr.github.io/Quantum-Computing.book-open-2/" },
  { label: "Volume III — Quantum Hardware, Error Correction & Applications", url: "https://skjaindr.github.io/Quantum-Computing.book-open-3" },
  { label: "Laboratory Manual I — Hands-on Qiskit Experiments", url: "https://skjaindr.github.io/Quantum-Computing.labmanual-open-1/" },
  { label: "Laboratory Manual II — Advanced Experiments - Security, Hardware Platforms and Applications", url: "https://skjaindr.github.io/Quantum-Computing.labmanual-open-2/" },
];
```

This list includes a link back to this same book (Volume III) — that's intentional per how the list was specified, not an oversight. If you'd rather this site omit a link to itself, remove that one entry from `SERIES_LINKS` in this file.

**Keep plain and protected sites cross-linked separately.** This is the plain (unprotected) version — every URL above ends in `-open-N` (or `labmanual-open-N`), i.e. the plain sites. The protected version of this book has its own `SERIES_LINKS` pointing to the corresponding `-N` (non-"open") protected URLs. Don't mix the two. If any of these repos hasn't been created/deployed yet, that particular link will simply 404 until it exists.

## Visitor counter & like button

Backed by [Abacus](https://abacus.jasoncameron.dev), a free counting API needing no signup or key. Volume III uses its own counter namespace (`qc-series-vol3-skjain`, set in `assets/js/app.js` as `COUNTER_NAMESPACE`) so its counts are tracked separately from the other volumes.

- **Visitor counter**: increments once per page load, shown in the sidebar footer.
- **Like button**: click once to like — turns solid red, count increments, remembered via `localStorage` so it can't be clicked repeatedly. No "unlike."

**Verify this actually works once deployed.** Built and tested in a sandboxed environment with no outbound internet access, so the graceful-failure path was verified but the live API calls were not. Click the like button on your deployed site and refresh to confirm the count persists.

## Publishing to GitHub Pages

1. Create a new GitHub repository (e.g. `quantum-hardware-book-site`, alongside your Volume I and II repos).
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

Opening `index.html` directly by double-clicking it will **not** work — browsers block `fetch()` of local files. Serve the folder instead:

```bash
python3 -m http.server 8000
# then open http://localhost:8000
```

## About the text conversion — please proofread

Converted from your `.docx` using the series pipeline, adapted for this book's specific structure (see above). Four real conversion bugs were found and fixed during this build, all verified:

1. A "Table Index" skip-heading in the front matter was leaving a skip-flag stuck on, which silently deleted all of Chapter 1's real content until the next incidental heading. Fixed — Chapter 1 now has its full ~90,000 characters of content.
2. Figures with two side-by-side image panels under one caption were only keeping the first panel. Fixed to capture all panels in a figure box.
3. A handful of figures were nested inside a container table that also had its own direct content (a formula), and only the outer content was rendering. Fixed to render nested table content too.
4. The cover image was tagged with a heading style in the source despite containing only an image — this exact bug also hit Volume II's dedication photo, and I'd fixed it there but the fix didn't carry over when this converter was rebuilt for Volume III's different structure. Now fixed and verified.

All 83 images in this book are accounted for — zero missing, zero orphaned, confirmed by scanning every chapter's image references against the extracted image files. Figures compressed from 13.7MB to 4.9MB with no visible quality loss.

A few small things worth knowing:

- **Learning Objectives boxes**: in Chapter 1 only, this appears as plain bold text rather than a styled box — the source used a table for this box in every other chapter but bold text in Chapter 1. Content is complete either way, just not boxed in that one instance.
- A small number of single-cell tables (e.g. a lone table just containing the word "Examples:") render as near-empty boxes — this is genuinely how the source document has them, not lost content.

None of this is destructive — the `.md` files are plain text you can hand-edit directly.
