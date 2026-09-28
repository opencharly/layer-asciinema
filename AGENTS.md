# AGENTS.md — layer-asciinema

Standalone candy repo for the `asciinema` layer. The candy lives in `charly.yml`
at the repo root: the `asciinema` distro package, the per-distro DejaVu font map,
the pinned `agg` binary download, an ordered `plan:` of build-time `check:` steps,
and the embedded `skill:` entity projected into the marketplace corpus as
`/charly-coder:asciinema`. There is no source tree and no service of its own.

Canonical files:

- `charly.yml` — the `asciinema:` candy entity and the `asciinema-skill:` skill entity.
- `.github/workflows/deploy.yml` — the manifest gate.
- `.github/workflows/tag-on-merge.yml` — CalVer tag + `CHANGELOG/` on merge.
- `README.md` — user overview only; never agent guidance.

## Load these skills first (R0)

- `/charly-coder:asciinema` — the owning skill. What the candy installs, the
  `record:` verb integration, and the agg GIF rendering options. Load before
  editing or troubleshooting the candy.
- `/charly-check:record` — the `record:` check verb (`record_mode: terminal`
  records, `record: gif` renders) that this candy backs. Load when touching the
  recorded behaviour or its declarative usage.
- `/charly-image:layer` — the candy authoring reference (`charly.yml` schema,
  `plan:` step verbs incl. `check:`, `download:`, package sections, per-distro
  `package_map`). Load before editing any entity field or plan step.

## Build / validate / test

- `charly box validate` at the repo root — the same structural gate CI runs: the
  manifest must parse and validate at the pinned charly. The CI pin lives in
  `.github/workflows/deploy.yml`; keep the `version:` schema stamp within the
  pinned charly's supported range (do not migrate the stamp past the pin).
- `.github/workflows/deploy.yml` — builds the pinned charly from a CI-time
  checkout and runs `charly box validate`. This is the merge gate.
- The candy's `plan:` asserts the binaries and exercises `agg` on a minimal
  asciicast (proving both the binary and the monospace font work); the live
  `record:` paths are proven by the owning skill's check beds.

## Modify this repo

- Edit the `asciinema:` candy entity AND the `asciinema-skill:` skill entity in
  `charly.yml` together. The skill is the projected usage source, so a package or
  behaviour change that is not mirrored in the skill leaves the corpus stale.
- Package changes go under `package:` / `distro:`; the `agg` version is a `var:`
  consumed by the `download:` step. Behaviour claims go in `plan:` as observable
  `check:` steps.
- Keep `version:` at the schema stamp the pinned CI charly supports.

## Landing

- PR-only. Every change lands through a pull request; the org-required
  `charly/pr-validator` validates the diff and body and arms native auto-merge
  on PASS. Direct pushes to `main` are blocked.
- History lives in `CHANGELOG/` (written by `tag-on-merge` at merge time); the
  PR body IS the changelog.
- The authoritative rulebook is the umbrella `AGENTS.md` in
  `opencharly/opencharly` and `charly/AGENTS.md` in the charly repo. Do not
  restate its rules here.
