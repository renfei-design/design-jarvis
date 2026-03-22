---
name: online-research
description: >
  Research any topic using live web sources before creating content or documentation.
  Use when asked to research, compare, analyze, or create content that requires
  up-to-date information beyond training data. Fetches authoritative sources,
  extracts structured data, and produces accurate, sourced content.
---

# Online Research — Speed-Optimized

Ground content in current facts with **minimum fetch calls**. The golden rule: **draft from training knowledge, fetch only to verify and update**.

## When to Use

- Content involves pricing, specs, rankings, dates, or statistics that change over time
- User explicitly asks to "research," "look up," "compare," or "find the latest"
- Creating a doc or brief where outdated info would hurt credibility

**Skip fetching when:** the topic is conceptual, historical, or opinion-based — training knowledge is sufficient.

## Speed Rules

1. **Max 2 `fetch_webpage` calls per research task** — hard cap
2. **Batch all URLs into one call** — `urls` accepts an array (3-6 URLs per call)
3. **Draft first, fetch second** — write full content from training knowledge, mark gaps, then fetch only for gaps
4. **Use sharp `query` strings** — focused query extracts faster, less noise
5. **Official sources only** — skip aggregators, comparison sites, and forums on first pass

## Workflow

### Step 1 — Triage (no fetching)

Split the question into two buckets:

| Bucket | Examples | Action |
|---|---|---|
| **Stable facts** | concepts, history, architecture | Use training knowledge |
| **Volatile facts** | pricing, versions, dates, availability | Must fetch |

If everything is stable → skip to output, no fetches needed.

### Step 2 — Draft from training knowledge

Write the full content structure **before fetching**:
- Headings, sections, table layout
- Fill in all stable facts
- Mark volatile facts with `[VERIFY]` placeholders

### Step 3 — Fetch (one call, multiple URLs)

```
fetch_webpage(
  urls: ["https://product-a.com/pricing", "https://product-b.com/pricing"],
  query: "pricing tiers, per-seat cost, free tier limits"
)
```

**URL selection:**

| Source type | Reliability | Use when |
|---|---|---|
| `/pricing` pages | ★★★ | Price comparisons |
| `/docs` or `/features` | ★★★ | Feature comparisons |
| Blog / changelog | ★★☆ | Announcements |
| Wikipedia | ★★☆ | Background context |
| News articles | ★★☆ | Recent events |
| Gated pages (login required) | ✗ | Never |

### Step 4 — Patch the draft

Replace `[VERIFY]` placeholders with fetched data:
1. Update numbers, dates, facts
2. Add source URLs as inline citations
3. If source contradicts training knowledge, **use fetched data** (newer)
4. If fetch returned nothing useful — use training knowledge with caveat

### Step 5 — Second fetch (only if critical gaps remain)

Only make a second call if:
- A primary URL was gated or empty
- Two sources conflict and a third can resolve it
- A high-stakes data point couldn't be verified

## Research Patterns

### Price Comparison
**1 fetch call.** Batch all vendor pricing URLs. Draft table from training knowledge. Patch prices.

### Feature Comparison
**1 fetch call.** Batch official feature pages. Use training knowledge for framework. Fetch for specifics.

### Market Analysis
**1 fetch call.** Training knowledge for narrative. Fetch one high-quality article for current numbers.

### Technical Deep-Dive
**Usually 0 fetches.** Training knowledge covers architecture well. Fetch only for current API details.

## Quality Checklist

- [ ] Volatile data verified from fetched sources
- [ ] Source URLs cited inline
- [ ] Used ≤2 `fetch_webpage` calls
- [ ] Pricing/dates include "as of" timestamp
- [ ] Gaps disclosed where applicable

## Anti-Patterns

- ✗ Fetching 4+ pages one at a time — batch them
- ✗ Fetching to confirm what training knowledge already covers
- ✗ Fetching aggregator sites when official pages are available
- ✗ Reading entire pages for one data point — use sharp `query`
- ✗ Exploratory fetches before knowing what you need — draft first
