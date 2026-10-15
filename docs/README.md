# Aura Glass: contributor documentation

This folder is for people (and AI agents) who change Aura Glass. If you just
want to use it, see the top-level [README](../README.md).

---

## What this project is

Aura Glass turns a stock GNOME 48–51 desktop into one coherent frosted-glass
desktop. It does this without a build system or a package. It's a Bash
installer that:

1. fetches **pinned** upstreams (a base theme, Blur My Shell, Custom OSD, optional icon, pointer and font packs),
2. layers its own **stylesheets**, **dconf presets** and **patches** on top,
3. installs a **first-party GNOME extension**, a few **user systemd services** and a **GTK settings app**,
4. remembers every choice as a small **memo file** under `~/.config/aura-glass`, so the next run gives the same result.

## What it is for

The goal is a desktop that looks like one material everywhere: shell, GTK4 and
GTK3, painted corners and blurred corners. It should stay that way across
re-runs, upgrades and mode switches, and come back out cleanly.

Five properties define "stable" for this project:

| Property | Meaning | Defended by |
|---|---|---|
| **Consistent** | Every front end (CLI, two wizards, settings app, live preview, adaptive worker, window menu) produces the same result for the same choice. | a single resolver plus parity checks ([CONSISTENCY.md](CONSISTENCY.md)) |
| **Deterministic** | A flagless re-run reproduces what was last applied. | memos and per-mode drawers ([CONFIG-REFERENCE.md](CONFIG-REFERENCE.md)) |
| **Reversible** | Everything written is either inside a marked block or backed up first. | `backup_once`, marked splice blocks, `uninstall.sh` |
| **Rootless by default** | Only three opt-in steps need root. | the step layout in `install.sh` |
| **Serialized** | Only one project writer at a time, and each CSS file is published atomically. | `aura-glass-operation` lock, atomic publish in `aura-glass-apply` |

When you fix a bug, you're almost always restoring one of these five.

---

## Reading order

| If you want to… | Read |
|---|---|
| Understand how it works end to end | [ARCHITECTURE.md](ARCHITECTURE.md) |
| See every file, what it is, where it gets installed, who calls it | [STRUCTURE.md](STRUCTURE.md) |
| Find the files behind a visual feature or a setting | [FEATURE-MAP.md](FEATURE-MAP.md) |
| Look up a flag, memo, path, env var or dconf/gsettings key | [CONFIG-REFERENCE.md](CONFIG-REFERENCE.md) |
| Keep the settings app, wizards and installer in agreement | [CONSISTENCY.md](CONSISTENCY.md) |
| See the bugs found so far, with reproductions | [KNOWN-ISSUES.md](KNOWN-ISSUES.md) |
| Make a change or fix a bug | [DEVELOPING.md](DEVELOPING.md) |
| Run the checks, or add one | [TESTING.md](TESTING.md) |
| Touch either GTK window | [GUI.md](GUI.md) |
| Cut a release | [RELEASING.md](RELEASING.md) |
| Work on this as an AI agent | [../CLAUDE.md](../CLAUDE.md) (and [../AGENTS.md](../AGENTS.md)) |

## Orientation in ten lines

```
install.sh            flags in → one resolver → steps out (full install or --settings-only retune)
lib/*.sh              the steps, one file per concern; definitions only
tokens/tokens.sh      every number written in more than one place (radii, sigmas, transparency)
css/                  stylesheets; the NN- prefix IS the cascade order
dconf/                Blur My Shell + Custom OSD presets (core), extras, opaque overrides (solid.ini)
bin/                  commands installed to ~/.local/bin (apply, settings, preview, ext, operation, workers)
gui/                  setup wizard + settings app: front ends that only ever produce install.sh flags
extensions/           the first-party GNOME extension (Aura Glass Helper)
tools/                runtime helpers (CSS rewriters, workers) + hermetic checks + live-desktop harnesses
uninstall.sh          reverses it, in scopes
```

## Other material in this folder

- `superpowers/specs/` and `superpowers/plans/` are **historical design records**
  (glass modes, stable apply, adaptive performance, Moga cursors). They capture
  the reasoning at the time and aren't kept current. Note that the glass-modes
  spec describes three modes that have since become two. The code and the
  documents above are the current truth.
- `pull-requests/` holds drafts of fixes offered back to upstream (Blur My Shell, Copyous).
