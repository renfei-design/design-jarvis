# Design Jarvis

An AI design team for VS Code — clone, open, and start designing with multi-agent collaboration.

## Quick Start

```bash
git clone https://github.com/nicepkg/design-jarvis
code design-jarvis
```

Open VS Code Copilot Chat and talk to **@Design Jarvis**. On first use, it will walk you through project setup — your project name, brand color, font, and design preferences. Or just start designing and it will use sensible defaults.

You can customize settings anytime in `jarvis.config.yaml`.

### Optional: Figma Integration

If you use Figma, the MCP servers in `.vscode/mcp.json` enable agents to read and write Figma files directly. Make sure the [Figma MCP](https://www.figma.com/community/plugin/1501516954498498498/figma-mcp-by-figma) plugin is running in Figma desktop.

## What's Included

### 7 Agents (`.github/agents/`)

| Agent | Role | Talk To |
|-------|------|---------|
| **Design Jarvis** | Orchestrator | `@Design Jarvis` in Copilot Chat |
| **Design Lead** | Strategic direction, critique, quality gates | Routed by Jarvis |
| **UX Designer** | Wireframes, user flows, information architecture | Routed by Jarvis |
| **UI Designer** | Hi-fi screens, visual design, presentations | Routed by Jarvis |
| **UX Researcher** | Competitive analysis, market research | Routed by Jarvis |
| **Content Designer** | UI text, terminology, content strategy | Routed by Jarvis |
| **Design Ops** | Project memory, decisions, session persistence | Routed by Jarvis |

### 6 Skills (`.skills/`)

| Skill | Purpose |
|-------|---------|
| wireframe | Build low-fi wireframe screens |
| hifi-design | Build high-fidelity visual design screens |
| competitive-ux | Competitive UX analysis reports |
| content-review | Review and fix UI text |
| design-checklist | Track design process progress |
| memory | Three-tier persistent memory management |

### Memory System (`.jarvis/memory/`)

Three-tier memory that persists context across sessions:
- **Hot** — project context, active decisions, last session summary (loaded automatically)
- **Warm** — past session archives (loaded on demand)
- **Cold** — auto-generated activity logs (reference only)

## Customize

### Project Settings

Edit `jarvis.config.yaml` to configure:
- Project name and design system tokens (brand color, font, spacing)
- Content style guide (Microsoft, Google, Apple, or custom)
- Figma library file keys

### Design System Defaults

| Token | Default |
|-------|---------|
| Screen size | 1920 × 1080 |
| Font | Inter (400, 600, 700) |
| Brand color | `#0078D4` |
| Spacing grid | 4px |
| Corner radius | 4 / 8 / 12 px |

### Add More Agents & Skills

The `extras/` directory contains 2 additional agents and 3 additional skills:

```bash
# Activate an agent
cp extras/agents/design-reviewer.agent.md .github/agents/

# Activate a skill
cp -r extras/skills/a11y-audit .skills/
```

See [extras/README.md](extras/README.md) for the full list.

## Architecture

```
.github/agents/              Agent definitions (VS Code Copilot discovers these)
  jarvis.agent.md           Orchestrator — routes to specialists
  design-lead.agent.md      Strategic direction, quality gates
  design-ops.agent.md       Project memory, decisions, session persistence
  ux-designer.agent.md      Wireframes, flows, UX briefs
  ui-designer.agent.md      Hi-fi screens, visual design
  ux-researcher.agent.md    Competitive analysis, research
  content-designer.agent.md UI text, terminology, voice & tone

.skills/                    Domain knowledge (agents reference these)
  wireframe/SKILL.md
  hifi-design/SKILL.md
  competitive-ux/SKILL.md
  content-review/SKILL.md
  design-checklist/SKILL.md
  memory/SKILL.md

.jarvis/memory/             Persistent project memory
  hot/                      Always loaded (context, decisions, last session)
  warm/sessions/            Session archives
  cold/auto-log/            Activity traces

.vscode/
  settings.json             Skill discovery paths
  mcp.json                  Figma MCP server config (optional)

extras/                     Optional agents & skills (copy to activate)
  agents/
  skills/

copilot-instructions.md     Shared context for all agents
jarvis.config.yaml          Project configuration
```

## License

MIT
