---
name: competitive-ux
description: >
  Research and generate a competitive UX analysis report focused on how competitors solve
  a given feature area. Use when asked to do competitive research, competitor analysis,
  market research, UX benchmarking, or compare how competitors implement a feature.
  Fetches public product docs, blogs, and community discussions, synthesizes findings,
  and builds a polished report in Figma.
---

# Competitive UX Analysis

Research how competitors solve a specific feature area and generate a structured analysis report.

## When to Use

- User asks to "research competitors" or "do competitive analysis"
- User asks "how do others handle {feature}?"
- Before starting design on a feature — understand the landscape first
- As input to a UX brief or design direction

## Workflow

### Step 1 — Identify Competitors

Based on the feature area in question, identify **4-6 relevant competitors**:

| Tier | Description | Examples |
|------|-------------|---------|
| **Direct** | Same product category, direct competition | Products in the same market segment |
| **Adjacent** | Different category but similar feature | Products with overlapping functionality |
| **Aspirational** | Best-in-class UX, any category | Products known for exceptional design of this feature type |

**Selection criteria:**
- Must have the feature area publicly visible (docs, marketing pages, screenshots)
- Prefer well-known products with established patterns (users expect these patterns)
- Include at least 1 aspirational example from outside the direct market

### Step 2 — Research (use online-research skill)

For each competitor, gather:

1. **Feature approach** — how do they solve the problem?
2. **Information architecture** — how is the feature organized?
3. **Key patterns** — what interaction patterns do they use?
4. **Strengths** — what works well?
5. **Weaknesses** — what's missing or poorly done?

**Sources (priority order):**
1. Official product documentation / feature pages
2. Product blogs / changelogs / release notes
3. Community forums / discussions
4. Published case studies or reviews
5. Training knowledge (for established products)

Use `fetch_webpage` to get current data. Batch URLs into a single call when possible.

### Step 3 — Synthesize Findings

Organize research into this structure:

```markdown
## Competitive UX Analysis: {Feature Area}

### Executive Summary
{2-3 sentences: what's the competitive landscape for this feature?}

### Competitor Matrix

| Capability | Competitor A | Competitor B | Competitor C | Competitor D |
|-----------|-------------|-------------|-------------|-------------|
| {capability 1} | ✓ / ✗ / ◐ | ... | ... | ... |
| {capability 2} | ... | ... | ... | ... |
| {capability 3} | ... | ... | ... | ... |

Legend: ✓ = Full support, ◐ = Partial, ✗ = Not supported

### Detailed Analysis

#### {Competitor A}
- **Approach:** {how they solve it}
- **Strengths:** {what works}
- **Weaknesses:** {what doesn't}
- **Notable pattern:** {specific UX pattern worth noting}

#### {Competitor B}
...

### Pattern Analysis

**Common patterns** (≥3 competitors use):
- {pattern} — used by: A, B, C

**Emerging patterns** (1-2 competitors, newer approach):
- {pattern} — used by: D

**Anti-patterns** (approaches with known problems):
- {anti-pattern} — seen in: B — problem: {why it fails}

### Recommendations

**Adopt** (high confidence):
- {recommendation with specific rationale}

**Explore** (medium confidence):
- {recommendation that needs validation}

**Avoid** (evidence against):
- {thing to skip and why}

### Sources
- [{Source title}]({URL}) — accessed {date}
```

### Step 4 — Build in Figma (Optional)

If the user wants the report in Figma, create a documentation page with the research findings using a two-column layout (heading on left, body on right).

## Research Quality Checklist

- [ ] 4-6 competitors analyzed (mix of direct, adjacent, aspirational)
- [ ] Each competitor has strengths AND weaknesses (not just a feature list)
- [ ] Patterns are grouped by commonality (common vs. emerging vs. anti-pattern)
- [ ] Recommendations are specific and actionable (not generic "do good UX")
- [ ] Sources are cited with URLs and dates
- [ ] Analysis is current (check for recent product changes)

## Tips

- **Don't just list features** — analyze the UX approach (how it works, not just what exists)
- **Look for non-obvious competitors** — the best pattern for "settings page" might come from a consumer app, not a direct competitor
- **Note user sentiment** — community forums often reveal what users love/hate about a competitor's approach
- **Screenshot competitor UIs** if possible — visual references make the analysis more actionable for the design team
- **Time-bound your research** — competitive analysis can expand infinitely. Set a hard cap of 2 `fetch_webpage` calls and fill gaps from training knowledge
