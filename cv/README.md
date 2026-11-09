# CV source

`CV_mio.tex` is the source of the CV the site serves at `/files/CV_mio.pdf`.
It replaces the old Word document: LaTeX keeps the CV in plain text, so it
diffs in git and an agent can edit it without touching a binary.

## Build

```
cd cv
xelatex CV_mio.tex        # twice, so the "page n of N" footer settles
cp CV_mio.pdf ../files/CV_mio.pdf
```

The CV is built on the Mac with MacTeX (`/Library/TeX/texbin/xelatex`).
XeLaTeX is required, not pdflatex: the file uses `fontspec` and Unicode
characters straight in the source (accents, curly quotes, €, ¿).

The typeface is JetBrains Mono (SIL Open Font License, release 2.304),
vendored in `fonts/` with its `OFL.txt`: Regular, Italic, Bold and BoldItalic
for the text, Medium for the labels. `fontspec` loads them with
`Path=fonts/`, so the build needs no installed fonts and runs from `cv/` on
any machine.

The folder is listed under `exclude:` in `_config.yml`, so Jekyll does not
publish the source. Only the copy in `files/` is served.

## Editing

Every entry is one `\cv{left column}{body}` call. The left column holds the
years or the label, the body the text. `\role{...}` sets the bold first line
of an entry, `\venue{...}` italicises a journal, book or conference name.
Add an entry by copying the line above it; nothing else has to change.

Keep the PDF and the `_publications/` collection in step. Whenever the site
gains a publication, the CV gains the same one.
