# Classes Site Redesign Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Turn the single-class MkDocs site at classes.smarthomesellout.com into a branded multi-class catalog, de-slopped visually and textually, with rolling per-class document releases.

**Architecture:** Keep MkDocs Material + GitHub Pages + pandoc docx/PDF pipeline. All branding lives in one `extra.css` overriding Material's CSS custom properties with smarthomesellout.com's design tokens. Classes live in per-class folders under `docs/`; the repo gets renamed to `classes` at the end, just before the final push.

**Tech Stack:** MkDocs Material (pinned 9.x), mkdocs-redirects, GitHub Actions, pandoc + LibreOffice, `gh` CLI.

**Spec:** `specs/2026-06-09-classes-site-redesign-design.md`. Read it first.

**Working directory:** `/Users/harvey/Claude Folder/home-assistant-intro-class`

**Conventions for every task:**
- No em dashes in ANY file written or edited. Use commas, colons, parentheses, or sentence breaks.
- `outline.md` and `handout.md` must stay plain portable Markdown (no admonitions, no HTML, no `{ .class }` attributes, no grid cards). Site-only `index.md` files may use Material features.
- Commit after each task with the messages given. Do not push until Task 8 says so.

---

### Task 1: Local build environment + restructure docs into per-class folders

**Files:**
- Create: `.venv/` (local only, gitignored)
- Modify: `.gitignore`
- Move: `docs/outline.md` -> `docs/intro-to-home-assistant/outline.md`
- Move: `docs/handout.md` -> `docs/intro-to-home-assistant/handout.md`
- Move: `docs/index.md` -> `docs/intro-to-home-assistant/index.md` (placeholder position; content rewritten in Task 4)
- Modify: `mkdocs.yml`

- [ ] **Step 1: Create a local venv with the site toolchain** (macOS, no Homebrew; use system `python3`)

```bash
cd "/Users/harvey/Claude Folder/home-assistant-intro-class"
python3 -m venv .venv
.venv/bin/pip install --quiet "mkdocs-material==9.*" mkdocs-redirects
.venv/bin/mkdocs --version
```

Expected: a `mkdocs, version 1.x.x` line. If `python3 -m venv` fails, stop and report; do not install anything system-wide.

- [ ] **Step 2: Gitignore the venv and build output**

Ensure `.gitignore` contains these lines (append any that are missing):

```
site/
.venv/
dist/
```

- [ ] **Step 3: Move the class files into their folder**

```bash
mkdir -p docs/intro-to-home-assistant
git mv docs/outline.md docs/intro-to-home-assistant/outline.md
git mv docs/handout.md docs/intro-to-home-assistant/handout.md
git mv docs/index.md docs/intro-to-home-assistant/index.md
```

`docs/CNAME` stays where it is.

- [ ] **Step 4: Create a temporary hub `docs/index.md`** (real content comes in Task 4; the site must build at every commit)

```markdown
# Classes

Free classes taught at Dallas Makerspace. Catalog landing page under construction.

- [Intro to Home Assistant](intro-to-home-assistant/index.md)
```

- [ ] **Step 5: Replace `mkdocs.yml` entirely with the new config**

```yaml
site_name: SmartHomeSellout Classes
site_description: Free smart home and maker classes taught at Dallas Makerspace. Read online, download print versions, or fork and teach your own.
site_url: https://classes.smarthomesellout.com/

repo_url: https://github.com/bharvey88/classes
repo_name: bharvey88/classes
edit_uri: edit/main/docs/

theme:
  name: material
  logo: assets/logo.svg
  favicon: assets/logo.svg
  icon:
    repo: fontawesome/brands/github
  font:
    text: Inter
    code: JetBrains Mono
  palette:
    - scheme: slate
      primary: custom
      accent: custom
      toggle:
        icon: material/weather-sunny
        name: Switch to light mode
    - scheme: default
      primary: custom
      accent: custom
      toggle:
        icon: material/weather-night
        name: Switch to dark mode
  features:
    - navigation.instant
    - navigation.top
    - navigation.footer
    - navigation.indexes
    - content.action.edit
    - search.suggest
    - search.highlight
    - toc.follow

extra_css:
  - stylesheets/extra.css

nav:
  - Home: index.md
  - Intro to Home Assistant:
      - intro-to-home-assistant/index.md
      - Speaker outline: intro-to-home-assistant/outline.md
      - Attendee handout: intro-to-home-assistant/handout.md

markdown_extensions:
  - admonition
  - tables
  - attr_list
  - md_in_html
  - toc:
      permalink: true
  - pymdownx.emoji:
      emoji_index: !!python/name:material.extensions.emoji.twemoji
      emoji_generator: !!python/name:material.extensions.emoji.to_svg

plugins:
  - search
  - redirects:
      redirect_maps:
        outline.md: intro-to-home-assistant/outline.md
        handout.md: intro-to-home-assistant/handout.md
```

