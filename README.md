# skills

**English** | [简体中文](README.zh-CN.md)

A collection of skills for Claude Code and Codex. Each skill lives in its own directory under `skills/<name>/`, and the directory name matches the `name` in its frontmatter. `SKILL.md` is the entry point, and reference files are loaded only when needed. The repository itself is also a Claude Code plugin.

## Skills

| Skill | Description | Source | License |
|---|---|---|---|
| [design-md](skills/design-md/) | Maintains a repository-root DESIGN.md visual contract: create, adopt a style from the catalog, update, lint, audit, and export to Tailwind/DTCG. Bundles the Google format spec and 84 style references | **Own**, bundling [google-labs-code/design.md](https://github.com/google-labs-code/design.md) and [VoltAgent/awesome-design-md](https://github.com/VoltAgent/awesome-design-md) | MIT (bundled references keep Apache-2.0 / MIT) |
| [impeccable](skills/impeccable/) | Frontend interface design, critique, polish, and audit | [pbakaus/impeccable](https://github.com/pbakaus/impeccable) | Apache-2.0 |
| [design-taste-frontend](skills/design-taste-frontend/) | Art direction for landing pages and websites that don't look AI-generated | [Leonxlnx/taste-skill](https://github.com/Leonxlnx/taste-skill) | MIT |
| [archify](skills/archify/) | Architecture, workflow, sequence, ERD, and other diagrams as interactive HTML | [tt-a1i/archify](https://github.com/tt-a1i/archify) | Apache-2.0 |
| [brooks-review](skills/brooks-review/) | PR code review | [hyhmrright/brooks-lint](https://github.com/hyhmrright/brooks-lint) | MIT |
| [brooks-audit](skills/brooks-audit/) | Architecture and module dependency audit | same | MIT |
| [brooks-debt](skills/brooks-debt/) | Tech debt assessment and refactoring priorities | same | MIT |
| [brooks-test](skills/brooks-test/) | Test suite quality review | same | MIT |
| [brooks-health](skills/brooks-health/) | Combined score across the four dimensions above | same | MIT |
| [brooks-sweep](skills/brooks-sweep/) | Full sweep that diagnoses and applies fixes | same | MIT |

`skills/_shared/` is not a skill. It holds reference files shared by the brooks skills, which they read through the relative path `../_shared/`.

## Install

**Claude Code** (whole collection):

```
/plugin marketplace add draco-china/skills
/plugin install draco-skills@draco-skills
```

**Codex and other agents** (all skills, or pick one):

```bash
npx skills add draco-china/skills
```

```bash
npx skills add draco-china/skills --skill design-md
```

**Manual**: clone the repo and symlink a skill directory into `~/.claude/skills/`, or `~/.agents/skills/` for Codex:

```bash
ln -s "$PWD/skills/design-md" ~/.claude/skills/design-md
```

**brooks skills outside Claude Code**: `npx skills add` installs only directories that contain a `SKILL.md`, so it skips `_shared` and the brooks skills can't find their reference files. For Codex, symlink them by hand together with `_shared`:

```bash
for d in _shared brooks-audit brooks-debt brooks-health brooks-review brooks-sweep brooks-test; do
  ln -s "$PWD/skills/$d" ~/.agents/skills/$d
done
```

Or use the [upstream installer](https://github.com/hyhmrright/brooks-lint#installation). The Claude Code plugin install above already includes `_shared`.

## Upstream sync

Apart from the parts of `design-md` written in this repo (`SKILL.md`, `assets/`, `agents/`), everything is a byte-for-byte mirror of an upstream. [sync.yml](.github/workflows/sync.yml) syncs it daily at 02:00 UTC and commits straight to main:

- [upstreams.json](upstreams.json) defines what to sync: the upstream repo, branch, an `upstream path → local path` map, and an optional `include` file filter.
- [skills.lock.json](skills.lock.json) records the upstream SHAs currently mirrored. CI generates it.
- Each upstream's LICENSE is copied into every directory mirrored from it.
- If one upstream fails, it keeps its previous version and the others still sync; the run is then marked failed so the problem shows up.
- Checks run before every sync commit, including a lint of the design-md template against the latest DESIGN.md spec.
- The plugin has no fixed `version`, so Claude Code treats each commit as a new version and `/plugin update` picks up every sync.

**Don't edit mirrored directories here.** The next sync overwrites them. Send improvements upstream instead.

To add an upstream, add an entry to `upstreams.json`. Pushing that change triggers a sync, or you can run `sync-upstream` manually from the Actions tab.

## Conventions

- Directory name == frontmatter `name`, enforced by [check.yml](.github/workflows/check.yml).
- No machine-specific paths such as `/Users/...`.
- Commands and APIs in skills written here must be verified, never written from memory.

## License

The repository and the skills written here are [MIT](LICENSE). Mirrored directories follow their upstream licenses; see the LICENSE file in each.
