# SmartHomeSellout Classes

Free, hands-on classes by **Brandon Harvey** (SmartHomeSellout), first taught at the [Dallas Makerspace](https://dallasmakerspace.org). Read them online, download print versions, or fork this repo and teach your own.

**Read online: [classes.smarthomesellout.com](https://classes.smarthomesellout.com/)**

## The classes

| Class | Status | Materials |
| --- | --- | --- |
| [Intro to Home Assistant](https://classes.smarthomesellout.com/intro-to-home-assistant/) | Live | [Speaker outline](docs/intro-to-home-assistant/outline.md) · [Attendee handout](docs/intro-to-home-assistant/handout.md) |
| ESPHome | Planned | |
| Low-Voltage LED Lighting with WLED | Planned | |
| Soldering | Planned | |

## Downloads

Every class has a rolling release with its current docx and PDF builds, refreshed automatically on every change. For Intro to Home Assistant: [the `intro-ha` release](https://github.com/bharvey88/classes/releases/tag/intro-ha).

## How it works

Each class is a folder of plain Markdown under `docs/`. Two GitHub Actions workflows run on every push to `main`:

- **Website** (`.github/workflows/pages.yml`): builds the Markdown into an [MkDocs Material](https://squidfunk.github.io/mkdocs-material/) site and deploys it to GitHub Pages.
- **Documents** (`.github/workflows/build.yml`): converts each class's outline and handout to docx (via [pandoc](https://pandoc.org)) and PDF (via LibreOffice), then refreshes that class's rolling release.

### Build locally (optional)

```bash
# Website
python3 -m venv .venv
.venv/bin/pip install "mkdocs-material==9.*" mkdocs-redirects
.venv/bin/mkdocs serve   # preview at http://127.0.0.1:8000

# Documents (needs pandoc and libreoffice)
pandoc docs/intro-to-home-assistant/outline.md -o Intro-to-HA-Speaker-Outline.docx
soffice --headless --convert-to pdf Intro-to-HA-Speaker-Outline.docx
```

## Teach your own

1. **Fork** this repo.
2. Edit a class's Markdown (or copy a class folder to start a new one), swapping in your own gear, stories, and demos.
3. Push, and the site and documents rebuild for you.

One rule: outline and handout files stay plain Markdown (no HTML, no theme-specific syntax) because the same files feed the docx/PDF builds.

## License

Content is licensed under [Creative Commons Attribution 4.0 (CC BY 4.0)](LICENSE): use it, remix it, teach with it. Just keep an attribution back to this project.
