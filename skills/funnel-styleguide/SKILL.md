---
name: funnel-styleguide
description: Apply a token-driven style guide when building or editing blocks in a themeable funnel or landing-page builder. Use when adding a new block type, styling an existing block, or touching colors, typography, spacing, or radius in a block-based page editor.
metadata:
  author: osiro
  version: "1.0.0"
---

# Style guide

A themeable block builder keeps its style guide in code, not a doc: one style-guide config object
plus a color-token table that every block reads instead of hardcoding visuals. Before styling a
block, find that config object and that token resolver in the codebase — they're the source of
truth — and read from them rather than reinventing a value locally.

## Colors are tokens, never hex

A block's config should store a color token (e.g. `PRIMARY`, `SECONDARY`, or a themed name like
`level1`, `accent-bg`), resolved to a real color only at render time by a shared resolver function.
Writing a raw hex or rgb value into a block's config is a style-guide violation — it can't follow
the theme when the palette changes. If light/dark shades exist, they should derive from the base
token (e.g. via CSS `color-mix`), not be separate hand-picked colors.

## One config object owns every visual knob

Radius, border thickness, element size, layout (boxed/full, width, padding), spacing, min-height,
vertical alignment, and animation type belong on the single style-guide config object, not as an
ad-hoc prop on one block. A new visual knob goes there so every block and every theme gets it —
bolting a one-off style prop onto a single block instead is the failure mode to avoid.

## Typography is a small fixed set of slots

Themeable builders typically expose a small, fixed number of font slots (commonly one heading font
and one body font) rather than letting each block pick its own font. Don't introduce a new font
slot or a per-block font override; extend the shared font list instead.

## Block file shape is consistent across the block catalog

Each block type in the catalog follows the same file layout (e.g. a registry entry, a config
schema, an editor setup panel, and a render component, each in its own file). Before adding a new
block, open two or three existing ones and match their file layout exactly — a variant with logic
inlined into the registry file, or a schema split across files, breaks the pattern every other
block follows.

Some block types (commonly a generic container and an auth/verification step) are "core" — shipped
by the shared editor package itself and merged into every theme's registry, rather than redefined
per theme. Don't reimplement a core block inside a theme.

## Reuse shared style helpers

Use the shared class-merging helper (a `clsx`/`tailwind-merge` combo, commonly named `cn()`) to
compose classes instead of string concatenation, and reuse shared primitive components (buttons,
inputs) instead of rebuilding their styling per block.

Done when: every color in the block's config is a token (not a raw hex/rgb literal), every visual
knob reads from the shared style-guide config (nothing new bolted on ad-hoc), the block's file
layout matches its neighbors in the catalog, and shared helpers/components are reused rather than
re-implemented.
