---
name: hifi-design
description: >
  Build high-fidelity design screens in Figma. Default: HTML prototype captured to Figma.
  Use when asked to create hi-fi designs, mockups, polished screens, or production-ready visuals.
---

# Hi-Fi Design Builder

Create high-fidelity design screens in Figma. **Default workflow: HTML prototype → Figma capture.** This produces polished, pixel-accurate designs fast.

## Workflow — HTML Prototype to Figma

This is the **primary workflow** for creating new screens. It produces polished visuals that are indistinguishable from component-based designs at review/presentation fidelity.

### Steps

1. **Create HTML prototype** — Build an HTML file in the project's prototype directory. Use CSS custom properties for design tokens. Reference the project's token stylesheet if one exists.

   ```html
   <!DOCTYPE html>
   <html>
   <head>
     <meta charset="UTF-8">
     <meta name="viewport" content="width=device-width, initial-scale=1.0">
     <title>{Screen Name}</title>
     <style>
       :root {
         --brand-color: #0078D4;
         --font-family: 'Inter', sans-serif;
         --bg-primary: #FAFAFA;
         --text-primary: #242424;
         --radius-medium: 4px;
       }
       body {
         font-family: var(--font-family);
         background: var(--bg-primary);
         color: var(--text-primary);
         margin: 0;
       }
       .btn-primary {
         background: var(--brand-color);
         color: #FFFFFF;
         border: none;
         border-radius: var(--radius-medium);
         padding: 8px 16px;
         font-weight: 600;
         cursor: pointer;
       }
     </style>
   </head>
   <body>
     <!-- Build your screen using semantic HTML -->
   </body>
   </html>
   ```

2. **Serve locally** and open in a browser to preview.

3. **Capture to Figma** — Use the Figma HTML-to-Design capture flow to import the rendered HTML as a Figma frame.

4. **Review** the captured frame in Figma.

### HTML Prototype Best Practices

- **Use CSS custom properties** not hardcoded hex values — `var(--brand-color)` not `#0078D4`
- **Use semantic class names** — they become readable Figma layer names
- **Use configured screen size** from `jarvis.config.yaml` (default: `1920×1080`)
- **Structure HTML cleanly** — DOM hierarchy maps to Figma layer hierarchy
- **Cannot traverse Shadow DOM** — use plain HTML, not Web Components
- **Include web font loading** if using custom fonts

### When This Workflow Is Sufficient
- Stakeholder presentations and design reviews
- Exploratory concepts and early-stage ideation
- User testing prototypes (use the HTML prototype directly)
- One-off screens that won't be iterated on in Figma
- Any case where visual fidelity matters more than component linkage

## Screen Size

Use the screen size configured in `jarvis.config.yaml` (default: **1920×1080**).

## Design Tokens

Map your design system tokens from `jarvis.config.yaml` to CSS custom properties:

| Token | CSS Variable | Default |
|-------|-------------|---------|
| Brand color | `--brand-color` | `#0078D4` |
| Font family | `--font-family` | `Inter` |
| Spacing unit | `--spacing-unit` | `4px` |
| Corner radius (small) | `--radius-small` | `4px` |
| Corner radius (medium) | `--radius-medium` | `8px` |
| Corner radius (large) | `--radius-large` | `12px` |

## Common Layout Patterns

### Screen shell (header + nav + content)
```html
<div class="screen" style="display:flex; flex-direction:column; width:1920px; height:1080px;">
  <header style="height:48px; background:var(--bg-surface); border-bottom:1px solid #e0e0e0;">
    <!-- Header content -->
  </header>
  <div style="display:flex; flex:1;">
    <nav style="width:280px; background:var(--bg-surface); border-right:1px solid #e0e0e0;">
      <!-- Navigation -->
    </nav>
    <main style="flex:1; padding:32px; overflow:auto;">
      <!-- Page content -->
    </main>
  </div>
</div>
```

### Card grid (2-3 cards in a row)
```html
<div style="display:grid; grid-template-columns:repeat(3, 1fr); gap:16px;">
  <div class="card" style="background:#fff; border-radius:8px; padding:24px;">
    <!-- Card content -->
  </div>
  <!-- More cards -->
</div>
```

### Form layout (stacked fields)
```html
<form style="display:flex; flex-direction:column; gap:16px; max-width:480px;">
  <div class="field">
    <label style="font-size:12px; font-weight:600;">Email address</label>
    <input type="text" style="width:100%; height:36px; border:1px solid #c0c0c0; border-radius:4px; padding:0 12px;">
  </div>
  <!-- More fields -->
</form>
```

## Design Quality Checklist

Before delivering any hi-fi screen:

- [ ] Colors use design tokens / CSS variables (no hardcoded hex)
- [ ] Typography uses configured font family and standard weights
- [ ] Spacing follows the spacing grid (configured `spacingUnit` increments)
- [ ] Visual hierarchy is clear (primary action stands out)
- [ ] Interactive states considered (hover, pressed, disabled, focus)
- [ ] Empty states included (if applicable)
- [ ] Text is realistic (not "Lorem ipsum")
- [ ] Screen size matches config
