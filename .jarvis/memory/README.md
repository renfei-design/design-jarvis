# Memory System

Design Jarvis uses a three-tier memory architecture:

## Hot (`.jarvis/memory/hot/`)
Always loaded at session start. Keep these files concise.
- `context.md` — Project overview, constraints, target users (max ~150 lines)
- `decisions-active.md` — Active design decisions with D-numbers (max ~200 lines)
- `last-session.md` — Summary of the most recent session (max ~80 lines)

## Warm (`.jarvis/memory/warm/`)
Loaded on demand when conversation needs historical context.
- `sessions/` — Past session summaries (YYYY-MM-DD.md format)

## Cold (`.jarvis/memory/cold/`)
Reference only, never auto-loaded.
- `auto-log/` — Auto-generated activity traces

## How Agents Use Memory
- **Design Ops** is the sole owner of memory writes — it loads, updates, archives, and summarizes
- **Design Jarvis** (orchestrator) invokes Design Ops at session start, after decisions, and at session end
- **Design Lead** reads hot memory for context when making decisions, but delegates writes to Design Ops
- All other agents can read hot memory for project context but never write to it directly
- Session summaries are written by Design Ops at the end of each session

## Size Budgets
When hot files exceed their line limits, older content should be compressed or archived to warm/cold tiers.