Notes for the implementer:
- `scheme: slate` listed first makes dark the default regardless of OS preference (matches smarthomesellout.com).
- `primary: custom` / `accent: custom` tells Material to expect our CSS custom properties (Task 2).
- `navigation.indexes` makes `intro-to-home-assistant/index.md` the clickable landing of its nav section.
- `theme.logo` and `extra_css` reference files created in Task 2. That is fine for this commit because Step 6 builds WITHOUT `--strict` (missing logo/css are warnings, not errors). Task 2 switches verification to `--strict`.

- [ ] **Step 6: Build to verify the restructure**

```bash
.venv/bin/mkdocs build 2>&1 | tail -5
```

Expected: "Documentation built" with no ERROR lines (warnings about the not-yet-existing `assets/logo.svg` and `stylesheets/extra.css` are acceptable in this task only). Also verify the redirect stubs exist:

```bash
ls site/outline/index.html site/handout/index.html site/intro-to-home-assistant/outline/index.html
```

Expected: all three files listed.

- [ ] **Step 7: Commit**

```bash
git add -A
git commit -m "Restructure docs into per-class folders with redirects from old URLs"
```

---

### Task 2: Brand restyle (extra.css + logo)

**Files:**
- Create: `docs/stylesheets/extra.css`
- Create: `docs/assets/logo.svg`

- [ ] **Step 1: Create `docs/assets/logo.svg`**

The mark is the main site's wordmark slash motif: an amber slash on a flat dark rounded tile. Single accent color, no gradients.

```svg
<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 32 32" role="img" aria-label="SmartHomeSellout Classes">
  <rect width="32" height="32" rx="7" fill="#141823"/>
  <path d="M19.8 6.5 12.2 25.5" stroke="#ffb454" stroke-width="3.5" stroke-linecap="round" fill="none"/>
</svg>
```

- [ ] **Step 2: Create `docs/stylesheets/extra.css`**

Tokens are copied from smarthomesellout.com `src/styles/global.css` (dark + light). Do not invent new colors.

