---
name: memory
description: >
  Manage the project's persistent memory — project context, decisions, preferences, and
  cross-project patterns using a three-tier hierarchical architecture that scales from
  small to large projects.
---

# Memory System

Manage Design Jarvis's persistent memory so it becomes more effective with each session.

## Architecture: Three-Tier Memory

Memory is organized into three tiers by access frequency:

| Tier | Description | When loaded | Budget |
|------|-------------|-------------|--------|
| **Hot** | Always loaded at session start | Automatically | ~300-400 lines total |
| **Warm** | Loaded on demand when conversation needs it | When user asks about history | No hard limit |
| **Cold** | Reference only, never auto-loaded | Targeted search only | Unlimited |

## Memory Directory Structure

```
.jarvis/
└── memory/
    ├── hot/                            ← Always loaded at session start
    │   ├── context.md                  ← Living project brief (max 150 lines)
    │   ├── decisions-active.md         ← Active decisions with D-numbers (max 200 lines)
    │   └── last-session.md             ← Most recent session summary (max 80 lines)
    ├── warm/                           ← Loaded on demand
    │   └── sessions/
    │       ├── YYYY-MM-DD.md           ← Session summaries
    │       └── decisions-archive.md    ← Superseded/retired decisions
    ├── cold/                           ← Reference only
    │   └── auto-log/
    │       └── YYYY-MM-DD.md           ← Raw activity traces
    ├── patterns.md                     ← Cross-project design patterns
    └── preferences.md                  ← User style + workflow preferences
```

The project name comes from `jarvis.config.yaml` (`project.name`), not from directory structure. This workspace supports one project at a time. The project name is used in memory summaries for context.

## Decision Lifecycle

```
Draft → Active → Under Review → Superseded | Retired
```

| Status | Storage |
|--------|---------|
| **Draft** | Hot (decisions-active.md, marked as draft) |
| **Active** | Hot (decisions-active.md) |
| **Under Review** | Hot (flagged as unstable) |
| **Superseded** | Warm (decisions-archive.md, with link to successor) |
| **Retired** | Warm (decisions-archive.md) |

### Decision Format

```markdown
### D7: Card-Based Settings Layout
**Status:** Active
**Date:** 2026-03-08
**Context:** Need to organize 12+ settings groups in a discoverable way.
**Decision:** Use card-based layout with search, grouped by category.
**Rationale:** Scales better than left-nav for 12+ groups. Research showed card layouts have higher discoverability.
```

## Size Budgets

| File | Max lines | Action when exceeded |
|------|-----------|---------------------|
| `context.md` | 150 | Compress older sections, move to `cold/` |
| `decisions-active.md` | 200 | Archive Retired/Superseded decisions |
| `last-session.md` | 80 | Overwrite each session |

## Session Compression Schedule

| Age | Format | Max lines |
|-----|--------|-----------|
| Current week | Full summary | 80 |
| This month | Compressed | 20 |
| Older | Milestone entry | 5 |

## When to Use Memory

### Session Start — Load Hot Memory
1. Read `jarvis.config.yaml` for project name
2. Read `.jarvis/memory/hot/context.md`
3. Read `.jarvis/memory/hot/decisions-active.md`
4. Read `.jarvis/memory/hot/last-session.md`

**Total load: ~300-400 lines** — constant regardless of project age.

### During Session — Write Incrementally
- **Decision made** → append to `.jarvis/memory/hot/decisions-active.md`
- **Decision superseded** → move to `.jarvis/memory/warm/sessions/decisions-archive.md`
- **User preference** → update `.jarvis/memory/preferences.md`

### Session End — Summarize & Compact
1. Write summary to `.jarvis/memory/warm/sessions/YYYY-MM-DD.md`
2. Overwrite `.jarvis/memory/hot/last-session.md`
3. Archive stale decisions
4. Compress if over budget

## Auto-Logging

If configured, activity traces are captured automatically:
- On every Figma command: command name + params summary buffered
- Every 5 minutes: buffer flushed to `.jarvis/memory/cold/auto-log/YYYY-MM-DD.md`
- On shutdown: final flush

Auto-logs are COLD tier — separate from human-written summaries.

**What auto-logging does NOT capture** (you must write explicitly):
- Decision rationale
- Project context and goals
- User preferences
- Cross-project patterns
- Component usage tracking
