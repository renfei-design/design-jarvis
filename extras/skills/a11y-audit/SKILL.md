---
name: a11y-audit
description: >
  Audit Figma designs for WCAG accessibility issues — color contrast, minimum font size,
  touch target sizing, and missing text alternatives. Use when asked to check accessibility,
  audit contrast ratios, find a11y issues, verify WCAG compliance, or review designs for
  accessibility problems. Scans nodes, analyzes offline, and annotates issues in Figma.
---

# Accessibility Audit

Audit Figma designs for **WCAG 2.2 AA** compliance. Scan the canvas, analyze offline, and annotate issues directly in Figma.

## When to Use

- User asks to "check accessibility" or "audit a11y"
- User asks about contrast ratios, font sizes, touch targets
- Before handoff — run as a final quality gate
- As part of a Design Reviewer unified review

## Audit Categories

### 1. Color Contrast (WCAG 1.4.3 / 1.4.6)

| Element | Minimum Ratio | Standard |
|---------|---------------|----------|
| Normal text (<18px, or <14px bold) | 4.5:1 | AA |
| Large text (≥18px, or ≥14px bold) | 3:1 | AA |
| UI components & graphical objects | 3:1 | AA |
| Enhanced (AAA — optional) | 7:1 / 4.5:1 | AAA |

**How to check:** Extract foreground color and background color for each text node. Calculate relative luminance contrast ratio using the WCAG formula:
```
contrast = (L1 + 0.05) / (L2 + 0.05)
where L1 = lighter, L2 = darker
L = 0.2126 * R + 0.7152 * G + 0.0722 * B
(with gamma correction: c <= 0.04045 ? c/12.92 : ((c+0.055)/1.055)^2.4)
```

### 2. Minimum Font Size

- No text below **12px** in the final design
- Body text should be **14px** minimum for comfortable reading
- Caption/helper text at **12px** is acceptable but flag it as a warning

### 3. Touch Target Sizing (WCAG 2.5.8)

- Interactive elements: minimum **44×44px** touch target (24×24px is the absolute minimum for AA)
- If the visual element is smaller, the hit area / spacing must compensate
- Buttons, links, checkboxes, toggles, sliders — all count

### 4. Text Alternatives

- **Icons without text labels** — need `aria-label` annotation or adjacent text
- **Image placeholders** — flag if no alt text annotation exists
- **Icon-only buttons** — must have a tooltip or label annotation

## Workflow

### Step 1 — Get the Canvas

Identify which frames to audit. If the user specified a frame, use that. Otherwise audit all top-level frames. Get screenshots for visual overview.

### Step 2 — Gather Node Data

For each frame to audit, collect all text nodes with their:
- Font size, font weight, font color
- Position and dimensions
- Parent container info

And all interactive-looking elements (buttons, inputs, etc.) with their dimensions.

### Step 3 — Analyze Offline

Process the scanned data locally — no additional Figma calls needed:

1. **Contrast checks** — for each text node, find its effective background color (walk up the parent chain to find the first opaque fill). Calculate contrast ratio.
2. **Font size checks** — flag any text node below 12px.
3. **Touch target checks** — flag any interactive element below 44×44px. For elements between 24×44px, flag as warning.
4. **Text alternative checks** — flag icon instances without adjacent text nodes.

### Step 4 — Report

Present findings in a structured format:

```markdown
## Accessibility Audit: {Frame Name}

### Critical Issues
- **[Contrast]** "{text content}" has {ratio}:1 contrast ({fg} on {bg}) — needs {required}:1
- **[Touch Target]** "{element name}" is {w}×{h}px — minimum 44×44px

### Warnings
- **[Font Size]** "{text content}" is {size}px — consider 14px minimum for body text
- **[Touch Target]** "{element name}" is {w}×{h}px — below 44×44px recommended size

### Passed
- ✓ {N} text nodes meet contrast requirements
- ✓ {N} interactive elements meet touch target requirements
- ✓ All icons have adjacent text labels

### Summary
| Category | Pass | Fail | Warning |
|----------|------|------|---------|
| Contrast | {n} | {n} | {n} |
| Font Size | {n} | {n} | {n} |
| Touch Targets | {n} | {n} | {n} |
| Text Alternatives | {n} | {n} | {n} |
```

### Step 5 — Annotate in Figma (Optional)

If the user wants visual annotations, add annotation markers next to problem areas:
- **Red dot + text** for critical issues
- **Yellow dot + text** for warnings
- Group all annotations in a frame named "A11y Audit — {date}"

Place the annotation frame adjacent to the audited frame, not overlapping.

## Quick Reference: Common Issues

| Issue | How to Spot | Fix |
|-------|-------------|-----|
| Light gray text on white | Contrast < 4.5:1 | Darken text or darken background |
| Tiny helper text | Font < 12px | Increase to 12px minimum |
| Small icon buttons | Touch target < 44px | Add padding or increase button size |
| Icon-only actions | No adjacent label | Add tooltip annotation or text label |
| Placeholder text contrast | Often too light | Ensure 4.5:1 even for placeholders |

## Notes

- This audit covers visual accessibility only — keyboard navigation and screen reader behavior require code inspection
- When multiple text colors exist on a gradient or image background, check against the worst-case (lightest) area
- Design tokens should map to colors that pass contrast requirements — if they don't, flag the token definition as the root cause
