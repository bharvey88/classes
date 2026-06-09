# Classes site redesign: monorepo + de-slop

**Date:** 2026-06-09
**Status:** Approved by Brandon (pending spec review)
**Repo:** `bharvey88/home-assistant-intro-class`, to be renamed `bharvey88/classes`
**Live site:** https://classes.smarthomesellout.com/

Note: specs live in `specs/` at the repo root, not `docs/`, because everything
under `docs/` is published by MkDocs.

## Problem

The site at classes.smarthomesellout.com reads as AI slop. Two causes:

1. **Visual:** completely stock MkDocs Material (default teal, default fonts, no
   logo, no images, homepage is a link list). Stock Material is the most
   recognizable "generated docs site" look there is.
2. **Writing:** the substance is good, but the presentation layer has classic
   generated-text tells: em dashes throughout `outline.md` and `handout.md`, the
   same "HA is the middleman... no ten vendor apps, no cloud lock-in, your data
   stays yours" paragraph pasted nearly verbatim in three files, bold-stuffing,
   and arrow chains ("mailbox opens → camera snapshot → phone notification").

Additionally, the repo is named and structured around a single class, but Brandon
is planning at least four: Intro to Home Assistant (live), ESPHome, low-voltage
LED lighting with WLED firmware, and soldering. classes.smarthomesellout.com
should be a catalog, and a GitHub Pages custom domain can only map to one repo,
so multi-repo approaches were considered and rejected (subdomain sprawl or a
fragile CI stitching pipeline, plus four drifting copies of theme/CI/guidance).

## Decisions made

- **Stay on MkDocs Material.** Brandon is comfortable maintaining it and wants
  built-in search. Astro rebuild was considered and rejected.
- **Monorepo for all classes.** Rename the repo to `classes` (GitHub redirects
  the old name; forks, clones, and release links keep working).
- **Branding matches smarthomesellout.com's 2026-06 de-slop redesign.** Dark
  default with a light toggle, one amber accent, flat 1px-border cards, no
  gradients, no glow, no shadows, no uppercase eyebrow labels.
- **Prose: fix style, keep substance.** All facts, gear picks, stories, timings,
  and block structure stay. Only the style-level tells get fixed.
- **Hub lists all four classes now:** one live card, three "coming soon" cards
  (ESPHome, WLED low-voltage LED, soldering). No dates promised.

## Repo structure (after)

```
classes/
  .github/workflows/
    pages.yml          # builds MkDocs site -> GitHub Pages (path updates only)
    build.yml          # builds docx/PDF per class (parameterized, see below)
  docs/
    CNAME              # classes.smarthomesellout.com (unchanged)
    index.md           # hub landing: intro + one card per class
    stylesheets/
      extra.css        # the entire brand restyle lives here
    assets/            # logo SVG, favicon
    intro-to-home-assistant/
      index.md         # class page: what it covers, downloads, byline
      outline.md       # speaker cue outline (moved, content cleaned)
      handout.md       # attendee handout (moved, content cleaned)
  specs/               # design docs, not published
  mkdocs.yml
  README.md            # rewritten for the catalog
  CLAUDE.md            # agent guidance (new)
  AGENTS.md            # one line pointing at CLAUDE.md (new)
  LICENSE              # CC BY 4.0, unchanged
```

Future classes are a new folder under `docs/` plus a card on the hub. The
ESPHome, WLED, and soldering classes get no folders yet, only coming-soon cards.

### URL structure

- `/` hub landing
- `/intro-to-home-assistant/` class page
- `/intro-to-home-assistant/outline/` and `/intro-to-home-assistant/handout/`

Old URLs `/outline/` and `/handout/` get redirects via the
`mkdocs-redirects` plugin so existing links and QR codes keep working.

## Visual design

All styling lives in `docs/stylesheets/extra.css`, registered via `extra_css`.
Design tokens are copied from smarthomesellout.com's `src/styles/global.css`:

- **Dark scheme (default):** bg `#0d0f17`, surface `#141823`, border `#232839`,
  text `#f0eee9`, muted `#a8a49c`, accent amber `#ffb454`.
- **Light scheme (toggle):** bg `#faf9f6`, surface `#ffffff`, border `#e3dfd5`,
  text `#1b1914`, accent `#9b5d00` for text/links (amber fails contrast on
  white), amber `#ffb454` stays for button fills.
