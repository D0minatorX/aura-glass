# Contributing

Thanks for looking at Aura Glass. This is the short version. The long version
starts at [docs/README.md](docs/README.md).

## Set up

```bash
git clone https://github.com/DevWebeloper/aura-glass.git && cd aura-glass
tools/install-hooks.sh     # once per clone: the pre-commit hook runs the check suite
./install.sh               # you develop against a real install
```

You need a GNOME 48–51 desktop (Wayland preferred), bash, python3 and git. For
`gui/` you also need PyGObject, libadwaita and pycairo.

## The rules

1. **One resolver.** `install.sh` plus the `apply_*` functions in `lib/` are the only code that turns a flag into a file. The settings app, wizards, preview and adaptive worker call it.
2. **Never hand-edit `~/.themes` or `~/.config/gtk-*`.** They're generated. Edit `css/`.
3. **Duplicated numbers are checked, not trusted.** Change `tokens/tokens.sh` and every consumer it names, then run `tools/check-tokens.sh`.
4. **Comments that say why a number is that number are the deliverable.** Keep them true.
5. **Apply your change to the live desktop** (`./install.sh --settings-only -y`) and look at it before calling it done.
6. **Keep every path consistent.** See the checklist in [docs/CONSISTENCY.md](docs/CONSISTENCY.md) §4.

## Before you open a PR

- `git commit` runs the hermetic suite against the staged tree. Stage explicitly, not with `git add -A`, because unrelated work is often in progress.
- For visual changes, run `tools/preview.sh && python3 tools/check-shots.py --mode glass` and include before/after images.
- If you fixed something from [docs/KNOWN-ISSUES.md](docs/KNOWN-ISSUES.md), move it to **Fixed**.

## Commits and branches

- `type(scope): lower-case sentence, no trailing period`, e.g. `fix(modes): save the drawer after an incremental apply`.
- Feature branch → `git merge --no-ff` into `main`. Releases are tags ([docs/RELEASING.md](docs/RELEASING.md)).
- **No AI attribution** (no `Co-Authored-By` trailers or "generated with" lines).

## Scope

Do the change you came for. Unrequested validation, defensive branches and
drive-by refactors cost more to review than they save. If you disagree with a
decision a comment records, say so in the PR rather than changing it silently.

## Reporting a bug

Include:

- `gnome-shell --version`, session type (Wayland or X11) and distro
- the output of `./install.sh --dry-run` (it prints the resolved `glass-mode:` line)
- `ls -R ~/.config/aura-glass` (the memos are what the installer thinks you chose)
- for visual bugs, a screenshot, and whether `aura-glass-apply` alone fixes it
- whether it happened after Apply in the settings app, a mode switch, or an adaptive switch (these are different code paths)

## Licence

MIT for the scripts, stylesheets, patches and tooling here ([LICENSE](LICENSE)).
Upstream themes, extensions, packs and fonts keep their own licences.
