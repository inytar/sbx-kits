# claude-theme-auto

A tiny [Claude Code](https://docs.claude.com/en/docs/claude-code/overview) mixin for
Docker Sandboxes that pins the theme to `auto` (match terminal), so every session in
the sandbox starts with a theme that follows the terminal's own light/dark mode
instead of the installed default.

The kit merges `{"theme": "auto"}` into `~/.claude/settings.json` — every other
setting (including another kit's, such as `claude-sbx-statusline`'s `statusLine` key)
is preserved. If a `theme` key already exists, it is replaced with `auto`, since
setting the theme is the point of installing the kit.

## Quick start

Not published as an OCI artifact — pull it straight from this repo over git (pinned by ref):

```console
sbx run claude --kit "git+https://github.com/inytar/sbx-kits.git#dir=claude-theme-auto" .
```

Or with a local clone of this repo:

```console
sbx run claude --kit ./claude-theme-auto .
```

Combine with other mixins by repeating `--kit`:

```console
sbx run claude \
  --kit "git+https://github.com/inytar/sbx-kits.git#dir=claude-theme-auto" \
  --kit "git+https://github.com/inytar/sbx-kits.git#dir=claude-sbx-statusline" \
  .
```

## How it works

The `install` hook (run as root) merges the `theme` key into `~/.claude/settings.json`
with `jq`:

```json
{ "theme": "auto" }
```

It creates the file if missing and leaves every other key untouched — only `theme` is
set (replacing a prior value if present) — then `chown`s back to the `agent` user
exactly the paths it touched: `~/.claude` and `~/.claude/settings.json`. Re-running is
idempotent. The temp file is created inside `~/.claude` so the final `mv` is an atomic
same-filesystem rename rather than a cross-device copy.

## Requirements

The install merge uses `jq`, which is present on the `claude-code` base image.
