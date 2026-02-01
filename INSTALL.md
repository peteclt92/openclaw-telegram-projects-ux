# Installation

This is an **out-of-tree** extension (not shipped with `openclaw/openclaw`).
It is **Telegram-first** and was validated in Telegram DMs.

## Prerequisites

- A working OpenClaw install with the Telegram channel configured.
- A local clone of this repo.

## Option A (recommended): install as a Global Extension

OpenClaw auto-discovers extensions from:

- `~/.openclaw/extensions/*/index.ts`

Steps:

```bash
# from anywhere
mkdir -p ~/.openclaw/extensions

# copy the plugin directory (contains openclaw.plugin.json)
cp -a /path/to/openclaw-telegram-projects-ux/extensions/projects-ux ~/.openclaw/extensions/projects-ux

# restart to load plugin code
openclaw gateway restart

# verify it is loaded
openclaw plugins list | grep projects-ux || true
```

## Option B: load via config (plugins.load.paths)

If you prefer not to copy files into `~/.openclaw/extensions`, point OpenClaw at this repo:

```bash
# edit your OpenClaw config
openclaw config edit
```

Add the plugin path:

```json5
{
  plugins: {
    load: {
      paths: ["/path/to/openclaw-telegram-projects-ux/extensions/projects-ux"]
    }
  }
}
```

Then restart:

```bash
openclaw gateway restart
```

## Enabling / configuring

Depending on your OpenClaw config defaults, you may need to enable the plugin explicitly.
If it appears in `openclaw plugins list` but is disabled:

```bash
openclaw plugins enable projects-ux
openclaw gateway restart
```

Plugin config is typically under:

- `plugins.entries.projects-ux.config`

(See `extensions/projects-ux/openclaw.plugin.json` for schema defaults.)

## Quick verification (manual)

In a Telegram DM with your bot:

1. Send `/projects` and confirm you see a **More…** button.
2. Send `/projects on`.
3. Create a project: `/projects new test-a`.
4. Store a project-scoped sentinel: `/memory remember cobalt`.
5. Create/switch to another project: `/projects new test-b`.
6. Confirm isolation by listing tokens: `/memory list` (should not show `cobalt`).
7. Go to `/projects` → **More…** and confirm **Reset…** is present.

## Uninstall / disable

- Removing the extension directory disables the UX:
  - delete `~/.openclaw/extensions/projects-ux/` (or remove it from `plugins.load.paths`)
  - then run `openclaw gateway restart`

Plugin-owned state lives under:

- `~/.openclaw/projects-ux/`

You may back it up or remove it manually. This plugin does **not** delete transcripts/history.