```css
/* =========================================================================
   classes.smarthomesellout.com - brand overrides for MkDocs Material
   Tokens mirror smarthomesellout.com src/styles/global.css.
   One accent (amber). Flat 1px borders. No shadows, no gradients.
   ========================================================================= */

@import url("https://fonts.googleapis.com/css2?family=Space+Grotesk:wght@500;600;700&display=swap");

/* ---- Dark scheme (default) ---- */
[data-md-color-scheme="slate"] {
  --md-default-bg-color: #0d0f17;
  --md-default-fg-color: #f0eee9;
  --md-default-fg-color--light: #a8a49c;
  --md-default-fg-color--lighter: #71737e;
  --md-default-fg-color--lightest: #232839;

  --md-primary-fg-color: #11141d;   /* header / nav chrome */
  --md-primary-bg-color: #f0eee9;   /* text on header */
  --md-accent-fg-color: #ffb454;
  --md-typeset-a-color: #ffb454;

  --md-code-bg-color: #141823;
  --md-code-fg-color: #f0eee9;

  --md-footer-bg-color: #11141d;
  --md-footer-bg-color--dark: #0d0f17;
  --md-footer-fg-color: #f0eee9;
  --md-footer-fg-color--light: #a8a49c;

  --shs-surface: #141823;
  --shs-surface-hover: #1a1f2e;
  --shs-border: #232839;
  --shs-border-strong: #323a4f;
  --shs-accent: #ffb454;
  --shs-accent-ink: #1a1409;
}

/* ---- Light scheme (toggle) ---- */
[data-md-color-scheme="default"] {
  --md-default-bg-color: #faf9f6;
  --md-default-fg-color: #1b1914;
  --md-default-fg-color--light: #59554b;
  --md-default-fg-color--lighter: #8b8779;
  --md-default-fg-color--lightest: #e3dfd5;

  --md-primary-fg-color: #ffffff;
  --md-primary-bg-color: #1b1914;
  /* Amber text fails contrast on white; links darken, fills stay amber. */
  --md-accent-fg-color: #9b5d00;
  --md-typeset-a-color: #9b5d00;

  --md-code-bg-color: #f2efe9;
  --md-code-fg-color: #1b1914;

  --md-footer-bg-color: #ffffff;
  --md-footer-bg-color--dark: #faf9f6;
  --md-footer-fg-color: #1b1914;
  --md-footer-fg-color--light: #59554b;

  --shs-surface: #ffffff;
  --shs-surface-hover: #f2efe9;
  --shs-border: #e3dfd5;
  --shs-border-strong: #c9c3b4;
  --shs-accent: #ffb454;
  --shs-accent-ink: #1a1409;
}

/* ---- Kill Material's elevation shadows everywhere ---- */
:root {
  --md-shadow-z1: none;
  --md-shadow-z2: none;
  --md-shadow-z3: none;
}

/* ---- Typography ---- */
.md-typeset h1,
.md-typeset h2,
.md-typeset h3,
.md-typeset h4,
.md-typeset h5 {
  font-family: "Space Grotesk", "Inter", system-ui, -apple-system, "Segoe UI", Roboto, sans-serif;
  font-weight: 600;
  color: var(--md-default-fg-color);
}
.md-typeset h1 { font-weight: 700; }

/* ---- Header: flat, bordered, no shadow ---- */
.md-header {
  box-shadow: none;
  border-bottom: 1px solid var(--shs-border);
}
.md-header__title {
  font-family: "Space Grotesk", "Inter", system-ui, sans-serif;
  font-weight: 600;
}

/* ---- Search field: flat surface ---- */
[data-md-toggle="search"]:checked ~ .md-header .md-search__form,
.md-search__form {
  background: var(--shs-surface);
  border: 1px solid var(--shs-border);
  border-radius: 0.5rem;
  box-shadow: none;
}
.md-search__output {
  border: 1px solid var(--shs-border);
  border-radius: 0 0 0.5rem 0.5rem;
  box-shadow: none;
}

/* ---- Grid cards: flat 1px-border surfaces ---- */
.md-typeset .grid.cards > ol > li,
.md-typeset .grid.cards > ul > li {
  background: var(--shs-surface);
  border: 1px solid var(--shs-border);
  border-radius: 0.75rem;
  box-shadow: none;
  transition: border-color 0.15s ease;
}
.md-typeset .grid.cards > ol > li:hover,
.md-typeset .grid.cards > ul > li:hover {
  border-color: var(--shs-border-strong);
  box-shadow: none;
}
.md-typeset .grid.cards > ul > li hr {
  border-bottom: 1px solid var(--shs-border);
}

/* Coming-soon cards: muted, non-interactive */
.md-typeset .coming-soon.grid.cards > ul > li {
  opacity: 0.6;
}
.md-typeset .coming-soon.grid.cards > ul > li:hover {
  border-color: var(--shs-border);
}

/* ---- Buttons ---- */
.md-typeset .md-button {
  border: 1px solid var(--shs-border-strong);
  border-radius: 0.75rem;
  color: var(--md-default-fg-color);
}
.md-typeset .md-button:hover {
  background: var(--shs-surface-hover);
  border-color: var(--shs-border-strong);
  color: var(--md-default-fg-color);
}
.md-typeset .md-button--primary {
  background: var(--shs-accent);
  border-color: var(--shs-accent);
  color: var(--shs-accent-ink);
}
.md-typeset .md-button--primary:hover {
  background: var(--shs-accent);
  border-color: var(--shs-accent);
  color: var(--shs-accent-ink);
  filter: brightness(1.06);
}

/* ---- Tables: flat, bordered, rounded ---- */
.md-typeset table:not([class]) {
  border: 1px solid var(--shs-border);
  border-radius: 0.75rem;
  box-shadow: none;
}
.md-typeset table:not([class]) th {
  background: var(--shs-surface);
}

/* ---- Admonitions (site-only pages): flat, single accent ---- */
.md-typeset .admonition,
.md-typeset details {
  border: 1px solid var(--shs-border);
  border-left: 3px solid var(--shs-accent);
  border-radius: 0.5rem;
  box-shadow: none;
}

/* ---- Footer ---- */
.md-footer-meta {
  border-top: 1px solid var(--shs-border);
}
```

