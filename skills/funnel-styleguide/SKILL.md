---
name: funnel-styleguide
description: Apply design taste when styling a funnel or landing page through osiro's MCP tools (set_funnel_style_guide, set_landing_page_style_guide, add_funnel_block, add_landing_page_block, add_funnel_composition). Use whenever building or restyling a funnel or landing page so the result looks intentional, not just technically valid.
metadata:
  author: osiro
  version: "1.0.0"
---

# Style guide taste

osiro's style-guide tools already explain their own mechanics — `get_funnel_style_guide` and
`get_landing_page_style_guide` return the current values, the valid option lists, and what each
color slot is for. What they don't tell you is what looks good. A palette or layout can be fully
valid input and still look amateurish; this skill is the taste layer on top of a valid call.

## Read before you write

Always call `get_funnel_style_guide` / `get_landing_page_style_guide` before the matching `set_`
tool. You need the current palette, fonts, and options in front of you to make one coherent change
— picking a new `level1` without seeing `level2` risks a clash you can't see from the patch alone.

## Color: contrast over count

- `level1` is the CTA color — it must read clearly against `elementColors.background` and against
  `layout.backgroundColor` / `cardBackgroundColor`. Check it isn't close in lightness to the surface
  it sits on, or the button disappears.
- `level2` carries headings and dark text — keep it genuinely dark/saturated relative to `level1`,
  not a second accent. A palette where `level1` and `level2` are both mid-tone brights reads as
  noisy, not branded.
- `level3`/`level4` are minor accents (badges, dividers, secondary icons) — never the CTA. Reserve
  them for the smallest, least frequent elements.
- A funnel with four competing bright colors looks like a spreadsheet, not a brand. When the org
  only gives you one or two real brand colors, derive `level3`/`level4` as muted or desaturated
  variants of those rather than inventing new hues.

## One mood, not a mix

`radiusStyle`, `borderThickness`, and `elementSize` together set a mood — pick one and hold it:

- soft: `rounded`/`curved` radius + thin borders + normal/large elements
- sharp: `cornered` radius + thick borders + normal/small elements

Mixing across the two (e.g. `curved` radius with `thick` borders) reads as indecisive. If the org's
existing guide already leans one way, match it instead of introducing the other.

## Fonts: contrast in role, not in personality

`fontHeading` and `fontBody` should differ enough to separate headings from body text (e.g. a
geometric sans heading over a humanist sans body) without fighting — two heavyweight display
fonts, or fonts with wildly mismatched x-heights, both read as unpolished. When unsure, pick a
pairing from `get_funnel_style_guide`'s `options.popularFonts` rather than guessing an arbitrary
Bunny Fonts family.

## Layout: match the container to the content

- `boxed` (a centered card) suits short, form-like funnels — quizzes, lead forms, single-question
  steps.
- `full` suits content-heavy steps — long-form landing pages, image-led hero steps.

Check the funnel's length and content density before picking one; don't default to the same layout
for every funnel.

## Structure: reach for a composition before hand-building

`list_funnel_compositions` and the block-type tools' `whenToUse` notes exist because similar-looking
blocks solve different problems. Before assembling a multi-select, picture-choice, or other common
pattern block-by-block, check `list_funnel_compositions` for a prebuilt template — a composition
already ships spaced, labeled, and wired with sensible defaults, which a hand-built equivalent
usually isn't.

## Icons: sparing and consistent

When a block supports an icon, `search_icons` for one that matches the existing icon set's style
(line vs. filled, same weight) rather than mixing styles across a funnel. An icon on every block is
noise — reserve icons for elements that need a visual anchor (choice options, benefit lists), not
filler on every heading.

Done when: the palette reads with clear CTA contrast and no more than one accent role per color
slot, radius/border/element-size agree on one mood, the fonts are a deliberate pairing, the layout
type matches content length, structure came from a composition where one exists, and any icons
share one visual style.
