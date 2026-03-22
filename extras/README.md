# Extras — Optional Agents & Skills

This directory contains additional agents and skills that are **not active by default**. Copy them into the main directories to activate.

## How to Activate

### Agents
Copy an agent file into `.github/agents/`:
```bash
cp extras/agents/design-reviewer.agent.md .github/agents/
```

Then update `.github/agents/jarvis.agent.md` to add the new agent to the `agents` list and optionally add a `handoffs` entry.

### Skills
Copy a skill directory into `.skills/`:
```bash
cp -r extras/skills/a11y-audit .skills/
```

Skills are automatically discovered by agents that reference them.

## Available Agents

| Agent | Purpose |
|-------|---------|
| **Design Reviewer** | Unified review — accessibility (WCAG 2.2 AA), content quality, and design token compliance |
| **Design Technologist** | Code prototypes, HTML/CSS/JS implementations, interactive demos |

## Available Skills

| Skill | Purpose |
|-------|---------|
| **a11y-audit** | WCAG 2.2 AA accessibility audit (contrast, font size, touch targets) |
| **canvas-read** | Read and understand the current Figma canvas |
| **online-research** | Web research with speed-optimized fetch strategy |
