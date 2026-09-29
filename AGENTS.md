# AGENTS.md — pod-waybar-labwc

Standalone candy repo for the `waybar-labwc` candy — a bottom Waybar status bar
wired for the labwc desktop session, launched once labwc's Wayland socket is
ready. The candy lives in `charly.yml` at the repo root plus its launcher,
config, and style.

Canonical files:

- `charly.yml` — the `waybar-labwc:` candy entity (description, `require`,
  `distro`, `service`, `plan`) and its `skill:` entity.
- `waybar-labwc-wrapper` — the launcher copied into `~/.local/bin`.
- `config.json`, `style.css` — the staged `~/.config/waybar` config and theme.
- `.github/workflows/tag-on-merge.yml` — CalVer tag + `CHANGELOG/` on merge.
- `CHANGELOG/` — per-CalVer history.
- `README.md` — user overview only; never agent guidance.

## Load these skills first (R0)

- `/charly-selkies:waybar-labwc` — the owning skill: the candy properties, the
  `wayland-0` socket pinning, and the shared unified config. Load before editing,
  building, deploying, or troubleshooting this candy.
- `/charly-selkies:waybar` — the sway-native sibling with the same config.
- `/charly-selkies:labwc` — the compositor this candy targets.
- `/charly-selkies:selkies-desktop-layer` — the metalayer that composes this
  candy.
- `/charly-image:layer` — the candy authoring reference (`charly.yml` schema,
  `plan:` step verbs incl. `copy:` / `check:`, service declarations).
- `/charly-check:check` — the check/R10 framework (`charly check box`,
  `charly check run <bed>`).
- `/charly-pod:pod` — the `kind: pod` / deploy schema reference (this candy is
  composed into a box; services).
- `/charly-internals:git-workflow` — before any git/PR action.

## Build / validate / test

- `charly box validate` at the repo root — the structural check: the manifest
  must parse and validate at the installed charly.
- The merge gate is the **org-wide** `charly/pr-validator` (required check
  `validate / validate`, defined in `opencharly/.github`); this repo has **no**
  per-repo candy gate. Its only workflow file is
  `.github/workflows/tag-on-merge.yml`.
- The candy's own `check:` steps assert `/usr/bin/waybar`, the launch wrapper,
  its `wayland-0` attachment, the staged config (`wlr/taskbar`), the style sheet,
  and the `waybar` package.

## Modify this repo

- Edit the `waybar-labwc:` candy entity in `charly.yml`; the `skill:` entity in
  the same file is the owning skill's source — a candy change and its skill
  change land together.
- Keep the wrapper pinned to `wayland-0` (labwc's socket), **not** pixelflux's
  `wayland-1`; the check asserts the `wayland-0` string.
- `config.json` / `style.css` are the unified config shared with the `waybar`
  candy; keep them in step across the two candies.
- The `skill:` entity is the source for `/charly-selkies:waybar-labwc`; never
  edit the generated `SKILL.md` — regenerate it.

## Landing

- PR-only. Every change lands through a pull request; the org-required
  `charly/pr-validator` validates the diff and body and arms native auto-merge on
  PASS. Direct pushes to `main` are blocked.
- History lives in `CHANGELOG/` (written by `tag-on-merge` at merge time); the PR
  body IS the changelog.
- The authoritative rulebook is the umbrella `AGENTS.md` in
  `opencharly/opencharly` and `charly/AGENTS.md` in the charly repo. Do not
  restate its rules here.
