# pod-waybar-labwc

The `waybar-labwc` candy of the OpenCharly candy library, as a standalone repo
(the candy de-submodule cutover, kind-prefixed naming). It provides a bottom
Waybar status bar wired for the labwc desktop session, launched once labwc's
Wayland socket is ready.

## What it provides

Installs the `waybar` package and drops a labwc-specific launch wrapper plus a
bottom-bar config (wlr taskbar, Chrome launcher, swaync notification module) and
a matching style sheet into the user's home. A supervisord service runs the
wrapper, which waits for labwc's `wayland-0` socket before exec'ing waybar so the
bar attaches to the labwc compositor, not the pixelflux capture display.

| Property | Value |
|---|---|
| Requires | `pod-labwc` |
| Service | `waybar` (`~/.local/bin/waybar-labwc-wrapper`, `restart: always`, priority 15) |
| Install files | `waybar-labwc-wrapper`, `config.json`, `style.css` |
| Package | `waybar` (RPM/pac) |

Connecting to `wayland-0` (not pixelflux's `wayland-1`) is what gives waybar
layer-shell exclusive zones and the `wlr-foreign-toplevel` taskbar.

## How to use it

Composed by the `selkies-desktop` metalayer:

```yaml
my-desktop:
  candy:
    - '@github.com/opencharly/pod-waybar-labwc:<tag>'
```

## Verification

The candy's `check:` plan asserts `/usr/bin/waybar`, the launch wrapper, that the
wrapper attaches to `wayland-0`, the staged config (wiring `wlr/taskbar`), the
style sheet, and the `waybar` package.

## Layout

- `charly.yml` — the `waybar-labwc:` candy entity (description, `require`,
  `distro`, `service`, `plan`) plus its `skill:` entity.
- `waybar-labwc-wrapper`, `config.json`, `style.css` — the staged launcher,
  config, and style.
- `.github/workflows/tag-on-merge.yml` — CalVer tag + `CHANGELOG/` on merge.
- `README.md` — this user overview.

## Related

- Owning skill: `/charly-selkies:waybar-labwc` — the candy properties, the
  `wayland-0` socket pinning, and the shared unified config.
- `/charly-selkies:waybar` — the sway-native sibling with the same config.
- `/charly-selkies:labwc` — the compositor this candy targets.
- [`opencharly/charly`](https://github.com/opencharly/charly) — the charly CLI and image builder
