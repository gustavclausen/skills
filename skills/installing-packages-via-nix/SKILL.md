---
name: installing-packages-via-nix
description: Use when about to install a CLI tool, library, or package on the user's machine — before running brew install, npm install -g, pip install --user, apt/pipx, or any other ad-hoc package manager command
---

# Installing Packages via Nix

## Overview

The user manages their machine declaratively through a personal Nix flake config repo (home-manager + nix-darwin/NixOS). Every package install goes through that repo — never through `brew`, `npm install -g`, `pip install --user`, `pipx`, `apt`, or any other ad-hoc manager, even "just this once" or "just to experiment."

**Why:** Ad-hoc installs aren't tracked, aren't reproducible across machines, and drift from the declarative config.

## When to use

Any time you're about to add a CLI tool, language runtime, or library to a machine the user uses — whether they asked directly ("install X") or it's a means to an end (a task needs a tool that isn't present).

## How to do it

Locate the repo — if you're Claude Code, check memory first (a reference memory entry records its path per machine); If you can't find it, ask where it lives rather than falling back to another package manager.

For where to add the package, which command builds/verifies, and which command activates (and who runs it), follow that repo's own `CLAUDE.md` / `AGENTS.md` — don't rely on this skill for those mechanics, and don't guess if the repo docs are silent on something; ask.

Install for the current machine's host config specifically — use the repo's documented layout (e.g. shared vs. per-host modules) to place the package correctly rather than guessing or defaulting to the first list you find. If it's unclear which host config applies to the current machine, ask.

## Rationalizations to reject

| Excuse | Reality |
|--------|---------|
| "It's faster with brew" | The Nix edit takes about the same time and stays reproducible. |
| "It's just for a quick experiment" | Experiments still drift the system if installed ad-hoc; add it to nix-config, remove it later if unneeded. |
| "This tool isn't in nixpkgs" | Check first (`nix search nixpkgs <name>` or search.nixos.org) — most CLI tools are. If it's genuinely absent, ask the user how they want to handle it rather than defaulting to another manager. |
| "Activation needs sudo I can't supply" | That's fine — verify per the repo's own docs, then hand the activation command to the user. It's not a reason to use brew/npm/pip instead. |
