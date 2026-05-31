# Smart Homes Done the Right Way

An introduction-to-**Home Assistant** class built for complete beginners and first taught at the [Dallas Makerspace](https://dallasmakerspace.org). Everything is here so you can **follow along, or fork it and teach your own version** at your local makerspace, library, or hackerspace.

## What's in here

| File | What it is |
| --- | --- |
| [`outline.md`](outline.md) | The full **speaker cue outline** — every block, timing, and talking-point cue for presenting the class. |
| [`handout.md`](handout.md) | The **attendee take-home guide** — gear picks, protocol cheat-sheet, links, and ideas. |

Both are plain Markdown, so you can read them right here on GitHub. Edit the Markdown and the formatted **`.docx` and `.pdf`** versions are built automatically (see below).

## Download the printable versions

Grab the latest Word and PDF builds from the [**Releases**](../../releases/latest) page:

- `Speaker-Cue-Outline.docx` / `.pdf`
- `HA-Attendee-Handout.docx` / `.pdf`

## How the build works

A [GitHub Actions workflow](.github/workflows/build.yml) converts the Markdown to `.docx` (via [pandoc](https://pandoc.org)) and then to `.pdf` (via LibreOffice) on every push. Tag a release and the four files are attached to it automatically:

```bash
git tag v1.0.0
git push origin v1.0.0
```

You can also download the build artifacts from any workflow run under the **Actions** tab.

### Build locally (optional)

If you have `pandoc` and `libreoffice` installed:

```bash
pandoc outline.md -o Speaker-Cue-Outline.docx
soffice --headless --convert-to pdf Speaker-Cue-Outline.docx
pandoc handout.md -o HA-Attendee-Handout.docx
soffice --headless --convert-to pdf HA-Attendee-Handout.docx
```

## Make it your own

1. **Fork** this repo.
2. Edit `outline.md` and `handout.md` — swap in your own gear, stories, and demos.
3. Push (or tag a release) and the build runs for you.

## License

Content is licensed under [Creative Commons Attribution 4.0 (CC BY 4.0)](LICENSE) — use it, remix it, teach with it. Just keep an attribution back to this project.
