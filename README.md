# openclaw-telegram-projects-ux

Telegram-first, plugin-scoped **Projects UX** for OpenClaw.

This repository is intentionally **out-of-tree** (not part of `openclaw/openclaw`).

## What this is

OpenClaw’s Telegram DMs don’t have reliable native threading or durable conversation boundaries.
This plugin provides an **opt-in** Projects UX that lets you segment a single Telegram DM into user-defined “projects”, and switch between them explicitly.

A **project** is a plugin-scoped unit of context that provides:
- project-scoped context/memory as implemented by the plugin (**not a security boundary**)
- explicit switching between contexts via `/projects`
- ability to turn Projects ON/OFF per DM
- a default project intended to approximate the user’s classic single-thread chat flow

This is implemented entirely at the plugin layer and is opt-in. If you never enable it, nothing changes.

## Lifecycle management (safe operations)

Once projects accumulate, cleanup/recovery needs to be safe and user-facing.
This plugin adds guarded lifecycle operations for the current Telegram DM/peer:

- **Remove (wipe) ONE project** (non-default)
- **Wipe ALL projects state** (recovery)
  - Resets Projects UX state for that DM and forces **Projects OFF (Classic)**

## Safety / constraints

- Two-step confirmation with a single-use nonce and short expiry (~120s)
- Automatic backup before any mutation of plugin-owned state
- Destructive actions are per-peer and limited to plugin-owned state under:
  - `~/.openclaw/projects-ux/`
- Default project is protected and cannot be removed
- No transcript/history deletion
- No modifications to repos under `~/projects/`

## Contents

- `extensions/projects-ux/` — the plugin, copied as-is.

## Status

See `UPSTREAM_STATUS.md` for upstream context and links.
