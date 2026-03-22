---
name: canvas-read
description: >
  Read and understand the current Figma canvas to build deep context before responding.
  Use when asked to read, look at, analyze, describe, or understand the canvas, Figma file,
  design, or selection. Also use proactively before any task that references existing
  canvas content — wireframe feedback, design review, content extraction, or iteration.
---

# Canvas Reader

Automatically scan the current Figma canvas to build deep visual + structural context.
**Goal:** understand what's on the canvas so you can answer questions, give feedback, extract content, or plan next actions accurately.

## Core Principle: Screenshot First, Details Later

**ALWAYS get screenshots before diving into node details.** Screenshots give you 90% of what you need in 1-2 seconds. Node tree inspection is slow and should only be used for targeted lookups after you already know what you're looking for from the screenshot.

**Anti-pattern (slow):** Get document info → inspect every child → parse tree → find target
**Correct pattern:** Get document info → take screenshots (parallel) → visually identify target → inspect that specific node

## When to Trigger

Activate this skill when the user says anything like:
- "read my canvas" / "look at my Figma" / "what's on my canvas"
- "analyze my design" / "describe what you see"
- "look at my selection" / "what did I select"
- Any question that requires understanding existing canvas content

Also activate **proactively** before design tasks that reference the canvas (e.g., "iterate on frame 3", "fix the spacing", "add a state to this screen").

## Workflow

### Step 1 — Index + Screenshot (do in parallel)

Get the frame list and take screenshots of each frame simultaneously.

**Parse screenshots visually:** Read the UI content, identify components, layouts, text, interaction states — all from the image. You have strong vision capabilities. Use them.

**For large canvases (>10 frames):** Screenshot the first 10, then ask the user if they want to continue or focus on specific frames.

### Step 2 — Targeted Node Lookup (only when needed)

After screenshots, if you need to **modify** a specific element you identified visually, inspect the direct children of the frame containing the target. Then drill into just the subtree that contains your target.

**Never scan the entire tree** looking for something — find it visually in the screenshot first, then narrow down by checking only the relevant branch.

**Efficient node-finding strategy:**
1. Screenshot tells you: "the settings panel is in the bottom-right of the main screen"
2. Inspect the frame → find the panel child (by name or position)
3. Inspect that panel → find the specific element (1-2 hops, not 8)

### Step 3 — Read Text Content (conditional)

If you need exact text content (copy, labels, data) that's too small to read in screenshots, extract text nodes from the target frame.

Use when:
- You need exact wording (for content review, copy extraction)
- Screenshots are too small to read fine text
- You're extracting structured data (tables, lists)

**Skip this step** if you can read the text from screenshots. You usually can.

### Step 4 — Read Selection (if user selected something)

If the user says "look at my selection" or "what did I select", read the current selection and inspect those nodes.

## Output Format

After scanning, present a structured summary:

### For Flow/Screen Designs
```
**Canvas: [Page Name]**
[N] frames — [brief description of what the flow covers]

| # | Frame | Description |
|---|-------|-------------|
| 1 | [name] | [what you see in the screenshot] |
| 2 | [name] | [what you see] |
...
```

### For Single Screen / Component
```
**[Frame Name]**
[Detailed description of what you see: layout, components, states, content]
```

### For Mixed Content
```
**Canvas: [Page Name]**
- [N] frames: [description]
- [N] text annotations: [description]
- [N] other elements: [description]
```

## Editing Workflow: Screenshot → Identify → Targeted Lookup → Modify

When the user asks you to change something on the canvas:

1. **Screenshot the frame** — visually confirm what needs changing and where it is
2. **Get direct children** of the frame — match by name or position to find the parent container of the target element
3. **Drill one level deeper** if needed — inspect that container to find the specific node ID
4. **Modify** — make the change using the appropriate Figma operation

**NEVER:** Parse the entire node tree to search for keywords. That's what screenshots are for.

**Rule of thumb:** If you need more than 2 node inspections to find your target, you're probably not using screenshots effectively.

## Tips

- **Parallel screenshots are the #1 priority** — they take ~1-2s each and give you 90% of context
- **Don't scan node trees for discovery** — use screenshots to discover, node info only to get IDs for modification
- **Name your observations** — describe what you see specifically, not generically
- **Note design patterns** — component reuse, consistent spacing, color themes, interaction states
- **Read text in screenshots** — you have vision capabilities, use them to read labels and copy directly
- **Track frame relationships** — are they sequential steps? alternative states? responsive breakpoints?
- **Cache what you've learned** — if you already screenshotted a frame earlier in the conversation, don't re-inspect. Use the visual context you already have.

## Performance

| Operation | Time | When to use |
|-----------|------|-------------|
| Get document info | ~100ms | Always first |
| Screenshot | ~1-2s each | Always — primary context tool |
| Node inspection (single) | ~200ms-1s | Targeted lookup after visual ID |
| Node inspection (batch) | ~500ms-2s | Only for batch property reads |
| Text extraction | ~200ms | Only when screenshot text unreadable |

**Goal:** Full canvas understanding in 2 steps (get frame list + parallel screenshots). That's ~3-5 seconds for 7 frames.
