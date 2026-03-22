---
name: wireframe
description: >
  Build low-fidelity wireframe screens in Figma. Default: HTML prototype captured to Figma.
  Use when asked to wireframe, sketch, prototype, create lo-fi screens, or build wireframes.
---

# Wireframe Builder

Create low-fidelity wireframe screens in Figma. **Default workflow: HTML prototype → Figma capture.** This produces clean wireframes fast using grayscale styling and placeholder content.

## Workflow — HTML Prototype to Figma

Create an HTML prototype and capture it to Figma. See the `hifi-design` skill for the full HTML-to-Figma workflow steps. For wireframes, use simpler styling:

- **Grayscale palette** — `#242424`, `#616161`, `#e0e0e0`, `#f5f5f5`, `#ffffff`
- **Placeholder boxes** — simple rectangles with labels for images, illustrations, icons
- **System font** — no custom web font needed; default sans-serif is fine for wireframes
- **No shadows or gradients** — keep it flat

## Common Layout Patterns

### Two-column layout (main + sidebar)
```html
<div style="display:flex; width:1440px; height:900px;">
  <main style="flex:1; padding:32px;">
    <!-- Main content -->
  </main>
  <aside style="width:380px; border-left:1px solid #e0e0e0; padding:24px;">
    <!-- Sidebar -->
  </aside>
</div>
```

### Form field (label + input placeholder)
```html
<div style="display:flex; flex-direction:column; gap:4px;">
  <label style="font-size:12px; font-weight:700; color:#424242;">Label</label>
  <div style="height:36px; border:1px solid #c0c0c0; border-radius:4px; padding:0 12px; display:flex; align-items:center;">
    <span style="color:#999;">Placeholder text</span>
  </div>
</div>
```

### Card container
```html
<div style="background:#fff; border-radius:8px; padding:24px; display:flex; flex-direction:column; gap:16px;">
  <!-- Card content -->
</div>
```

## Screen Size

**Default: 1440×900** for wireframes (smaller than hi-fi to emphasize structure over detail).

Override with the screen size from `jarvis.config.yaml` if configured.

## Wireframe Style Guidelines

- **Colors**: Grayscale only — white `#ffffff`, light gray `#f5f5f5`, medium gray `#e0e0e0`, dark text `#242424`, secondary `#424242`, muted `#6b6b6b`. No brand colors unless requested.
- **Typography**: System font (Inter or similar). Weights: 400 Regular, 700 Bold.
- **Font sizes**: 28 page title, 20 section heading, 16 body, 14 secondary, 12 caption.
- **Spacing**: 8px grid. Common values: 4, 8, 12, 16, 24, 32, 48.
- **Corner radius**: 4-8px for cards/inputs.

## Design Principles

- **Structure first** — nail the layout before thinking about visuals
- **Content-driven** — use realistic content lengths, not "Lorem ipsum"
- **Progressive disclosure** — show essentials, hide complexity
- **Consistency** — reuse patterns within the project
- **Create 2-3 variants** unless directed otherwise, each exploring a different approach
