# Design Jarvis — Project Context

This file provides shared context that applies to ALL agents in this workspace.

## Project Overview

This workspace uses the **Design Jarvis** multi-agent system — an AI design team embedded in VS Code + Figma.

### How It Works

- **@Design Jarvis** is the orchestrator — talk to it and it routes to the right specialist
- **5 specialist agents** handle different design domains (wireframes, hi-fi, content, research, direction)
- **1 operations agent** (Design Ops) manages project memory and decision persistence
- **6 skills** provide domain knowledge that agents reference automatically
- **Figma integration** via MCP server (official Figma MCP) — optional

### Agents

| Agent | Role | What It Does |
|-------|------|-------------|
| Design Jarvis | Orchestrator | Routes your tasks to the right specialist |
| Design Lead | Director | Strategic direction, critique, quality gates |
| UX Designer | Specialist | Wireframes, user flows, information architecture |
| UI Designer | Specialist | Hi-fi screens, visual design, presentations |
| UX Researcher | Specialist | Competitive analysis, market research |
| Content Designer | Specialist | UI text, terminology, content strategy |
| Design Ops | Operations | Project memory, decisions, session persistence |

### Skills

| Skill | Used By | Purpose |
|-------|---------|---------|
| wireframe | UX Designer | Build low-fi wireframe screens |
| hifi-design | UI Designer | Build high-fidelity design screens |
| competitive-ux | UX Researcher | Competitive UX analysis reports |
| content-review | Content Designer | Review and fix UI text |
| design-checklist | Design Ops | Track design process progress |
| memory | Design Ops | Three-tier persistent memory management |

### Skill Dependencies

Each agent requires specific skills to function. Core skills (`.skills/`) are always available. Extras skills (`extras/skills/`) must be activated first.

| Agent | Required Skills (core) | Optional Skills (extras) |
|-------|----------------------|------------------------|
| Design Jarvis | — | — |
| Design Lead | — | — |
| Design Ops | memory, design-checklist | — |
| UX Designer | wireframe | — |
| UI Designer | hifi-design | — |
| UX Researcher | competitive-ux | online-research |
| Content Designer | content-review | — |

If an agent references an extras skill that isn't activated, it should fall back to its core capabilities and note the limitation.

Additional agents and skills are available in the `extras/` directory — copy them into `.github/agents/` or `.skills/` to activate.

### Design System Defaults

Update these values in `jarvis.config.yaml` to match your project:

- **Screen size:** 1920 × 1080
- **Font:** Inter (Regular 400, Semibold 600, Bold 700)
- **Brand color:** `#0078D4`
- **Spacing grid:** 4px increments
- **Corner radius:** 4px buttons/inputs, 8px cards, 12px dialogs

### Content Style

Content follows the **Google** developer documentation style guide by default. Change this by updating `contentStyle` in `jarvis.config.yaml` or instructing the Content Designer agent.

### Memory System

Project memory lives in `.jarvis/memory/` with a three-tier architecture:
- **Hot** (always loaded): `context.md`, `decisions-active.md`, `last-session.md`
- **Warm** (on demand): `sessions/`
- **Cold** (auto-logged): `auto-log/`

**Design Ops** manages all memory operations. The orchestrator invokes Design Ops at session start (load context), after decisions (log with D-numbers), and at session end (write summary). Other agents should never write to memory files directly.

### Figma Integration (Optional)

Figma MCP server is configured in `.vscode/mcp.json`. When running:
- `figma-desktop` — Official Figma MCP (reads design context, screenshots, metadata)

Agents work without Figma for non-visual tasks (research, UX briefs, content strategy). If Figma tools are unavailable, agents will describe designs in text or create HTML prototypes instead.
