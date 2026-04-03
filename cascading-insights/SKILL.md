---
name: cascading-insights
description: Use when needing portfolio-wide status across all repos, finding cross-project patterns, deciding what to work on next, or when asking "what's the state of everything?" Triggers on portfolio review, project status, cross-repo analysis, priority decisions, stale project detection.
---

# Cascading Insights

Portfolio-level intelligence sweep across all your repos. Scan, classify, connect, recommend.

## When This Fires

- "What's the state of everything?"
- "What should I work on next?"
- "Which projects are stale?"
- Start of a work session when direction is unclear

Do NOT fire on:
- Single-project deep dives
- Design brainstorms
- Implementation tasks

## Protocol

### Phase 1: SCAN — What Exists

For each repo in the working directory:

```bash
# Last commit date + message
git -C {repo} log -1 --format="%ai %s" 2>/dev/null

# Branch count and current branch
git -C {repo} branch --list | head -10

# Uncommitted changes
git -C {repo} status --short | head -10
```

Also check:
- Does a CLAUDE.md / README exist?
- Any test files? How many?
- Last meaningful activity?

Output per repo:
```
REPO: my-project
  Last commit: 2026-03-29 "add user auth"
  Branch: main (+1 feature branch)
  Uncommitted: 2 files modified
  Tests: 45 files
  Status: ACTIVE
```

### Phase 2: CLASSIFY — What State Is It In

Every repo gets exactly one label:

| Status | Definition |
|--------|-----------|
| **ACTIVE** | Commits in last 7 days |
| **WARM** | Commits in last 30 days, paused but not abandoned |
| **STALLED** | No commits 30-90 days |
| **SHIPPED** | Deployed/released, maintenance only |
| **BACKBURNER** | Intentionally parked |
| **DEAD** | No plan, no activity, candidate for archive |

Present as a sorted table, most active first:
```
STATUS REPORT — 2026-04-03
━━━━━━━━━━━━━━━━━━━━━━━━━

ACTIVE:
  project-a     — last: 2d ago — Main product
  project-b     — last: 5d ago — API server

WARM:
  project-c     — last: 15d ago — Mobile app

STALLED:
  project-d     — last: 45d ago — Needs decision
```

### Phase 3: CONNECT — Find Patterns

Look across all repos for:

**Shared Logic:**
- Same algorithm or pattern reimplemented in multiple repos
- Same dependencies appearing across projects
- Similar data structures or schemas

**Dependencies:**
- What blocks what?
- What unlocks the most if finished next?

**Duplication:**
- Are two repos doing the same thing slightly differently?
- Could they share a library?

Output:
```
CROSS-PROJECT PATTERNS
━━━━━━━━━━━━━━━━━━━━━

SHARED: Auth middleware
  Found in: project-a/src/auth, project-c/lib/auth
  Consider: Extract to shared package?

BLOCKER: project-a → project-b
  project-a's API client depends on project-b endpoints
  project-b has uncommitted breaking changes
```

### Phase 4: RECOMMEND — What To Work On

Based on scan + classify + connections:

```
RECOMMENDATION
━━━━━━━━━━━━━

1. [project-b] — Uncommitted breaking changes block project-a
   Action: Commit or revert the API changes
   Unlocks: project-a can resume

2. [project-a] — Highest momentum, most recent commits
   Action: Continue auth feature
   Unlocks: MVP launch

PARK FOR NOW:
  - project-d: No clear direction yet, don't force it

NEEDS DECISION:
  - project-c: Shared auth — extract now or after MVP?
```

## Rules

1. **Git is truth.** Don't rely on memory or assumptions about project state.
2. **Classify before connecting.** Don't find patterns in dead projects.
3. **Prioritize, don't list.** One clear recommendation beats five options.
4. **Surface decisions, don't make them.** You recommend, the user decides.
5. **Time-bound recommendations.** "This weekend" not "eventually."
6. **Size your scan.** 5 repos? Scan all. 50 repos? Scan top-level and ask which to deep-dive.