- **Fonts:** Space Grotesk for headings (loaded + applied in extra.css), Inter
  for body (via Material's `theme.font`), JetBrains Mono for code.
- **Shape:** flat cards, 1px solid borders, 0.75rem radius, no box shadows.
- Teal/violet/green are data colors on the main site; here they are simply not
  used. One accent.

Implementation notes:

- `theme.palette` uses `primary: custom` / `accent: custom` with two manual
  toggle entries, dark listed first so dark is the default regardless of OS
  preference (matches the main site). CSS custom properties for both schemes
  (`[data-md-color-scheme="slate"]` and `="default"`) are overridden in
  extra.css: `--md-default-bg-color`, `--md-primary-fg-color`,
  `--md-accent-fg-color`, `--md-typeset-a-color`, code block colors, etc.
- Header gets a small SVG logo mark reusing the main site's wordmark slash
  motif (amber slash), plus a matching favicon. Final asset drawn during
  implementation; constraint is: simple, flat, single accent color.
- Tables, admonitions, search, and nav restyled to flat-card look. Material's
  layout bones (header/sidebar/search) are kept; they earn their keep and the
  goal is de-slopping, not hiding that it's a docs site.

## The shared-markdown constraint (hard rule)

`outline.md` and `handout.md` feed BOTH the website and the pandoc docx/PDF
builds. Therefore they stay plain portable Markdown:

- No admonition blocks (`!!! note`), no Material grid-card syntax, no raw HTML,
  no `attr_list` tricks. Pandoc renders all of these as literal garbage.
- Their visual upgrade comes from extra.css targeting plain elements (headings,
  tables, lists, hr, blockquotes).

Site-only pages (`index.md` files at hub and class level) are never consumed by
pandoc, so Material features (grid cards, buttons, icons) are allowed and
encouraged there.

## Content changes

### Hub landing (`docs/index.md`, new)

- Title + one-paragraph intro: free classes Brandon teaches at Dallas
  Makerspace; read online, download print versions, fork and teach your own.
- Grid of class cards. Live card: Intro to Home Assistant (links to class
  page). Coming-soon cards: ESPHome, Low-Voltage LED Lighting with WLED,
  Soldering. Coming-soon cards are non-links, visually muted, no dates.
- Byline + CC BY 4.0 note.

### Class page (`docs/intro-to-home-assistant/index.md`)

Reworked from the current `index.md`: what the class covers (the one canonical
"middleman" paragraph lives here), links to outline and handout, download
buttons for docx/PDF from the latest release, fork-and-teach instructions.

### Outline and handout (moved + style cleanup)

Fix style, keep substance:

- Remove every em dash (replace with commas, colons, parentheses, or sentence
  breaks). This matches the no-em-dash rule used across all Brandon's repos.
- The "middleman" paragraph appears once, on the class page. The outline keeps
  only its talking-point cue version in Block 2; the handout keeps one short
  "What it is" line reworded so it is not a verbatim paste.
- Cut bold down to genuinely load-bearing labels (gear names, giveaway cues).
- Smooth arrow chains into sentences where prose reads better. Arrows may stay
  in genuinely sequential cue lists in the speaker outline where they help at a
  glance while presenting; the handout prose loses them.
- No reordering, no dropped facts, no new claims, no reworded gear picks.
  Timings, block structure, and Brandon's stories stay exactly as written.

### README (rewritten)

Catalog framing: what this repo is, the live class, the three planned classes,
how the builds work (site + per-class docx/PDF), fork-and-teach instructions,
license. Same facts as today's README where still true, updated paths and repo
name, no em dashes.

## CI / workflows

### pages.yml

Unchanged in spirit: build MkDocs site, deploy to GitHub Pages. Gains
`mkdocs-redirects` in the pip install step.

### build.yml (docx/PDF)

Parameterized per class:

- A class is identified by its folder slug (`intro-to-home-assistant`).
- Tag scheme is per class: `intro-ha-v1.2.0` builds and releases only that
  class's documents, attached to the release. A small mapping in the workflow
  resolves tag prefix to folder slug and output basenames
  (`Intro-to-HA-Speaker-Outline.docx/pdf`, `Intro-to-HA-Attendee-Handout.docx/pdf`).
- Push builds (artifacts only) build all classes' documents.
- Existing `v*` releases stay as history; new releases use the per-class scheme.

Pandoc/LibreOffice conversion pipeline itself is unchanged.

## Guidance files

### CLAUDE.md (new)

Covers, tersely:

- What the repo is: monorepo for SmartHomeSellout classes, MkDocs Material on
  GitHub Pages at classes.smarthomesellout.com, pandoc docx/PDF builds per
  class via release tags.
- The shared-markdown hard rule (outline/handout plain Markdown only; site-only
  index pages may use Material features).
- Design rules: one amber accent, dark default, flat 1px borders, no gradients,
  no glow, no shadows, no em dashes anywhere, terse concrete voice, plain
  headings. Tokens mirror smarthomesellout.com's global.css; if the main site's
  tokens change, update extra.css to match.
- How to add a class: folder under docs/, hub card, nav entry, tag mapping in
  build.yml.
- Workflow: edit markdown, push to main, Pages deploys; tag `<class>-vX.Y.Z`
  for document releases. Local preview: `pip install mkdocs-material
  mkdocs-redirects && mkdocs serve`.

### AGENTS.md (new)

One line: see CLAUDE.md.

## Out of scope

- No content changes beyond the style cleanup described above.
- No new classes authored now (folders/cards only when Brandon writes them).
- No analytics, comments, or custom domain changes.
- No changes to the main smarthomesellout.com repo.

## Risks

- **Repo rename:** GitHub redirects old web URLs, git remotes, and release
  links. Local clone remotes should still be updated. CNAME/Pages config
  survives a rename, but verify Pages still serves after renaming.
- **URL moves:** handled by mkdocs-redirects; the site is 8 days old, risk is
  low.
- **Material CSS overrides:** Material's custom-property surface is stable and
  documented; pin `mkdocs-material` in the pip install to avoid surprise theme
  changes (currently unpinned).

## Acceptance

1. classes.smarthomesellout.com serves the hub with one live card and three
   coming-soon cards, dark by default, amber accent, Space Grotesk headings.
2. Old `/outline/` and `/handout/` URLs redirect to the new paths.
3. `git tag intro-ha-v1.1.0 && git push --tags` produces a release with the
   four renamed documents attached.
4. No em dash remains in any markdown file in `docs/`.
5. The middleman paragraph appears in full exactly once.
6. docx/PDF outputs contain no literal `!!!`, raw HTML, or card syntax.
7. CLAUDE.md and AGENTS.md exist and reflect the above.
