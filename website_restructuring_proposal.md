# Restructuring proposal — arturobertero.github.io

## The actual problem

The site isn't dead because it looks bad. It's dead because **updating it costs too much per action.**
academicpages spreads content across many collections (`_publications`, `_talks`, `_teaching`,
`_portfolio`, `_posts`), each with its own file-naming rule and front-matter schema, and it expects a
**local Ruby/Jekyll build to preview**. For someone who publishes a few times a year, that friction is
fatal: by the time you remember the workflow, you'd rather not. Every design decision below optimises for
one metric: **time-to-add-a-paper**, ideally under two minutes, with a preview you can trust.

Two levers fix this:
1. **Single source of truth** for publications — one file you append to, not one file per paper in a folder you have to remember exists.
2. **No local build** — edit and preview in the browser (or in your own daily toolchain), never `bundle install` again.

---

## Three paths, ranked

### Path A — De-friction the current site (½–1 day, lowest risk)
Keep academicpages, keep your URLs and talks, just remove the friction.

- **Publications from one file.** Replace the `_publications/*.md`-per-paper pattern with a single
  `_data/publications.yml` (or a `.bib` + `jekyll-scholar`) that a short Liquid loop renders on `/publications/`.
  Adding a paper = paste a 6-line YAML block at the top. No new file, no date-slug naming, no permalink.
- **Prune collections.** Delete `_teaching`, `_portfolio`, `_posts` scaffolding you don't use. Fewer concepts = lower cognitive load.
- **Kill the local build.** Edit via github.com's web editor; GitHub Pages builds on push. For preview, enable
  a PR + the built-in Pages preview, or just accept push-to-see (build is ~60s).
- **Verdict:** fastest to reach, but you're still on a template thousands of academics use, and the graphics ceiling is low.

### Path B — Migrate to **al-folio** (2–3 days, RECOMMENDED for effort-to-payoff)
al-folio is the de-facto "nice academic site" Jekyll theme. Still GitHub Pages, still free.

- **Publications are a single `.bib` file.** You already export BibTeX from Zotero/Scholar. Drop one entry in
  `_bibliography/papers.bib` and it renders as a card with abstract toggle, PDF/code/DOI buttons, and
  altmetric/citation badges. *This is the single biggest update-friction win available.*
- **Graphics are genuinely good out of the box:** clean type, responsive publication cards, dark mode,
  automatic social/preview cards, a real projects/CV layout.
- **CV** can be a single `.yml` or the linked PDF — your choice.
- **Cost:** a real migration (re-point content, set config, move the 8 PDFs and talks). But it's a well-trodden
  path with copious docs and example repos to copy from.
- **Verdict:** best ratio of "looks distinctive + trivial to update" to effort. This is my default recommendation.

### Path C — Rebuild as a **Quarto website** (2–4 days, most tailored to *you*)
You live in R/Quarto — every skill in your workflow is `.qmd`. A Quarto site means the website runs on your
daily toolchain, not a Ruby stack you touch once a year.

- **Publications** from a `.bib` via a Quarto listing, or a `publications.qmd` you append to.
- **Update loop is `quarto render` + push** — the exact muscle memory you already have from your analyses.
  You can preview live with `quarto preview` in RStudio. Zero new tooling.
- **You can embed real analysis**: a network plot, an interactive `ggiraph`/`plotly` figure, a reproducible
  demo of CCA/EGA right on the page. Nobody else's academic site can do this as naturally as yours could.
- **Cost:** the most rebuild, and GitHub Pages needs a tiny Actions workflow to render Quarto (well documented).
- **Verdict:** if you want the site to be *yours* and effectively free to maintain forever, this is it.
  If you'd rather not rebuild from scratch, take Path B.

---

## Recommendation

- **If you want it fixed this week:** Path A now, and it's already scoped in `CLAUDE.md`.
- **If you'll invest one weekend for a site you'll actually keep updated:** **Path B (al-folio)**.
- **If you want the site to live in your R/Quarto world and never fight Ruby again:** Path C.

My pick given your profile: **B for the graphics/community, C if you value toolchain fit over migration cost.**
Both make "add a paper" a one-file, sub-two-minute action, which is the whole point.

---

## The update workflow you're buying (Path B/C)

Adding a new paper, start to live:
1. Copy the BibTeX entry (Zotero → right-click → Export, or the "Cite" button on the journal page).
2. Paste into `papers.bib` (B) or `publications.qmd` (C). Add `pdf=`, `code=`, `abstract=` fields if you want buttons.
3. `git commit -am "add paper"` → `git push` (B), or `quarto render && git push` (C).
4. Live in ~1 minute. No date-slug filename, no permalink, no per-file front matter.

---

## Graphics upgrades (apply on any path)

- **Typography with a point of view.** Pair a characterful display face (e.g. Fraunces, Newsreader, or a
  grotesk like Space Grotesk) with a clean body (Inter, Source Serif). Set an explicit type scale. This alone
  moves you off "default template".
- **A hero that states your thesis**, not a stock bio line: one sentence on belief systems as networks, over a
  faint network-graph motif drawn from your own data. Your research *is* the visual.
- **Publication cards**, not a bulleted list: title, venue chip, year, and inline buttons (PDF · Code · DOI).
- **Dark mode + social preview cards** (both free in al-folio; a few lines in Quarto).
- **Restraint:** one signature element (the network motif), everything else quiet.

---

## Suggested sequence

1. **This week:** run the `CLAUDE.md` tasks (Path A). The site is correct and current — stop the bleeding.
2. **Decide B vs C** (I can scaffold either as a fresh repo/branch so your live site stays up during the build).
3. **Migrate content once**, cut over, retire the old structure.

Tell me B or C and I'll generate the starter repo, the `papers.bib` seeded with your 5 papers, and the config.