- [ ] **Step 3: Build strictly and eyeball the result**

```bash
.venv/bin/mkdocs build --strict 2>&1 | tail -3
```

Expected: "Documentation built", zero warnings (strict mode fails on warnings; the logo and css now exist).

Then serve and verify by eye in the preview/browser:

```bash
.venv/bin/mkdocs serve --dev-addr 127.0.0.1:8001
```

Check at `http://127.0.0.1:8001`: dark background `#0d0f17`, amber links, Space Grotesk headings, flat header with bottom border, the slash logo in the header, the sun toggle switches to the light scheme with dark-amber links. Stop the server after checking.

- [ ] **Step 4: Commit**

```bash
git add docs/stylesheets/extra.css docs/assets/logo.svg
git commit -m "Brand restyle: smarthomesellout tokens, dark default, amber accent, flat borders"
```

---

### Task 3: Prose cleanup of outline.md and handout.md

**Files:**
- Modify: `docs/intro-to-home-assistant/outline.md`
- Modify: `docs/intro-to-home-assistant/handout.md`

These two files feed pandoc as well as the site. They must remain plain Markdown. Fix style, keep substance: no reordering, no dropped facts, no new claims, no changed gear picks, timings and block structure untouched.

- [ ] **Step 1: Replace every em dash in both files**

Rules, in order of preference per occurrence:
1. Heading separators like `## Block 1 — The Hook (5 min)` become a colon: `## Block 1: The Hook (5 min)`.
2. Definition-style dashes like `**HA Green** — official plug-and-play` become a colon: `**HA Green**: official plug-and-play`.
3. Mid-sentence asides become commas, parentheses, or a sentence break, whichever reads naturally.
4. Never just delete the dash and mash words together.

Em dashes inside quoted speaker lines (the italicized things Brandon literally says aloud) get the same treatment; spoken text does not need dashes.

Verify both files:

```bash
grep -c '—' docs/intro-to-home-assistant/outline.md docs/intro-to-home-assistant/handout.md; grep -c '–' docs/intro-to-home-assistant/outline.md docs/intro-to-home-assistant/handout.md
```

Expected: `0` for every file on both greps (grep exits 1 when count is 0; that is the pass condition).

- [ ] **Step 2: De-duplicate the middleman paragraph**

The canonical full version will live on the class page (Task 4). In these two files:

In `outline.md`, Block 2, replace:

```markdown
- **The core idea — HA is the middleman.** It sits in the middle and connects all your devices and protocols (Zigbee, Z-Wave, Wi-Fi, Bluetooth) so you control everything from one central app, instead of ten vendor apps that don't talk to each other.
```

with the cue-card version:

```markdown
- **The core idea: HA is the middleman.** One app in the middle, every protocol (Zigbee, Z-Wave, Wi-Fi, Bluetooth) connected to it, instead of ten vendor apps that don't talk to each other.
```

In `handout.md`, replace the opening "What it is" paragraph:

```markdown
**What it is:** Home Assistant is the **middleman** that connects all your smart devices and protocols (Zigbee, Z-Wave, Wi-Fi, Bluetooth) into one local app — no ten vendor apps, no cloud lock-in, your data stays yours. It's free and open source, backed by the non-profit Open Home Foundation.
```

with:

```markdown
**What it is:** Home Assistant puts every smart device in your house, whatever protocol it speaks, into one app that runs locally. Your data never leaves home and there is no subscription. It's free and open source, backed by the non-profit Open Home Foundation.
```

- [ ] **Step 3: Cut bold-stuffing**

Keep bold ONLY for: gear and product names being recommended, giveaway cues (`**GIVEAWAY**`), and genuine warnings (`**Must be present to win.**`). Unbold rhetorical emphasis like `**middleman**`, `**follow along, or fork it and teach your own version**`, `**the showstoppers**`. When in doubt, unbold. Roughly half the current bold should survive.

- [ ] **Step 4: Smooth arrow chains in the handout only**

In `handout.md`, rewrite the automation example:

```markdown
- **Automations** run themselves ("when X happens, do Y"). Example: mailbox opens → camera snapshot → phone notification with the photo → "You've got mail" plays on your speakers.
```

as:

```markdown
- Automations run themselves: when X happens, do Y. Example: the mailbox opens, a camera grabs a snapshot, your phone gets the photo, and "You've got mail" plays on the speakers.
```

