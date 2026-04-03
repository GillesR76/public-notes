# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project overview

Static site published via GitHub Pages. No build step, no dependencies — pure HTML/CSS. Each note is a self-contained `index.html` inside its own subfolder. The root `index.html` is the index page linking to all notes.

## Structure

```
index.html                  ← index page (cards linking to notes)
ecosysteme-claude/
  index.html                ← first note (multi-section reference doc with inline SVGs)
```

Each note folder contains a single `index.html` — no assets, no external files. Everything (styles, SVGs, content) is inline.

## Design system

All pages share the same design tokens (defined in `:root` CSS vars):
- **Fonts**: `Outfit` (body) + `JetBrains Mono` (labels/code), loaded from Google Fonts
- **Palette**: `--navy #0D1B2A`, `--accent #8B4A2F`, `--accent-mid #C17A5A`, `--bg #F7F4F0`
- **Colors by context**: green `#3A5C42 / #E8F0EA` for skills, terracotta `#8B4A2F / #FDF6F0` for commands, indigo `#4A5480 / #E8EAF0` for plugins/config

When creating a new note, copy the header/footer pattern and CSS variables from an existing note to stay consistent.

## SVG diagrams — text centering rules

SVG text inside rects uses `<tspan>` with explicit `y` coordinates. The only reliable pattern:

- **No `dominant-baseline` on the `<text>` parent**
- **`dominant-baseline="central"` on every `<tspan>`** — makes `y` the geometric center of the em box, font-agnostic
- **`y` values calculated from rect geometry**: `center_y = rect_y + rect_height / 2`

For multi-line blocks:
- 2 lines (sizes 11–12): `tspan1_y = center_y - 7`, `tspan2_y = center_y + 7`
- 3 lines (title 13 + 2×11): `tspan1_y = center_y - 17`, `tspan2_y = center_y`, `tspan3_y = center_y + 17`
- 3 lines (uniform 11): `tspan1_y = center_y - 14`, `tspan2_y = center_y`, `tspan3_y = center_y + 14`

Do **not** use `dominant-baseline="middle"` on `<text>` — it is inherited inconsistently across browsers when tspans have explicit `y` attributes, and shifts with web font loading.

## Adding a new note

1. Create `note-slug/index.html` — model it on `ecosysteme-claude/index.html`
2. Add a `.note-card` entry in the root `index.html` pointing to `note-slug/`
3. Update the date in the root footer if needed
