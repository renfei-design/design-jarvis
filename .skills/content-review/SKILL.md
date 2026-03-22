---
name: content-review
description: >
  Review and fix UI text in Figma designs against writing style guide principles.
  Use when asked to review content, fix text, check grammar, capitalization, word choice,
  or improve UI strings in a Figma design. Extracts text from Figma, reviews against
  style rules, and applies approved changes back to Figma.
---

# Content Review

Review UI text in Figma against writing style guidelines. Extract text, identify issues, propose fixes, and apply approved changes.

## When to Use

- User asks to "review content" or "check the text"
- User asks about grammar, capitalization, or wording
- As part of a Design Reviewer unified review pass
- Before handoff — clean up UI strings

## Style Guide Configuration

The content style guide is configured in `jarvis.config.yaml` under `contentStyle`. Supported values:
- `"microsoft"` — Microsoft Writing Style Guide
- `"google"` — Google developer documentation style
- `"apple"` — Apple Human Interface Guidelines writing style
- `"custom"` — custom style guide path in `customStyleGuide`

The rules below are **universal best practices** that apply regardless of the configured style guide. Style-specific rules (e.g., capitalization conventions) should be checked against the configured guide.

## Universal Content Rules

### 1. Capitalization

| Element | Rule | Example |
|---------|------|---------|
| Page/dialog titles | Sentence case | "Account settings" not "Account Settings" |
| Button labels | Sentence case | "Save changes" not "Save Changes" |
| Menu items | Sentence case | "Export as PDF" not "Export As PDF" |
| Tab labels | Sentence case | "General settings" not "General Settings" |
| Column headers | Sentence case | "Last modified" not "Last Modified" |
| Acronyms | ALL CAPS | "API", "URL", "ID" |
| Product names | As branded | Keep exact casing per brand guidelines |

> **Note:** Some style guides (e.g., Apple) use Title Case for certain elements. Check the configured `contentStyle` and adjust accordingly.

### 2. Voice & Tone

- **Active voice** preferred: "Save the file" not "The file will be saved"
- **Direct address**: "You can edit..." not "Users can edit..."
- **Positive framing**: "To continue, sign in" not "You can't continue without signing in"
- **Contractions**: Use naturally — "don't", "can't", "it's" (not forced formality)

### 3. Action Labels

- **Be specific**: "Save settings" not "Submit" or "OK"
- **Start with a verb**: "Create workspace" not "New workspace"
- **Match the action**: "Delete" for destructive, "Remove" for non-destructive
- **Avoid "click" / "tap"**: Input-method-neutral — "Select" instead

### 4. Error Messages

Structure: What happened → Why → What to do
- ✓ "Couldn't save the file. The file is read-only. Try saving to a different location."
- ✗ "Error: Save failed."

### 5. Common Issues

| Issue | Bad | Good |
|-------|-----|------|
| Redundant words | "In order to" | "To" |
| Passive voice | "Settings are saved" | "Settings saved" or "We saved your settings" |
| Jargon | "Instantiate a new instance" | "Create" |
| Vague labels | "Process" / "Submit" | "Save changes" / "Create report" |
| Double negatives | "Not unlike" | "Similar to" |
| Wordiness | "At this point in time" | "Now" |
| Emoji in UI | "Saved! 🎉" | "Saved successfully" |

## Workflow

### Step 1 — Extract Text

Collect all text from the target Figma frame(s) — including each text element’s content, position, and context (parent frame name).

### Step 2 — Review Against Rules

For each text node, check:
1. Capitalization matches the element type
2. Voice is active and direct
3. Action labels are specific and verb-first
4. No common issues from the table above
5. Consistent terminology (same concept = same word throughout)
6. No typos or grammar errors

### Step 3 — Report

```markdown
## Content Review: {Frame Name}

### Issues Found

| # | Node | Current Text | Issue | Suggested Fix | Severity |
|---|------|-------------|-------|---------------|----------|
| 1 | Header/Title | "Configure Your Settings" | Title Case → Sentence case | "Configure your settings" | Warning |
| 2 | Button | "Submit" | Vague action label | "Save changes" | Warning |
| 3 | Error msg | "Error occurred" | Missing context/recovery | "Couldn't save. Try again." | Critical |
| 4 | Tooltip | "Click here to..." | Input-method-specific | "Select to..." | Info |

### Terminology Consistency
- "workspace" vs "project" — used interchangeably. Standardize on one.
- "delete" vs "remove" — both used for same action. Pick based on destructiveness.

### Summary
- Critical: {N} | Warning: {N} | Info: {N}
```

### Step 4 — Apply Fixes (with approval)

After presenting the report, ask the user which fixes to apply. Then batch all approved text changes into the Figma file.

## Quality Checklist

- [ ] All text extracted from the frame(s)
- [ ] Capitalization checked for every text element
- [ ] Action labels are specific and verb-first
- [ ] Terminology is consistent across the entire flow
- [ ] Error messages include context and recovery steps
- [ ] No passive voice in primary actions
- [ ] No style-guide-specific violations (per configured `contentStyle`)

## Notes

- **Don't over-correct** — if the existing text is clear, concise, and consistent, leave it alone
- **Context matters** — a button label makes sense in the context of its screen. "Delete" is fine if the screen title makes the object clear.
- **Defer to product naming** — product-specific terms should match the product's glossary, not generic rewrites
- **Batch changes** — always present all changes as a report for approval before modifying
