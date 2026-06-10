# Working in this repo

Catalog of classes Brandon Harvey teaches at Dallas Makerspace, published at classes.smarthomesellout.com. MkDocs Material on GitHub Pages; pandoc builds docx/PDF versions of each class's materials.

## Layout

- `docs/index.md`: hub landing page, one card per class.
- `docs/<class-slug>/`: one folder per class (`index.md` class page, `outline.md` speaker outline, `handout.md` attendee handout).
- `docs/stylesheets/extra.css`: the entire brand restyle. No other styling anywhere.
- `specs/`, `plans/`: design docs, NOT published (everything under `docs/` is).

## The hard rule: shared Markdown

`outline.md` and `handout.md` in each class folder feed BOTH the website and the pandoc docx/PDF builds. They must stay plain portable Markdown:

- No admonitions (`!!! note`), no raw HTML, no `{ .class }` attributes, no grid cards. Pandoc renders all of these as literal garbage in the print versions.
- Site-only pages (`docs/index.md` and each class's `index.md`) are never read by pandoc; Material features are fine there.

## Design rules

Tokens in `extra.css` mirror smarthomesellout.com's `src/styles/global.css` (the 2026-06 de-slop redesign). If the main site's tokens change, update `extra.css` to match.

- Dark default, light toggle. ONE accent: amber `#ffb454` (`#9b5d00` for text links in light mode).
- Flat 1px borders, 0.75rem radius. Never add box shadows, gradients, glow, or extra accent colors.
- Headings: Space Grotesk. Body: Inter. Code: JetBrains Mono.
- Voice: terse, concrete, plain headings. No em dashes anywhere, in any file, ever: use commas, colons, parentheses, or sentence breaks.
- Keep bold for product names, giveaway cues, and real warnings. No bold-stuffing.

## Adding a class

1. Create `docs/<class-slug>/` with `index.md`, `outline.md`, `handout.md`.
2. Promote its coming-soon card on `docs/index.md` to a live card linking the class page.
3. Add a nav section in `mkdocs.yml` (first child is the bare `index.md` path so `navigation.indexes` makes it the section landing).
4. In `.github/workflows/build.yml`: add `build` lines for the new files and a "Refresh rolling release" step with a new tag (pattern: short class slug, e.g. `esphome`).

## Workflow

- Edit Markdown, push to `main`. Pages deploys the site; build.yml refreshes the touched class's rolling release. There is no staging.
- Releases are rolling, one per class (`intro-ha`, ...). Never create version tags for class materials; download URLs under `releases/download/<tag>/` must stay permanent.
- Local preview: `python3 -m venv .venv && .venv/bin/pip install "mkdocs-material==9.*" mkdocs-redirects && .venv/bin/mkdocs serve`.
- Verify before pushing: `.venv/bin/mkdocs build --strict` must pass with zero warnings.
- Moving or renaming a page requires a `redirect_maps` entry in `mkdocs.yml`.