In `outline.md`, arrows MAY stay in cue lists where they help the speaker scan at a glance (the Block 7 example chains, demo combos like "Magic Cube gesture → WLED color change"). Leave those alone.

- [ ] **Step 5: Build and verify pandoc output is clean**

```bash
.venv/bin/mkdocs build --strict 2>&1 | tail -3
```

Expected: builds clean. Then confirm the files are still pandoc-safe (no Material syntax slipped in):

```bash
grep -nE '^!!!|\{ \.|<div|<span' docs/intro-to-home-assistant/outline.md docs/intro-to-home-assistant/handout.md
```

Expected: no output.

- [ ] **Step 6: Commit**

```bash
git add docs/intro-to-home-assistant/outline.md docs/intro-to-home-assistant/handout.md
git commit -m "Prose cleanup: drop em dashes, dedupe middleman pitch, cut bold-stuffing"
```

---

### Task 4: Hub landing page + class page

**Files:**
- Replace: `docs/index.md` (the Task 1 placeholder)
- Replace: `docs/intro-to-home-assistant/index.md`

Both are site-only: pandoc never reads them, Material features allowed.

- [ ] **Step 1: Write `docs/index.md`**

```markdown
---
hide:
  - navigation
  - toc
---

# Classes

Free, hands-on classes I teach at the [Dallas Makerspace](https://dallasmakerspace.org), published here for anyone. Read them online, download the print versions, or fork the repo and teach your own.

<div class="grid cards" markdown>

-   **Intro to Home Assistant**

    ---

    Smart homes done the right way: every device in one local app, no cloud lock-in. From "what is this?" to live automations in about 2.5 hours.

    [Go to the class](intro-to-home-assistant/index.md){ .md-button .md-button--primary }

</div>

## Coming soon

<div class="coming-soon grid cards" markdown>

-   **ESPHome**

    ---

    Turn cheap ESP32 boards into your own sensors and devices with simple YAML.

-   **Low-Voltage LED Lighting with WLED**

    ---

    Addressable LED strips and controllers running WLED firmware: planning, power, and effects.

-   **Soldering**

    ---

    Iron technique, clean joints, and your first kit build.

</div>

Everything here is plain Markdown in a [public repo](https://github.com/bharvey88/classes), licensed [CC BY 4.0](https://github.com/bharvey88/classes/blob/main/LICENSE). Fork it, swap in your own gear and stories, and teach it at your makerspace or library.

*By Brandon Harvey (SmartHomeSellout) · [smarthomesellout.com](https://smarthomesellout.com)*
```

- [ ] **Step 2: Write `docs/intro-to-home-assistant/index.md`**

This page owns the one canonical middleman paragraph. Download links point at the rolling release (created by CI in Task 5; the links 404 until the first CI run after the rename, which is expected and resolved in Task 8).

```markdown
# Intro to Home Assistant

*Smart Homes Done the Right Way*

A class by **Brandon Harvey** (aka SmartHomeSellout) · [smarthomesellout.com](https://smarthomesellout.com), first taught at the [Dallas Makerspace](https://dallasmakerspace.org).

Home Assistant is the middleman that connects all your smart devices and protocols (Zigbee, Z-Wave, Wi-Fi, Bluetooth) into one local app. No ten vendor apps, no cloud lock-in: your data stays yours. Over about 2.5 hours the class goes from "what is this?" to running automations, choosing protocols and gear, and letting an AI agent build your dashboards.

[Speaker outline](outline.md){ .md-button .md-button--primary }
[Attendee handout](handout.md){ .md-button }

## Download the print versions

Built automatically from the same Markdown on every change:

- [Speaker outline (PDF)](https://github.com/bharvey88/classes/releases/download/intro-ha/Intro-to-HA-Speaker-Outline.pdf) · [docx](https://github.com/bharvey88/classes/releases/download/intro-ha/Intro-to-HA-Speaker-Outline.docx)
- [Attendee handout (PDF)](https://github.com/bharvey88/classes/releases/download/intro-ha/Intro-to-HA-Attendee-Handout.pdf) · [docx](https://github.com/bharvey88/classes/releases/download/intro-ha/Intro-to-HA-Attendee-Handout.docx)

## Teach it yourself

1. [Fork the repo](https://github.com/bharvey88/classes/fork).
2. Edit the Markdown in `docs/intro-to-home-assistant/` to swap in your own gear, stories, and demos.
3. Push, and the site and documents rebuild automatically.

Content is licensed [CC BY 4.0](https://github.com/bharvey88/classes/blob/main/LICENSE): use it, remix it, teach with it.
```

