---
name: storing-personal-skills
description: Use when about to create a new personal skill, edit an existing one, or decide where a SKILL.md file should be written or looked for on this machine
---

# Storing Personal Skills

## Overview

On some of the user's machines, a runtime's local skills directory (`~/.claude/skills/`, `~/.codex/skills/`, `~/.agents/skills/`, etc.) is a symlink into the Nix store, generated declaratively by a personal Nix flake config repo. Writes there are silently discarded on the next rebuild — nothing is actually saved.

**Why:** The real source of truth is the user's own personal skills Git repo, which the Nix config syncs out to every runtime's local directory on every machine. Editing the generated copy edits nothing durable.

## How to do it

Before writing or editing any personal skill, check whether the current directory's skills folder is a symlink:

```bash
readlink ~/.claude/skills/<name>   # or the equivalent path for the current runtime
```

- **Not a symlink** (plain file/dir, or doesn't exist yet as a symlink): this machine isn't managed this way — write/edit directly here.
- **Resolves into a Nix store path, and it's the user's own skill**: don't edit it. Locate the personal skills repo instead — check memory first (a reference memory entry records its path per machine); if you can't find it, ask where it lives rather than guessing or falling back to editing the store copy.
- **Resolves into a Nix store path, but the skill comes from an upstream source the user doesn't own** (e.g. a plugin): editing it isn't possible here at all. Say so; don't silently work around it (e.g. by writing a competing local override) or give up quietly.

Once you have the repo path, follow its own `CLAUDE.md`/`AGENTS.md` for where within it new skills go (e.g. a `skills/` subdirectory) and how it gets synced — don't guess the internal layout if the repo's own docs are silent on it; ask.

This overrides any other skill's own instructions about where personal skills live (e.g. `writing-skills` naming a runtime's local directory) — that directory is the synced *destination* here, not the place to author from.

## Rationalizations to reject

| Excuse | Reality |
|--------|---------|
| "The other skill said to write to the runtime's skills directory" | That guidance is generic to all Claude Code users; on a Nix-managed machine that path is read-only. This skill's guidance wins here. |
| "It's just a quick edit, I'll fix the source later" | The store symlink is regenerated wholesale on rebuild; there's no "later" — the edit is already gone. |
| "I'll write directly to the store path since it looks writable" | Store paths are content-addressed and read-only by design; don't attempt to write through them. |
| "I don't know the repo path, I'll just guess `~/dev/skills` or similar" | Check memory or ask — a wrong guess can silently write into the wrong place. |
