---
name: design-checklist
description: >
  Create a design project checklist in Figma that designers can use to track progress
  through the design process. Reads checklist content from the current Figma page
  (or uses the standard design checklist) and builds an interactive tracking layout.
  Use when asked to create a checklist, design tracker, project progress tracker,
  or design process checklist in Figma.
---

# Design Checklist Builder

Build a design progress checklist in Figma to track a project through the design process.

## When to Use

- User asks to "create a checklist" or "track design progress"
- At project kickoff — set up the tracking structure
- User asks for a "design process tracker"

## Standard Design Process Stages

These are generic stages that apply to most design projects. Customize per project needs.

### Stage 1: Discovery
- [ ] Gather requirements from stakeholders
- [ ] Review existing research and data
- [ ] Identify user personas and scenarios
- [ ] Define success metrics
- [ ] Document constraints and dependencies

### Stage 2: Research
- [ ] Competitive analysis complete
- [ ] User interviews / surveys planned or reviewed
- [ ] Key insights documented
- [ ] Opportunity areas identified

### Stage 3: Define
- [ ] UX brief / project brief created
- [ ] Information architecture defined
- [ ] User flows mapped
- [ ] Key design decisions documented
- [ ] Scope confirmed with stakeholders

### Stage 4: Wireframe
- [ ] Low-fidelity wireframes for key screens
- [ ] Wireframe variants explored (2-3 approaches)
- [ ] Wireframe review with Design Lead
- [ ] Direction selected and documented

### Stage 5: Design
- [ ] High-fidelity screens built
- [ ] Design system components applied correctly
- [ ] Interactive states designed (hover, active, disabled, focus)
- [ ] Empty states and error states included
- [ ] Responsive considerations addressed

### Stage 6: Review
- [ ] Accessibility audit passed (WCAG 2.2 AA)
- [ ] Content review passed
- [ ] Design token compliance verified
- [ ] Design Lead review approved
- [ ] Stakeholder review completed

### Stage 7: Refinement
- [ ] Review feedback addressed
- [ ] Edge cases covered
- [ ] Micro-interactions defined
- [ ] Final visual polish applied

### Stage 8: Handoff
- [ ] Specs and redlines documented
- [ ] Component usage annotated
- [ ] Developer walkthrough completed
- [ ] Assets exported as needed
- [ ] Design file organized and labeled

## Workflow

### Step 1 — Determine Checklist Content

Check if the user provided a custom checklist or wants the standard stages above. If custom content exists on the Figma page, read it. Otherwise, use the standard stages defined above.

### Step 2 — Build the Checklist in Figma

Create a structured frame with each stage as a section:

- **Root frame**: 800px wide, vertical layout, white background, 32px padding, 24px spacing
- **Title**: "Design Checklist" at 28px/700 weight
- **Subtitle**: "Project: {name} — Created {date}" at 14px, gray
- **Stage sections**: Light gray background, 8px corner radius, 16px padding
  - Stage heading at 18px/600 weight
  - Checklist items at 14px, prefixed with ☐

### Step 3 — Position on Canvas

Place the checklist in empty space, avoiding overlap with existing content.

## Customization

- **Add stages** — add more stage sections for project-specific phases
- **Remove stages** — skip stages that don't apply (e.g., skip Research for small features)
- **Add items** — add project-specific checklist items to any stage
- **Progress tracking** — replace ☐ with ☑ as items are completed

## Notes

- The checklist is a **living document** — update it throughout the project
- Each stage should be clearly labeled so any team member can understand progress
- Consider adding dates or assignees for team accountability
- The Design Ops agent typically manages checklist updates