- [ ] **Step 3: Build strictly, check both pages render**

```bash
.venv/bin/mkdocs build --strict 2>&1 | tail -3
```

Expected: clean build. Serve and verify by eye: hub shows one bright card with an amber button and three muted coming-soon cards; class page shows two buttons and the download list.

- [ ] **Step 4: Commit**

```bash
git add docs/index.md docs/intro-to-home-assistant/index.md
git commit -m "Hub landing with class cards; class page with canonical pitch and downloads"
```

---

### Task 5: Workflows (pages.yml + build.yml)

**Files:**
- Modify: `.github/workflows/pages.yml`
- Replace: `.github/workflows/build.yml`

- [ ] **Step 1: Update the install and build steps in `pages.yml`**

Replace:

```yaml
      - name: Install MkDocs Material
        run: pip install mkdocs-material

      - name: Build site
        run: mkdocs build
```

with:

```yaml
      - name: Install MkDocs Material
        run: pip install "mkdocs-material==9.*" mkdocs-redirects

      - name: Build site
        run: mkdocs build --strict
```

Everything else in `pages.yml` stays as-is.

- [ ] **Step 2: Replace `build.yml` entirely**

```yaml
name: Build class materials

on:
  push:
    branches: [main]
  pull_request:
    branches: [main]
  workflow_dispatch:

permissions:
  contents: write

jobs:
  build:
    runs-on: ubuntu-latest
    env:
      GH_TOKEN: ${{ github.token }}
    steps:
      - name: Checkout
        uses: actions/checkout@v6

      - name: Install pandoc + LibreOffice
        run: |
          sudo apt-get update
          sudo apt-get install -y --no-install-recommends pandoc libreoffice-writer

      # One block per class. To add a class: add its build lines here and a
      # release step below, with a new rolling tag.
      - name: Build docx + pdf
        run: |
          mkdir -p dist
          build() {
            local src="$1" out="$2"
            pandoc "$src" -o "dist/$out.docx"
            soffice --headless --convert-to pdf --outdir dist "dist/$out.docx"
          }
          build docs/intro-to-home-assistant/outline.md "Intro-to-HA-Speaker-Outline"
          build docs/intro-to-home-assistant/handout.md "Intro-to-HA-Attendee-Handout"
          ls -la dist

      - name: Upload build artifacts
        uses: actions/upload-artifact@v7
        with:
          name: class-materials
          path: dist/*

      - name: Refresh rolling release (intro-ha)
        if: github.event_name == 'push' && github.ref == 'refs/heads/main'
        run: |
          tag="intro-ha"
          title="Intro to Home Assistant (latest build)"
          notes="Current speaker outline and attendee handout, rebuilt automatically on every change. Last updated: $(date -u +%Y-%m-%d)."
          if gh release view "$tag" >/dev/null 2>&1; then
            gh release edit "$tag" --notes "$notes"
          else
            gh release create "$tag" --title "$title" --notes "$notes"
          fi
          gh release upload "$tag" \
            dist/Intro-to-HA-Speaker-Outline.docx dist/Intro-to-HA-Speaker-Outline.pdf \
            dist/Intro-to-HA-Attendee-Handout.docx dist/Intro-to-HA-Attendee-Handout.pdf \
            --clobber
```

Notes for the implementer:
- The old `tags: ['v*']` trigger is gone on purpose; releases are now refreshed from pushes to main. The existing `v1.0.0` release is left alone as history.
- `--clobber` replaces same-named assets so download URLs stay permanent.
- `gh` is preinstalled on GitHub runners and authenticates via `GH_TOKEN`.

- [ ] **Step 3: Validate workflow syntax locally**

```bash
python3 -c "import yaml,sys; yaml.safe_load(open('.github/workflows/build.yml')); yaml.safe_load(open('.github/workflows/pages.yml')); print('ok')"
```

Expected: `ok`. (If PyYAML is missing from system python, run it with `.venv/bin/python`; mkdocs depends on PyYAML so the venv has it.)

- [ ] **Step 4: Commit**

```bash
git add .github/workflows
git commit -m "CI: pin mkdocs-material, strict build, rolling per-class release instead of version tags"
```

---

### Task 6: README rewrite

**Files:**
- Replace: `README.md`

