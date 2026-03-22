# Design Jarvis

A multi-agent AI design team for VS Code — composable specialists that collaborate through an orchestrator, extensible with drop-in agents and skills.

## Why Multi-Agent?

A single AI assistant handles every request the same way. A multi-agent framework splits design work across **specialists that each excel at one thing** — then coordinates them through an orchestrator that knows who to call and when.

This matters for design work because:

- **Deeper expertise** — Each agent carries domain-specific instructions, constraints, and skills. A UX Designer thinks in flows and hierarchy; a Content Designer thinks in terminology systems and voice. A single agent can't hold all of that context effectively.
- **Composable workflows** — Agents hand off to each other. Ask for a "landing page" and the orchestrator can route wireframing to UX Designer, visual polish to UI Designer, and copy review to Content Designer — each using the right skill for the job.
- **Persistent memory** — A dedicated Design Ops agent manages a three-tier memory system, so decisions, context, and session history survive across conversations without polluting other agents' context windows.
- **Quality gates** — The Design Lead agent provides critique and review as a separate step, not an afterthought bolted onto the generator.

## Quick Start

```bash
git clone https://github.com/renfei-design/design-jarvis
code design-jarvis
```

Open VS Code Copilot Chat → **@Design Jarvis**. It routes your request to the right specialist automatically.

Customize project settings anytime in `jarvis.config.yaml` (brand color, font, spacing, content style guide, Figma library keys).

## Architecture

The framework has four layers: **agents**, **skills**, **memory**, and **config**.

```
Orchestrator (Design Jarvis)
  ├── routes tasks to specialist agents
  ├── agents invoke skills for domain knowledge
  ├── Design Ops manages persistent memory
  └── config drives tokens, style, and tool adapters
```

### Agents — The Team

Agents live in `.github/agents/` — VS Code Copilot discovers them automatically.

| Agent | Role | Why It's Separate |
|-------|------|-------------------|
| **Design Jarvis** | Orchestrator | Routing logic stays clean; specialists stay focused |
| **Design Lead** | Direction & critique | Review is a distinct cognitive mode from creation |
| **UX Designer** | Wireframes, flows, IA | Low-fi thinking shouldn't be polluted by visual polish |
| **UI Designer** | Hi-fi screens, visual design | Pixel-level work needs different constraints than IA |
| **UX Researcher** | Competitive analysis, research | Evidence-gathering is a separate workflow |
| **Content Designer** | UI text, terminology, voice | Language work needs its own style rules and context |
| **Design Ops** | Memory, decisions, sessions | Memory management requires its own persistence logic |

### Skills — Domain Knowledge

Skills live in `.skills/`. Each is a self-contained `SKILL.md` file that an agent loads on demand — keeping agent definitions lightweight while giving them deep domain expertise when needed.

| Skill | Agent | What It Provides |
|-------|-------|-----------------|
| wireframe | UX Designer | Low-fi screen generation workflow |
| hifi-design | UI Designer | Hi-fi visual design workflow |
| competitive-ux | UX Researcher | Competitive analysis report structure |
| content-review | Content Designer | UI text review against style guides |
| design-checklist | Design Ops | Design process tracking |
| memory | Design Ops | Three-tier persistent memory management |

### Memory — Cross-Session Persistence

The memory system (`.jarvis/memory/`) gives agents continuity across conversations:

| Tier | Contents | Loaded |
|------|----------|--------|
| **Hot** | Project context, active decisions, last session summary | Automatically |
| **Warm** | Past session archives | On demand |
| **Cold** | Auto-generated activity logs | Reference only |

Design Ops manages all reads and writes. Other agents never touch memory files directly — this prevents conflicts and keeps the memory schema consistent.

## Extensibility

The framework is designed to grow. Every component — agents, skills, config — is a plain file you can add, remove, or modify.

### Add an Agent

1. Create a `.agent.md` file in `.github/agents/`
2. Define the agent's role, instructions, and which skills it uses
3. Update the orchestrator to route to it

Two ready-made agents ship in `extras/agents/`:

| Agent | Purpose |
|-------|---------|
| **Design Reviewer** | Unified a11y + content + token compliance review |
| **Design Technologist** | HTML/CSS/JS code prototypes and interactive demos |

```bash
cp extras/agents/design-reviewer.agent.md .github/agents/
```

### Add a Skill

1. Create a directory in `.skills/` with a `SKILL.md` file
2. Reference it from an agent's definition

Three ready-made skills ship in `extras/skills/`:

| Skill | Purpose |
|-------|---------|
| **a11y-audit** | WCAG 2.2 AA accessibility audit |
| **canvas-read** | Read and understand the current Figma canvas |
| **online-research** | Web research with speed-optimized fetch |

```bash
cp -r extras/skills/a11y-audit .skills/
```

Skills are automatically discovered — no registration step required.

### Customize Config

`jarvis.config.yaml` controls project-level settings without touching agent or skill definitions:

```yaml
project:
  name: "My Product"

designSystem:
  tokens:
    brandColor: "#0078D4"
    fontFamily: "Inter"
    spacingUnit: 4

contentStyle: "google"   # or "microsoft" | "apple" | "custom"

agents:
  qualityGate: "Design Lead"
  required: ["Design Jarvis", "Design Lead", "Design Ops"]
```

### Build Your Own

The patterns here aren't design-specific. The same architecture — **orchestrator → specialists → skills → memory** — works for any domain where tasks benefit from role separation:

- **Engineering team**: Architect, Frontend Dev, Backend Dev, QA, DevOps
- **Content team**: Strategist, Writer, Editor, SEO Specialist
- **Research team**: Lead, Quant Analyst, Qual Researcher, Synthesizer

The key principles:
1. **One agent, one role** — keep context windows focused
2. **Skills as loadable knowledge** — agents stay lightweight, skills provide depth
3. **Memory as a service** — one agent owns persistence, others stay stateless
4. **Config over code** — project settings live in YAML, not buried in prompts

### Figma Integration (Optional)

If you use Figma, configure the MCP server in `.vscode/mcp.json` and run the [Figma MCP](https://www.figma.com/community/plugin/1501516954498498498/figma-mcp-by-figma) plugin. Agents work without Figma for non-visual tasks — they'll produce HTML prototypes or text descriptions instead.

## File Structure

```
.github/agents/              Agent definitions (auto-discovered by VS Code)
.skills/                     Skill definitions (loaded on demand by agents)
.jarvis/memory/              Persistent project memory (managed by Design Ops)
.vscode/                     Editor config and MCP server setup
extras/                      Optional agents & skills (copy to activate)
copilot-instructions.md      Shared context for all agents
jarvis.config.yaml           Project configuration
```

## License

MIT
