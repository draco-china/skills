---
name: design-md
description: Create, adopt, update, lint, and enforce a repository-root DESIGN.md, the visual source of truth for coding agents, in the Google Labs DESIGN.md format, optionally drawing style direction from the bundled VoltAgent awesome-design-md catalog. Use when the user asks for DESIGN.md, a design system or design tokens, a brand or UI style direction, wants an existing interface distilled into a design contract, wants to borrow the look of a known product (e.g. "make it feel like Linear/Stripe/Notion"), asks to export tokens to Tailwind/CSS/DTCG, or asks for UI implementation or review in a repository whose root already contains DESIGN.md.
---

# DESIGN.md

`DESIGN.md` at the repository root is a durable visual contract: YAML tokens give agents exact values, and markdown prose explains why they exist and how to apply them. This skill creates and maintains that file and checks UI work against it.

Bundled references (synced daily from upstream, do not edit):

| Path | What it is |
|---|---|
| `references/design.md/spec.md` | Google Labs DESIGN.md format spec: the authority on structure, token schema, and lint rules |
| `references/awesome-design-md/<slug>/DESIGN.md` | VoltAgent catalog: DESIGN.md files reverse-engineered from public product sites |
| `assets/DESIGN.template.md` | Minimal valid starting file |

Paths are relative to this skill's directory. If a reference is missing, use the upstream instead: <https://github.com/google-labs-code/design.md/blob/main/docs/spec.md> and `https://raw.githubusercontent.com/VoltAgent/awesome-design-md/main/design-md/<slug>/DESIGN.md`.

## Ground rules

1. **User requirements and repository instructions outrank DESIGN.md.** The contract records decisions. It does not override the user.
2. **Never overwrite an existing DESIGN.md blindly.** Read it first, show the intended delta, then edit only what the task covers. Keep its format unless the user asks for a migration.
3. **Evidence before taste.** Before proposing values, inspect the existing UI, theme files, Tailwind/CSS variables, component library, fonts actually loaded, and dark mode support. Derive tokens from what ships, then improve deliberately.
4. **Read the spec, don't recall it.** The format is alpha and changes. Read `references/design.md/spec.md` (or run `npx @google/design.md spec --rules`) before writing tokens.
5. **Audits are read-only** unless the user asked for fixes.
6. **Catalog entries are references, not brands to clone.** Never copy logos, brand names, product copy, or proprietary fonts into the target project.

## Pick the mode

| User intent | Mode |
|---|---|
| "Make a DESIGN.md", no existing file | **Create** |
| "Make it look like X", "pick a style" | **Adopt** (then Create or Update) |
| Change colors/type/components in the contract | **Update** |
| "Does this UI follow DESIGN.md?" / review UI | **Audit** |
| Build UI in a repo with DESIGN.md | **Implement** |
| Tailwind / CSS variables / design tokens file | **Export** |

### Create

1. Survey the project (rule 3). Note the product type, audience, density, platforms, light/dark, and existing tokens.
2. Read `references/design.md/spec.md`. Copy `assets/DESIGN.template.md` to the repo root as `DESIGN.md`.
3. Fill tokens from evidence: `colors` (must include `primary`), `typography`, `rounded`, `spacing`, and `components` that reference other tokens via `{colors.primary}`-style references. Wire every color into at least one component, otherwise lint flags it as orphaned.
4. Write the prose sections in spec order: Overview, Colors, Typography, Layout, Elevation & Depth, Shapes, Components, Do's and Don'ts. Omit sections that don't apply rather than padding them. The prose should say *when* to use each token, not repeat values.
5. Lint (see **Validate**). Fix all errors and justify any remaining warnings to the user.

### Adopt a style from the catalog

1. Index the catalog cheaply. Each entry is around 500 lines, so don't read them whole. Run `grep -m1 '^description:' references/awesome-design-md/*/DESIGN.md` to get a one-paragraph summary per product (most entries have one; for the rest, read the first `## Overview` paragraph). Slugs are product names, e.g. `linear.app`, `stripe`, `notion`, `vercel`, `apple`.
2. Shortlist 2–3 by product fit: category, density, mood, light/dark coverage, and CJK or multilingual typography needs. For the shortlist only, read the frontmatter tokens and the `## Overview` and `## Colors` sections.
3. Present the shortlist with one line on what each contributes, and let the user choose. If they already named a product, skip to step 4.
4. Adopt **principles**, not identity: hierarchy, density, type roles, palette strategy (e.g. "neutral canvas + single accent"), surface and elevation model, radius language. Re-derive concrete values for the target product: swap brand hues, substitute available fonts, recheck contrast.
5. Use one primary reference. Add a second only to fill a specific gap (e.g. data-table density), and never average conflicting token values.
6. Record the adopted references in the Overview prose ("Hierarchy and density adapted from …").

### Update

1. Read the current DESIGN.md and copy it to a temp file as the baseline.
2. Make the requested edits, keeping token names stable. Renaming tokens breaks references in code.
3. Run `diff` against the baseline (see **Validate**) and report added, removed, and modified tokens and any regression.
4. If tokens were exported to code (Tailwind config, CSS variables), re-export and update those files too, within the authorized scope.

### Audit

1. Read DESIGN.md and lint it first. A broken contract makes the audit meaningless.
2. Scope the audit to the changed files or screens the user named.
3. Check for: hardcoded colors or sizes that should be tokens, off-palette colors, wrong type roles, radius or spacing off-scale, component variants missing from the contract, and contrast failures.
4. Report findings as `file:line → expected token → actual value`, grouped by severity. Do not edit.

### Implement

Read DESIGN.md before writing UI. Use the project's token layer (CSS variables, Tailwind theme, or theme object) instead of literal values. If the task needs a value the contract lacks, stop and propose a contract update rather than inventing one silently.

### Export

```bash
npx @google/design.md export --format css-tailwind DESIGN.md > <tmp>/theme.css     # Tailwind v4 @theme
npx @google/design.md export --format json-tailwind DESIGN.md > <tmp>/tailwind.json # Tailwind v3 theme.extend
npx @google/design.md export --format dtcg DESIGN.md > <tmp>/tokens.json           # W3C design tokens
```

Export to a temp path first, inspect it, then move it over the project file only when that replacement was authorized.

## Validate

```bash
npx @google/design.md lint DESIGN.md                 # exit 1 on errors
npx @google/design.md diff before.DESIGN.md DESIGN.md # exit 1 on regression
npx @google/design.md spec --rules                    # current rules, if unsure
```

- On Windows or PowerShell, use `npx -p @google/design.md designmd lint DESIGN.md`. The `.md` in the binary name collides with the file association.
- Prefer a project-installed version if `package.json` has one. Don't add a dependency just to lint.
- Errors (`broken-ref`, duplicate sections or keys) must be fixed. Warnings (`missing-primary`, `contrast-ratio`, `orphaned-tokens`) must be fixed or explicitly justified.
- If the CLI can't run (offline, registry issues), check manually against the spec and tell the user what was not verified.

## Output to the user

End with: what changed in DESIGN.md (or the audit findings), the lint summary (errors, warnings), which catalog references were adopted, if any, and anything left unverified.
