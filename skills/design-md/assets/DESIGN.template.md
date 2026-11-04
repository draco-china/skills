---
version: alpha
name: Product Name
description: One sentence on the product and the feeling the interface should create.
colors:
  primary: "#1F4FD8"
  on-primary: "#FFFFFF"
  ink: "#16181D"
  ink-muted: "#5B6170"
  canvas: "#FFFFFF"
  surface: "#F5F6F8"
  hairline: "#E3E5EA"
  danger: "#B42318"
  on-danger: "#FFFFFF"
typography:
  display:
    fontFamily: Inter
    fontSize: 2.5rem
    fontWeight: 600
    lineHeight: 1.15
    letterSpacing: -0.02em
  heading:
    fontFamily: Inter
    fontSize: 1.25rem
    fontWeight: 600
    lineHeight: 1.3
  body:
    fontFamily: Inter
    fontSize: 1rem
    fontWeight: 400
    lineHeight: 1.5
  label:
    fontFamily: Inter
    fontSize: 0.875rem
    fontWeight: 500
    lineHeight: 1.4
rounded:
  sm: 4px
  md: 8px
  lg: 12px
spacing:
  xs: 4px
  sm: 8px
  md: 16px
  lg: 24px
  xl: 40px
components:
  page:
    backgroundColor: "{colors.canvas}"
    textColor: "{colors.ink}"
    typography: "{typography.body}"
  card:
    backgroundColor: "{colors.surface}"
    textColor: "{colors.ink}"
    rounded: "{rounded.lg}"
    padding: "{spacing.lg}"
  caption:
    backgroundColor: "{colors.canvas}"
    textColor: "{colors.ink-muted}"
    typography: "{typography.label}"
  divider:
    backgroundColor: "{colors.hairline}"
    height: 1px
  button-primary:
    backgroundColor: "{colors.primary}"
    textColor: "{colors.on-primary}"
    typography: "{typography.label}"
    rounded: "{rounded.md}"
    padding: 10px 16px
  button-danger:
    backgroundColor: "{colors.danger}"
    textColor: "{colors.on-danger}"
    typography: "{typography.label}"
    rounded: "{rounded.md}"
    padding: 10px 16px
---

<!-- Template: replace every value with evidence from the project, delete sections that don't apply, then lint. -->

## Overview

Two or three sentences on the product, its audience, and the intended character (for example "calm, dense, tool-like" or "editorial, generous, warm"). Name any catalog references adopted and what was taken from each.

## Colors

- **Primary:** the single interaction color for primary actions, links, and focus. Never used for large fills.
- **Ink / Ink muted:** body text and secondary text such as captions and metadata.
- **Canvas / Surface:** page background, and raised areas such as cards and panels.
- **Hairline:** borders and dividers only.
- **Danger:** destructive actions and errors. Always paired with text, never color alone.

## Typography

One family throughout. Display is used only for page titles. Heading is for section titles, body is the default, and label is for buttons, form labels, and captions.

## Layout

A 4px base grid. Use `md` for padding inside components, `lg` between related groups, and `xl` between page sections. State the max content width and breakpoints here.

## Elevation & Depth

Prefer separating areas with surface color and hairlines rather than shadows. If shadows exist, list them here and say when each is used.

## Shapes

`sm` for inputs and tags, `md` for buttons, `lg` for cards and dialogs. Don't mix radii within one component.

## Components

Buttons: one primary per view, and secondary actions use outline or ghost styles. Cards: surface background, no border unless they sit on a surface already.

## Do's and Don'ts

- Do use tokens through the project's theme layer instead of literal values.
- Do keep text contrast at WCAG AA or better.
- Don't introduce colors, radii, or spacing outside these scales without updating this file.
