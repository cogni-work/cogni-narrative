# Format Templates

Detailed guidelines and edge cases for each derivative format.

## General Principles

1. **Fidelity over creativity** -- Derivative formats condense; they do not embellish
2. **Arc integrity** -- All 4 elements preserved in every format, in order
3. **Evidence priority** -- Quantitative claims are the last thing to cut
4. **Language inheritance** -- Derivative matches source language exactly

---

## Executive Brief

### Template Structure

```markdown
---
type: executive-brief
source: "{source filename}"
arc_id: "{arc_id}"
word_count: {300-500}
language: "{en|de}"
date_created: "{ISO 8601}"
---

# {Title}

*{Subtitle}*

{Condensed hook -- 2-3 sentences distilling the opening tension}

---

## {Element 1 Header}

{75-125 words: core claim + 1-2 key evidence points with citations}

## {Element 2 Header}

{60-100 words: core claim + 1-2 key evidence points with citations}

## {Element 3 Header}

{75-125 words: core claim + 1-2 key evidence points with citations}

## {Element 4 Header}

{50-80 words: core claim + call to action with citations}
```

**Rules:**
- Preserve all 4 `##` headers using exact arc element names
- Maintain citation format `<sup>[N](file.md)</sup>` -- renumber sequentially
- Keep the arc's rhetorical progression intact
- Prioritize quantitative evidence over qualitative description

### Word Budget

| Component | Words |
|-----------|-------|
| Hook (condensed) | 30-50 |
| Element 1 | 75-125 |
| Element 2 | 60-100 |
| Element 3 | 75-125 |
| Element 4 | 50-80 |
| **Total** | **300-500** |

### Condensation Strategy

For each arc element:

1. **Identify the lead claim** -- Usually the first or second sentence of the original section
2. **Select top 2 evidence points** -- Prefer quantitative over qualitative
3. **Compress supporting text** -- Remove examples, analogies, and extended explanations
4. **Preserve transitions** -- Keep 1-sentence transition logic between sections
5. **Renumber citations** -- Sequential from 1, only citations that survive condensation

### Citation Handling

- Keep `<sup>[N](file.md)</sup>` format
- Renumber sequentially starting from 1
- Only include citations that accompany evidence retained in the brief
- Target: 8-12 citations (roughly half of full narrative)

### Common Pitfalls

- Cutting all evidence and leaving only assertions (too abstract)
- Keeping too much text from one element, starving others
- Losing the arc's rhetorical progression (each element must still lead to the next)
- Dropping the hook entirely (brief still needs an opening frame)

---

## Talking Points

### Template Structure

```markdown
---
type: talking-points
source: "{source filename}"
arc_id: "{arc_id}"
language: "{en|de}"
date_created: "{ISO 8601}"
---

# Talking Points: {Title}

**Arc:** {Arc Display Name} | **Source:** {source filename}

---

## {Element 1 Header}

- {Core message -- 1 sentence}
- {Supporting evidence with number/stat}
- {Supporting evidence with number/stat}

## {Element 2 Header}

- {Core message -- 1 sentence}
- {Supporting evidence with number/stat}
- {Supporting evidence with number/stat}

## {Element 3 Header}

- {Core message -- 1 sentence}
- {Supporting evidence with number/stat}
- {Supporting evidence with number/stat}
- {Key differentiator or capability}

## {Element 4 Header}

- {Core message -- 1 sentence}
- {Key metric or ROI point}
- {Call to action}

---

**Key Numbers:**

- {Most impactful stat from element 1}
- {Most impactful stat from element 2}
- {Most impactful stat from element 3}
- {ROI or payoff stat from element 4}
```

**Rules:**
- Each element gets 3-4 bullets maximum
- Lead each element with the core message (answer-first)
- Include the strongest quantitative evidence per element
- End with "Key Numbers" section pulling the hero stats
- No citations in bullets -- keep clean for verbal delivery
- End element 4 with an actionable call-to-action bullet