- [ ] **Step 1: Replace `README.md` entirely**

```markdown
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
```

- [ ] **Step 2: Commit**

```bash
git add README.md
git commit -m "README: rewrite for the multi-class catalog"
```

---

### Task 7: CLAUDE.md + AGENTS.md

**Files:**
- Create: `CLAUDE.md`
- Create: `AGENTS.md`

- [ ] **Step 1: Create `CLAUDE.md`**

```markdown
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
```

- [ ] **Step 2: Create `AGENTS.md`**

```markdown
See [CLAUDE.md](CLAUDE.md).
```

- [ ] **Step 3: Commit**

```bash
git add CLAUDE.md AGENTS.md
git commit -m "Add CLAUDE.md and AGENTS.md agent guidance"
```

---

### Task 8: Repo rename, push, live verification

This task touches the live site. Do it last, in one sitting.

- [ ] **Step 1: Rename the repo**

```bash
gh repo rename classes --repo bharvey88/home-assistant-intro-class --yes
git remote set-url origin "$(git remote get-url origin | sed 's|home-assistant-intro-class|classes|')"
git remote -v
```

Expected: both fetch and push URLs end in `bharvey88/classes.git`. GitHub redirects the old name for web, git, and release URLs.

- [ ] **Step 2: Update the repo description**

```bash
gh repo edit bharvey88/classes --description "Free smart home and maker classes taught at Dallas Makerspace. MkDocs site + auto-built docx/PDF. Fork it and teach your own."
```

- [ ] **Step 3: Push everything**

```bash
git push origin main
```

- [ ] **Step 4: Watch both workflows**

```bash
gh run watch --repo bharvey88/classes --exit-status $(gh run list --repo bharvey88/classes --limit 1 --json databaseId --jq '.[0].databaseId')
gh run list --repo bharvey88/classes --limit 4
```

Expected: "Deploy site" and "Build class materials" both succeed. If either fails, read the log (`gh run view <id> --log-failed`), fix, commit, push again.

- [ ] **Step 5: Verify the live site and redirects**

```bash
curl -s -o /dev/null -w "%{http_code}\n" https://classes.smarthomesellout.com/
curl -s https://classes.smarthomesellout.com/outline/ | grep -o 'url=[^"]*' | head -1
curl -s -o /dev/null -w "%{http_code}\n" https://classes.smarthomesellout.com/intro-to-home-assistant/outline/
```

Expected: `200`, a redirect URL containing `intro-to-home-assistant/outline`, `200`.

- [ ] **Step 6: Verify the rolling release and permanent download URLs**

```bash
gh release view intro-ha --repo bharvey88/classes --json assets --jq '.assets[].name'
curl -sL -o /dev/null -w "%{http_code}\n" https://github.com/bharvey88/classes/releases/download/intro-ha/Intro-to-HA-Attendee-Handout.pdf
```

Expected: the four asset names, then `200`.

- [ ] **Step 7: Verify Pages custom domain survived the rename**

```bash
gh api repos/bharvey88/classes/pages --jq '{cname: .cname, https: .https_enforced, status: .status}'
```

Expected: `cname` is `classes.smarthomesellout.com`, status `built`. If the domain dropped, re-set it: `gh api -X PUT repos/bharvey88/classes/pages -f cname=classes.smarthomesellout.com`.

- [ ] **Step 8: Final acceptance sweep (from the spec)**

```bash
grep -rn '—' docs/ && echo "FAIL: em dash found" || echo "PASS: no em dashes"
grep -rln 'middleman' docs/
```

Expected: PASS, and `middleman` appears only in `docs/intro-to-home-assistant/index.md` (canonical) and `docs/intro-to-home-assistant/outline.md` (cue version). Visually confirm the live hub: dark, amber, one live card, three muted coming-soon cards.

---

## Self-review notes

- Spec coverage: rename (T8), restructure + redirects (T1), restyle (T2), prose (T3), hub + class page (T4), rolling releases + pinned CI (T5), README (T6), guidance files (T7), acceptance checks (T8). Coming-soon cards unlinked and dateless (T4). Old `v1.0.0` release untouched (T5).
- The download links written in Task 4 404 until Task 8's first CI run; this is called out in Task 4 and verified in Task 8 Step 6.
- TDD does not apply in the unit-test sense to a content site; every task ends with a concrete verification command (`mkdocs build --strict`, greps, curl checks) and its expected output.
