# asciinema

Terminal session recording and GIF rendering for OpenCharly images.

The `asciinema` candy installs the [asciinema](https://asciinema.org) terminal
recorder and the [agg](https://github.com/asciinema/agg) GIF generator. The
`/usr/bin/asciinema` CLI captures a terminal session — timing, input, and output —
into a replayable asciicast v2 `.cast` file, and `/usr/local/bin/agg` renders a
stopped `.cast` into an animated GIF. It is the engine behind the `record:` check
verb's terminal and GIF modes (served out-of-process by `candy/plugin-record`).
No service and no daemon: the candy is a package plus one pinned binary download.

## What it provides

| Property | Value |
|---|---|
| Layer / candy | `asciinema` |
| Package | `asciinema` (RPM / DEB / PAC) |
| Binaries | `/usr/bin/asciinema`, `/usr/local/bin/agg` (pinned v1.9.0) |
| Font | DejaVu Sans Mono (per-distro: `dejavu-sans-mono-fonts` / `ttf-dejavu` / `fonts-dejavu-core`) |
| Requires | `candy/plugin-record` (the `record:` verb) |
| Service / port | none |

## How to use it

Compose the layer by pinning this repo in a box's `candy:` list. The named
entity is a box: its `candy:` value is the box BODY (holding `base:` and the
nested composition `candy:` list):

```yaml
my-box:
  candy:
    base: fedora
    candy:
      - '@github.com/opencharly/layer-asciinema:v2026.246.1622'
```

Then, inside the built image:

```bash
asciinema rec demo.cast        # record a terminal session
asciinema play demo.cast       # replay it
agg demo.cast demo.gif         # render it to an animated GIF
```

The `record:` check verb drives the same binaries declaratively — `record: start`
with `record_mode: terminal` records, and `record: gif` renders:

```yaml
plan:
  - check: a terminal recording starts
    record:
      method: start
      record_name: demo
      record_mode: terminal
    context: [deploy]
  - check: the recording is rendered to an animated gif
    record:
      method: gif
      record_name: demo
      artifact: demo.gif
      theme: monokai
      speed: 2
    context: [deploy]
```

## Layout

- `charly.yml` — the candy manifest: the `asciinema` package, the per-distro
  DejaVu font map, the pinned `agg` download, an ordered `plan:` of build-time
  `check:` steps, and the embedded `skill:` entity.
- `.github/workflows/deploy.yml` — builds the pinned charly and runs
  `charly box validate` on the manifest (the merge gate).
- `.github/workflows/tag-on-merge.yml` — CalVer tag + `CHANGELOG/` on merge.
- `README.md` — this user overview.

## Related

- Owning skill: `/charly-coder:asciinema`
- Recording verb: `/charly-check:record`
- Bundled by: `/charly-coder:dev-tools`
- [`opencharly/charly`](https://github.com/opencharly/charly) — the charly CLI and image builder
- [`opencharly/opencharly`](https://github.com/opencharly/opencharly) — the umbrella
