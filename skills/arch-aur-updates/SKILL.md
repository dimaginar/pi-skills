---
name: arch-aur-updates
description: Find and display full PKGBUILD contents of AUR packages that have updates available. Use when checking which locally installed AUR packages need updating and reviewing their PKGBUILD details.
---

# Arch Linux AUR Update Checker

## Purpose

Finds packages that are both installed locally (via pacman) and available in the AUR (via paru), then displays their full PKGBUILD contents so you can review what's being updated.

## Workflow

When the user says "check" or any variation, execute the following two steps **in order**:

### Step 1 — Fetch PKGBUILDs (raw output)

Run this command and output its **exact stdout/stderr** — verbatim, no commentary:

```bash
comm -12 <(pacman -Qmq | sort) <(paru -Quq | sort) | while read pkg; do
  echo "=== $pkg ==="
  paru -Gp $pkg
  echo
done 2>&1
```

Rules for Step 1 output:
- Do NOT reformat, re-wrap, or re-indent the PKGBUILD content.
- If no packages match, output nothing (empty result is valid).
- If the command fails, output the error message exactly as-is.

### Step 2 — Security Analysis

For **each package** returned by the PKGBUILD tool, perform a static security analysis:

1. **source/source_x86_64**: only official domains (GitHub releases, vendor sites)? Flag unknown domains, pastebins, raw IPs.
2. **sha256sums**: present for every source? Flag `SKIP` or missing.
3. **package()**: only `install`/`cp`/`ln`/`sed`? Flag `npm`/`pip`/`curl`/`wget`/`eval`/`bash -c`.
4. **prepare()/build()**: no external downloads or script execution?
5. **.install file referenced?** If `install=$pkgname.install` appears in the PKGBUILD, fetch it via curl from the AUR web interface:
   ```bash
   curl -s "https://aur.archlinux.org/cgit/aur.git/plain/$pkgname/$pkgname.install"
   ```
   Inspect the `.install` file for malicious pre/post install hooks. Do NOT execute the script.

**Constraints:** Static analysis only — do NOT execute any fetched scripts. Do NOT download from package `source` URLs. Fetching `.install` files from the AUR web interface (aur.archlinux.org) via curl is allowed for static inspection only.

**Report per package:** `CLEAN` or `SUSPICIOUS` with specific line references for each of the five checks.

## Notes

- Requires `pacman` and `paru` to be installed.
- Requires write access to `~/.local/state/paru/` for paru to function. If you get a "Read-only file system" error, ensure the sandbox allows writes to that path.
