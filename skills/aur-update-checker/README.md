# aur-update-checker

A [Pi coding agent](https://github.com/earendil-works/pi) skill that detects AUR package updates and performs static security analysis on their PKGBUILDs before you install them.

## Why this exists

In June 2026, the **"Atomic Arch"** supply chain attack compromised 1500+ AUR packages by injecting malicious build scripts into orphaned packages. The attack deployed an infostealer and eBPF rootkit targeting developer credentials, SSH keys, and browser tokens, all triggered silently during a normal `paru` update.

This skill adds a review step before you update, catching suspicious patterns like unexpected `npm install`, `curl | bash`, or unknown download sources inside PKGBUILDs.

## Requirements

- Pi coding agent with bash tool access
- Write access to `~/.local/state/paru/` in your sandbox (see below)

## Installation

Copy the full skill directory (`aur-update-checker/`) into your (project's) skills folder.

## Sandbox setup

If you run Pi inside a bubblewrap sandbox, paru needs write access to its state directory. Ensure your sandbox script includes:

```bash
--bind ~/.local/state/paru ~/.local/state/paru \
--bind ~/.cache/paru ~/.cache/paru \
```

And make sure the directories exist:

```bash
mkdir -p ~/.local/state/paru ~/.cache/paru
```

## Usage

Tell Pi to check your AUR updates:

```
check aur updates
```

Pi will fetch all pending AUR PKGBUILDs and report `CLEAN` or `SUSPICIOUS` per package with specific line references.

## What it checks

1. Download sources: only official domains allowed
2. SHA256 checksums: must be present for every source
3. `package()` function: flags dangerous commands
4. `prepare()`/`build()`: no external downloads or script execution
5. `.install` hooks: fetched from AUR and statically inspected