# Intro to Home Assistant

*Smart Homes Done the Right Way*

A **Home Assistant** class built for complete beginners and first taught at the [Dallas Makerspace](https://dallasmakerspace.org). Everything is here so you can **follow along, or fork it and teach your own version** at your local makerspace, library, or hackerspace.

## Read it online

**→ [classes.smarthomesellout.com](https://classes.smarthomesellout.com/)** — a clean, searchable site with clickable links (no download needed).

## What's in here

| File | What it is |
| --- | --- |
| [`docs/outline.md`](docs/outline.md) | The full **speaker cue outline** — every block, timing, and talking-point cue for presenting the class. |
| [`docs/handout.md`](docs/handout.md) | The **attendee take-home guide** — gear picks, protocol cheat-sheet, links, and ideas. |

Both are plain Markdown. Edit them and the **website**, plus the formatted **`.docx` and `.pdf`** versions, all rebuild automatically (see below).

## Download the printable versions

Grab the latest Word and PDF builds from the [**Releases**](../../releases/latest) page:

- `Speaker-Cue-Outline.docx` / `.pdf`
- `HA-Attendee-Handout.docx` / `.pdf`

## How the build works

Two GitHub Actions workflows run on every push:

- **[Website](.github/workflows/pages.yml)** — builds the Markdown into a [MkDocs Material](https://squidfunk.github.io/mkdocs-material/) site and deploys it to GitHub Pages.
- **[Documents](.github/workflows/build.yml)** — converts the Markdown to `.docx` (via [pandoc](https://pandoc.org)) and then to `.pdf` (via LibreOffice). Tag a release and the four files are attached to it automatically:

```bash
git tag v1.0.0
git push origin v1.0.0
```

You can also download the build artifacts from any workflow run under the **Actions** tab.

### Build locally (optional)

If you have `pandoc` and `libreoffice` installed:

```bash
# Documents
pandoc docs/outline.md -o Speaker-Cue-Outline.docx
soffice --headless --convert-to pdf Speaker-Cue-Outline.docx
pandoc docs/handout.md -o HA-Attendee-Handout.docx
soffice --headless --convert-to pdf HA-Attendee-Handout.docx

# Website (needs: pip install mkdocs-material)
mkdocs serve   # preview at http://127.0.0.1:8000
```

## Make it your own

1. **Fork** this repo.
2. Edit `docs/outline.md` and `docs/handout.md` — swap in your own gear, stories, and demos.
3. Push (or tag a release) and the site + documents rebuild for you.

## License

Content is licensed under [Creative Commons Attribution 4.0 (CC BY 4.0)](LICENSE) — use it, remix it, teach with it. Just keep an attribution back to this project.
