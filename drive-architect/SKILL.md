---
name: drive-architect
description: Use when auditing drive or directory organization, finding wasted space, locating misplaced files, or proposing directory restructuring. Triggers on drive cleanup, storage optimization, file organization, "this drive is a mess."
---

# Drive Architect

Audit a drive's structure, classify everything, find problems, and propose a clean layout.

## When This Fires

- "This drive is a mess"
- "Where is all my space going?"
- "I need to reorganize"
- "What's orphaned / junk?"
- Before major project restructuring

Do NOT fire on:
- Single file searches
- Repo-internal cleanup
- Build artifact cleanup within a single project

## Protocol

### Phase 1: MAP — What's On The Drive

```bash
# Top-level directories with sizes
du -h --max-depth=1 /path/to/drive/ 2>/dev/null | sort -rh | head -40

# Large files that might be misplaced (>100MB)
find /path/to/drive/ -maxdepth 3 -size +100M -type f 2>/dev/null | head -20

# Recent activity
find /path/to/drive/ -maxdepth 2 -mtime -7 -type f 2>/dev/null | head -30
```

Output:
```
DRIVE MAP: D:\ (450GB used / 1TB)
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

DIRECTORY              SIZE     FILES    CLASS
repos/                 120GB    45,000   DEV
output/                8GB      2,300    OUTPUT
archive/               200GB    12,000   ARCHIVE
random-folder/         50GB     3,200    UNKNOWN
```

### Phase 2: CLASSIFY — What Is Everything

| Class | Definition |
|-------|-----------|
| **DEV** | Active source code, repositories |
| **OUTPUT** | Generated files, exports, renders |
| **ARCHIVE** | Cold storage, old projects |
| **TOOLS** | Installed software, SDKs |
| **DATA** | Datasets, media, assets |
| **JUNK** | Build artifacts, caches, orphaned temp |
| **UNKNOWN** | Needs investigation |
| **MISPLACED** | Right file, wrong location |

For UNKNOWN directories, investigate:
```bash
ls -la /path/to/unknown/ | head -20
cat /path/to/unknown/README* 2>/dev/null
cat /path/to/unknown/package.json 2>/dev/null | head -5
```

### Phase 3: DIAGNOSE — What's Wrong

Look for:

**Space Waste:**
- `node_modules/` outside active projects
- Build output directories (`target/`, `dist/`, `.next/`, `build/`)
- `.git/` objects in large repos (check sizes)
- Duplicate files across directories

**Structural Problems:**
- Repos outside the repos directory
- Output mixed into source directories
- No clear separation between dev/output/archive
- Inconsistent naming conventions

**Orphans:**
- Directories with no recent activity and no clear purpose
- Config for uninstalled tools
- Temp directories that became permanent

Output:
```
DIAGNOSIS
━━━━━━━━

SPACE WASTE (recoverable: ~85GB):
  random/node_modules/       50GB   orphaned
  repos/project/target/      8GB    rebuildable
  old-downloads/             15GB   stale 6+ months

STRUCTURAL:
  loose-project/     should be in repos/
  repos/output/      output mixed into repos dir

ORPHANS:
  test-thing/        last modified 2025-08, no README
```

### Phase 4: PROPOSE — Clean Structure

Target structure:
```
drive:\
  repos/        ALL source code (consistent naming convention)
  output/       Generated files, exports (dated subdirs OK)
  archive/      Cold storage, old projects
  tools/        Installed software, SDKs
  data/         Datasets, media, assets
  docs/         Non-repo documentation
  temp/         Explicitly temporary, safe to delete
```

Present as a diff:
```
PROPOSED CHANGES
━━━━━━━━━━━━━━━

MOVES:
  loose-project/  →  repos/loose-project/
  repos/output/   →  output/march-2026/

DELETES (confirm each):
  random/node_modules/     50GB   orphaned
  old-downloads/           15GB   stale

RENAMES:
  MyProject_v2_FINAL/  →  repos/my-project/
```

### Phase 5: EXECUTE — With Confirmation

Group operations by safety:

```
SAFE (no data loss):
  1. mkdir repos/ archive/ temp/

MOVES (reversible):
  2. mv loose-project/ repos/

DELETES (confirm each individually):
  3. rm -rf random/node_modules/    # 50GB orphaned
```

**Never execute deletes without explicit approval.**

### Phase 6: VERIFY

```bash
# Re-scan sizes
du -h --max-depth=1 /path/to/drive/ | sort -rh

# Verify repos still work
for d in repos/*/; do git -C "$d" status --short 2>/dev/null | head -3; done
```

## Rules

1. **Never delete without approval.** Present plan, wait for "go."
2. **Moves before deletes.** Reorganize first, clean second.
3. **Size matters.** Don't discuss 50MB orphans. Focus on GB-scale waste.
4. **Git repos need care.** Check remotes and submodules before moving.
5. **Respect intentional mess.** Some directories are working surfaces — ask first.
6. **One drive at a time.** Don't cross-drive unless asked.
7. **Show sizes always.** Every directory and delete candidate gets a size.