### Structure Rules

- **3-4 bullets per element** (no more)
- **Lead bullet = core message** (answer-first, one complete sentence)
- **Supporting bullets = evidence** (stat + context, not full sentences)
- **No citations** -- bullets are for verbal delivery
- **Key Numbers section** pulls 4 hero stats (one per element)

### Bullet Writing Guidelines

**Core message bullets:**
- Start with the insight, not the background
- One sentence, 15-25 words
- Active voice, direct
- Example: "Digital transformation spending will exceed $3.4 trillion by 2026, yet 70% of initiatives fail to meet objectives."

**Evidence bullets:**
- Lead with the number
- Provide just enough context
- 8-15 words per bullet
- Example: "70% failure rate in digital transformation initiatives (McKinsey 2024)"

**Call-to-action bullet (Element 4 only):**
- Imperative form
- Specific and time-bound where possible
- Example: "Pilot the integrated monitoring approach in Q3 with 2 business units"

### Key Numbers Section

Select the single most impactful statistic from each element. Criteria:
1. **Surprising** -- challenges assumptions
2. **Specific** -- exact number, not "many" or "significant"
3. **Relevant** -- directly supports the element's core message
4. **Memorable** -- easy to quote in conversation

---

## One-Pager

### Template Structure

```markdown
---
type: one-pager
source: "{source filename}"
arc_id: "{arc_id}"
word_count: {400-600}
language: "{en|de}"
date_created: "{ISO 8601}"
---

# {Title}

> {1-2 sentence executive summary distilling the entire narrative}

---

| Key Metric | Value |
|------------|-------|
| {Metric 1 label} | {Value with unit} |
| {Metric 2 label} | {Value with unit} |
| {Metric 3 label} | {Value with unit} |
| {Metric 4 label} | {Value with unit} |

---

## {Element 1 Header}

{2-3 sentences: core finding and primary evidence}

## {Element 2 Header}

{2-3 sentences: core finding and primary evidence}

## {Element 3 Header}

{2-3 sentences: core finding and primary evidence}

## {Element 4 Header}

{2-3 sentences: core finding with call to action}

---

**Next Steps:**

1. {Concrete action item derived from the narrative}
2. {Concrete action item derived from the narrative}
3. {Concrete action item derived from the narrative}

---

*Source: {source filename} | Arc: {Arc Display Name} | Generated: {date}*
```

**Rules:**
- Executive summary is the single most important sentence
- Key metrics table pulls the 4 strongest numbers (one per element)
- Each element section is exactly 2-3 sentences
- "Next Steps" provides 3 actionable items
- Citations are omitted for clean print layout
- Footer references the source for traceability

### Layout Constraints

Designed for a single printed page (approximately 400-600 words with table and formatting).

### Key Metrics Table

| Rule | Detail |
|------|--------|
| Exactly 4 rows | One metric per arc element |
| Metric label | Short (2-5 words), descriptive |
| Value | Number + unit, no sentences |
| Source | Drawn from source narrative's cited evidence |

**Example table:**

| Key Metric | Value |
|------------|-------|
| Market opportunity | EUR 47B by 2028 |
| Implementation timeline | 6-8 months |
| Competitive advantage window | 18 months |
| Expected ROI | 3.2x in year 1 |

### Section Writing

Each element section: exactly 2-3 sentences.
- Sentence 1: Core finding (what)
- Sentence 2: Key evidence (how much / how fast)
- Sentence 3 (optional): Implication (so what)

### Next Steps

Derive 3 action items from the narrative's fourth element (the action/decision element in every arc):
- Concrete and specific
- Time-bound where possible
- Progressively scoped (quick win, medium effort, strategic investment)

### Footer

Always include source attribution:
```
*Source: {filename} | Arc: {Arc Display Name} | Generated: {YYYY-MM-DD}*
```
