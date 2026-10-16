# sbx-kits

Personal [Docker Sandboxes](https://docs.docker.com/ai/sandboxes/) kits for `claude`.

- **[claude-sbx-statusline](claude-sbx-statusline)** — a fork of
  [docker/sbx-kits-contrib](https://github.com/docker/sbx-kits-contrib)'s kit of the same name,
  with a reasoning-effort segment and colours that read on both light and dark backgrounds.
- **[claude-theme-auto](claude-theme-auto)** — pins Claude Code's theme to `auto` (match
  terminal) via `~/.claude/settings.json`.

Each kit is a standalone `kind: mixin` — see its own README for what it does and how to use it
with `sbx run claude --kit ...`.
