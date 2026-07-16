# CLAUDE.md — arturobertero.github.io

Context and working instructions for Claude Code. Read this fully before acting.

## What this repo is
Personal academic website for **Arturo Bertero** (postdoctoral quantitative social scientist,
University of Milan; LONELY-EU project; belief systems, loneliness, conspiracism, network methods).
It is a **Jekyll site built on the `academicpages` template** (a fork of Minimal Mistakes),
served by **GitHub Pages from the `master` branch, root folder**. There is no custom domain,
no CI workflow — pushing to `master` triggers the default Pages build (~1 min to go live).

## Ground truth: page → source-file map
- Homepage  → `_pages/about.md`
- Top menu  → `_data/navigation.yml`   (this file, NOT the pages, controls what visitors can click)
- Site title / author bio / social links / SEO → `_config.yml`
- Publications collection → `_publications/*.md` (renders at `/publications/`)
- Talks collection → `_talks/*.md` (renders at `/talks/`, currently the only real content besides About)
- Uploaded PDFs → `files/`  (e.g. the real CV is `files/CV_mio.pdf`)
- Images → `images/`

## Current known problems (fix these)
1. **Stale identity.** `_config.yml` and `_pages/about.md` still say "Ph.D. Student" / "Turin".
   He is now a **postdoctoral researcher at the University of Milan**, working on the Horizon Europe
   **LONELY-EU** project. Update `description`, `author.bio`, `author.location`, and the About intro.
2. **Placeholder cruft still in the repo** (unlinked but crawlable / in sitemap):
   - `_publications/2009-10-01-paper-title-number-1.md`, `-2`, `-3`  → DELETE
   - `_pages/cv.md` ("B.S. in GitHub, GitHub University") → DELETE (the CV link points to the PDF)
   - `_teaching/2014-spring-teaching-1.md`, `-2` → DELETE (Teaching is not in the menu)
   - `_portfolio/portfolio-1.md`, `portfolio-2.html` → DELETE
   - `_posts/prova1.md` and the nested `_posts/_posts/` directory → DELETE (test/lorem content + a structural bug)
3. **`repository:` in `_config.yml`** still points to `academicpages/academicpages.github.io`.
   Change to `arturobertero/arturobertero.github.io`.
4. **Oversized avatar.** `images/my_photo.jpeg` is ~16 MB, loaded on every page. Compress it (see task 5).
5. **SEO/social blank.** No `og_image`, no site `description` for previews. Set them.

## Tasks (do in this order; commit after each numbered task)

### Task 1 — Add real publications
- Copy the 5 files from `NEW_publications/` (provided alongside this file) into `_publications/`.
- Delete the 3 placeholder `paper-title-number-*.md` files.
- Some files contain `[verify: ...]` markers and HTML comments where a DOI/volume was unconfirmed.
  Do NOT invent values. Leave the marker, and print a summary listing every `[verify]` still open
  so Arturo can fill them.
- Commit: `feat: add real publications, remove placeholders`.

### Task 2 — Surface the publications page in the menu
- In `_data/navigation.yml`, the `Publications` item currently points to Google Scholar.
  Change its `url` to `/publications/` so visitors land on the on-site list.
- Keep a "Google Scholar" link available — either add it as a separate menu item, or ensure
  `author.googlescholar` is set in `_config.yml` (it is) so the publications page shows the Scholar link.
- Commit: `feat: point Publications menu to on-site list`.

### Task 3 — Fix identity + metadata in `_config.yml`
- `description`: replace the "Ph.D. Student…" string with a postdoc description.
- `author.bio`: "Postdoctoral researcher, University of Milan" (or Arturo's preferred wording — ASK if unsure).
- `author.location`: update from "Turin, Italy" (confirm current city with Arturo before changing — do not guess).
- `repository`: `arturobertero/arturobertero.github.io`.
- `og_image`: set to a compressed image in `/images/` (the avatar after Task 5, or a dedicated card).
- Commit: `fix: update identity and site metadata`.

### Task 4 — Rewrite the About intro (`_pages/about.md`)
- Update the opening line from "Ph.D. student at NASP" to his postdoctoral role.
- Keep his voice: concise, direct, first person, no hedging. Do NOT bloat it.
- Fix the typo "melts threee streams" → "blends three streams".
- Replace the fragile GitHub user-attachment image URL (`github.com/.../assets/...`) with a local
  file committed to `images/`. If you cannot access the original, leave a clearly-marked TODO — do not
  drop the figure silently.
- Commit: `content: update about page to postdoc`.

### Task 5 — Compress the avatar
- `images/my_photo.jpeg` (~16 MB) → target < 400 KB, longest edge ~1000px, quality ~82.
- Use ImageMagick if available: `convert images/my_photo.jpeg -resize 1000x1000\> -quality 82 images/my_photo.jpeg`
  (or `magick ...`). Verify the file size dropped and the image still renders. Keep the same filename
  so `_config.yml`'s `author.avatar` needs no change.
- Commit: `perf: compress avatar from 16MB`.

### Task 6 — Delete remaining cruft
- Remove the placeholder collections/pages listed under "Current known problems" item 2 (except anything
  already handled). Before deleting a `_pages/*.md`, grep `_data/navigation.yml` to confirm nothing links to it.
- Commit: `chore: remove template placeholder pages`.

## Guardrails
- **Never invent citations, DOIs, dates, or a location.** If a value is unknown, leave a `[verify]`/TODO and report it.
- Preserve Arturo's writing voice on any prose you touch. No AI-generic filler, no em-dash-heavy hedging.
- Confirm the site builds: run `bundle exec jekyll build` locally if the toolchain is present; otherwise
  do a `jekyll doctor`-style sanity check on YAML front matter (every `_publications/*.md` needs valid
  front matter or the build fails silently on Pages).
- One logical change per commit, with the message stubs above. Do not force-push.
- When done, print: (a) the list of open `[verify]` markers, (b) any TODO you left, (c) the new size of the avatar.

## Model guidance
- This file-surgery workload (Tasks 1–3, 5, 6) is mechanical — run it on **Claude Sonnet 5**; it's fast and sufficient.
- Task 4 (voice-sensitive prose) is better drafted with **Claude Opus 4.8**, then pasted in.
