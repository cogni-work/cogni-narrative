---
name: narrative
description: "Transform structured content into compelling executive narratives using story arc frameworks. Use when users ask to 'create a narrative', 'write a narrative from', 'transform to story arc', 'apply corporate visions arc', 'generate insight summary', 'narrative transformation', or when other plugins need arc-driven narrative generation from input files."
---

# Narrative Transformation

## Purpose

Transform input markdown files into a structured executive narrative (1,450-1,900 words) using one of 5 story arc frameworks. Each arc provides a distinct rhetorical progression — mapping source evidence to arc elements, applying narrative techniques, and producing a citation-grounded insight summary.

## When to Use

- Transform research syntheses, analyses, or structured findings into executive narratives
- Apply a specific story arc framework (Corporate Visions, Technology Futures, etc.)
- Generate an insight summary from a set of markdown files
- Create arc-driven narratives for reports, presentations, or strategic communication

**Not for:**
- Editing or polishing existing narratives (use copywriter skill instead)
- Creating slide decks from narratives (use story-to-slides skill instead)
- Raw research or data collection (use deeper-research skills instead)

---

## Parameters

| Parameter | Required | Description |
|-----------|----------|-------------|
| `--source-path` | Yes | Directory containing input `.md` files, or path to a single `.md` file |
| `--arc-id` | No | Explicit arc selection; overrides auto-detection |
| `--language` | No | Output language: `en` (default) or `de` |
| `--output-path` | No | Output file path; defaults to `insight-summary.md` in source directory |
| `--project-path` | No | Research project directory; enables loading entity data beyond source path |
| `--research-question` | No | Original research question for narrative hook framing |
| `--content-map` | No | YAML map of content category keys to file/directory paths for additional context |

**Content map keys:** `executive_summary`, `dimension_syntheses`, `trends_summary`, `trend_entities`, `megatrends_summary`, `megatrend_entities`, `domain_concepts`, `research_hub`, `initial_question`

---

## Output

A single markdown file (`insight-summary.md` by default):

```markdown
---
title: "{Arc-Specific Compelling Title}"
subtitle: "{Research Question or Topic}"
arc_id: "{selected-arc}"
arc_display_name: "{Arc Display Name}"
word_count: {1450-1900}
language: "{en|de}"
date_created: "{ISO 8601}"
source_file_count: {N}
---

# {Title}

*{Subtitle}*

{Opening paragraph with narrative hook -- 150-200 words}

---

## {Element 1 Header}

{350-500 words with evidence grounding}

## {Element 2 Header}

{300-450 words with evidence grounding}

## {Element 3 Header}

{350-500 words with evidence grounding}

## {Element 4 Header}

{200-350 words with evidence grounding}
```

**Word count target:** 1,450-1,900 words total.

**JSON summary returned on completion:**

```json
{
  "success": true,
  "output_path": "insight-summary.md",
  "arc_id": "corporate-visions",
  "arc_display_name": "Corporate Visions",
  "word_count": 1650,
  "citation_count": 22,
  "elements": 4,
  "language": "en"
}
```

---

## Core Workflow

```text
Setup → Arc Selection → Pattern Loading → Transformation → Validation → Write
```

**MANDATORY:** Read each phase reference file BEFORE executing that phase.

### Phase 1: Setup & Content Loading

1. Validate `--source-path` exists; HALT if not found
2. Load all `.md` files from source directory using Read tool
3. Load `narrative-config.json` from source directory if present
4. If `--content-map` provided, load additional files from each path:
   - Directory paths: load all `.md` files
   - File paths: load that specific file
   - Glob patterns: expand and load matches
   - Tag each file in CONTENT_REGISTRY with its content-map key
   - Skip non-existent paths with warning (non-blocking)
5. If `--research-question` provided, store in CONTENT_REGISTRY for hook construction
6. Build CONTENT_REGISTRY: list of loaded files with titles, word counts, key sections, category tags

### Phase 2: Arc Selection

**Read:** [references/story-arc/arc-registry.md](references/story-arc/arc-registry.md)

1. If `--arc-id` provided, use it directly
2. If `narrative-config.json` contains `content_type`, apply detection algorithm from arc-registry
3. If neither, analyze loaded content for keyword density using detection algorithm
4. Fallback: `corporate-visions`
5. Present selected arc to user for confirmation using AskUserQuestion:
   - Show detected arc with detection reason
   - Offer alternatives
   - Accept user confirmation or override

**Store:** `arc_id`, `arc_display_name`, `detection_reason`

### Phase 3: Load Arc Patterns

Read the arc-specific files:

1. `references/story-arc/{arc_id}/arc-definition.md` -- element definitions, word targets, quality gates
2. `references/story-arc/{arc_id}/{element}-patterns.md` -- transformation patterns per element (4 files)
3. `references/narrative-techniques/techniques-overview.md` -- narrative techniques reference

### Phase 4: Narrative Transformation

**Read:** `references/phase-workflows/phase-4b-synthesis-{arc_id}.md` (if exists)

For each of the 4 arc elements:

1. **Map source content** to element using arc-definition source content mapping
2. **Apply transformation patterns** from loaded pattern files
3. **Apply narrative techniques** (PSB, IS-DOES-MEANS, Number Plays, etc.) per the technique-arc matrix
4. **Construct element:**
   - Arc-specific header (localized if `de`)
   - Evidence-grounded body text
   - Inline citations: `<sup>[N](source-file.md)</sup>` format
   - Word count within element target (+/-50 words)
5. **Build transitions** between elements using arc-definition transition patterns

Assemble the full narrative:

1. Generate arc-specific compelling title (not generic)
2. Write hook paragraph (150-200 words) using arc's hook construction pattern
3. Assemble 4 elements with transitions
4. Write closing using arc's closing pattern

### Phase 5: Validation

All quality gates must pass:

- [ ] Total word count: 1,450-1,900
- [ ] All 4 arc elements present with arc-specific headers
- [ ] Hook present (150-200 words)
- [ ] Element word counts within targets (+/-50 words)
- [ ] Arc-specific techniques applied (check arc quality gates)
- [ ] Citations: minimum 15, format `<sup>[N](file.md)</sup>`
- [ ] Smooth transitions between elements
- [ ] Title is arc-specific (not generic "Insight Summary")
- [ ] Frontmatter contains all required fields
- [ ] Language correct: proper umlauts for `de` (ä, ö, ü, ß), ZERO ASCII fallbacks (ae, oe, ue, ss) in body text

If validation fails: report failures, fix, re-validate.

### Phase 6: Write Output

1. Write narrative to output path (default: `insight-summary.md` in source directory)
2. Verify file created with correct word count
3. Return JSON summary (see Output section above)

---

## Available Story Arcs

| Arc ID | Elements | Best For |
|--------|----------|----------|
| `corporate-visions` | Why Change -> Why Now -> Why You -> Why Pay | Market research, B2B, sales enablement |
| `technology-futures` | Emerging -> Converging -> Possible -> Required | Innovation, R&D, technology trends |
| `competitive-intelligence` | Landscape -> Shifts -> Positioning -> Implications | Competitive analysis, threat assessment |
| `strategic-foresight` | Signals -> Scenarios -> Strategies -> Decisions | Long-range planning, scenario analysis |
| `industry-transformation` | Forces -> Friction -> Evolution -> Leadership | Industry analysis, regulatory impact |

See [references/story-arc/arc-registry.md](references/story-arc/arc-registry.md) for detection signals, word targets, and extension guidelines.

---

## Output Language

Generate ALL narrative content in the specified language (`--language` parameter).

### German (`de`) -- MANDATORY Umlaut Rules

**CRITICAL: When `language: de`, you MUST use proper Unicode umlauts (ä, ö, ü, Ä, Ö, Ü, ß) in ALL generated text. ASCII transliterations (ae, oe, ue, ss) in body text are a BLOCKING DEFECT.**

| Element | Rule | Correct | WRONG |
|---------|------|---------|-------|
| Body text | Proper umlauts | für, über, Änderung, größte | fuer, ueber, Aenderung, groesste |
| Section headings | Proper umlauts | Übersicht, Kräfte, Führung | Uebersicht, Kraefte, Fuehrung |
| Frontmatter `title`, `subtitle` | Proper umlauts | "Maschinenbau: Drei Kräfte" | "Maschinenbau: Drei Kraefte" |
| Frontmatter `research_question` | Proper umlauts | "Welche Trends beeinflussen" | "Welche Trends beeinflussen" |
| File names, slugs | ASCII transliterations | ue, ae, oe, ss | ü, ä, ö, ß |
| YAML keys, identifiers | ASCII only | arc_id, entity_type | -- |

**Self-check after generation:** Scan the complete output for these ASCII patterns that indicate umlaut failure: `ue` (should be `ü`), `ae` (should be `ä`), `oe` (should be `ö`), `ss` where `ß` is correct. Common failures: "fuer"→"für", "ueber"→"über", "Aenderung"→"Änderung", "groesste"→"größte", "Fuehrung"→"Führung", "Uebergangsfrist"→"Übergangsfrist", "Praezision"→"Präzision", "Massnahme"→"Maßnahme".

See [references/language-templates.md](references/language-templates.md) for localized headers per arc.

---

## Citation Strategy

- Cite input source files only -- never fabricate references
- Format: `Claim text<sup>[N](source-file.md)</sup>`
- Every quantitative claim requires a citation
- Target: 15-25 total citations, sequentially numbered from 1

---

## Narrative Techniques

See [references/narrative-techniques/techniques-overview.md](references/narrative-techniques/techniques-overview.md) for the full library:

| Technique | Purpose |
|-----------|---------|
| Pyramid Principle | Answer First architecture |
| PSB | Problem-Solution-Benefit for unconsidered needs |
| IS-DOES-MEANS | Power Position structure |
| Number Plays | 6 quantification techniques |
| Forcing Functions | Urgency through external pressures |
| Contrast Structure | Cognitive dissonance patterns |
| You-Phrasing | Direct address to reader |
| Compound Impact | Cost of inaction stacking |

---

## Error Handling

| Phase | Failure | Action |
|-------|---------|--------|
| 1 | Source path not found | HALT with clear error message |
| 1 | No `.md` files in source | HALT with error |
| 2 | Unknown `arc_id` | HALT with available arcs list |
| 3 | Arc pattern files missing | HALT with missing file list |
| 4 | Transformation fails | HALT with error JSON |
| 5 | Validation fails | Report failures, fix, re-validate |

On any HALT, return error JSON:

```json
{
  "success": false,
  "error": "Description of what went wrong",
  "phase": "Phase where failure occurred"
}
```

---

## Constraints

- DO NOT fabricate citations or evidence
- DO NOT generate content beyond what input files support
- ALWAYS cite back to input source files
- ALWAYS apply the selected arc's complete 4-element structure
- ALWAYS respect word count targets per element (+/-50 words)

---

## Bundled Resources

### References

| File | Purpose | Load When |
|------|---------|-----------|
| `references/story-arc/arc-registry.md` | Arc index, detection algorithm, extension guide | Phase 2 |
| `references/story-arc/{arc_id}/arc-definition.md` | Element definitions, word targets, quality gates | Phase 3 |
| `references/story-arc/{arc_id}/*-patterns.md` | Transformation patterns per element (4 files) | Phase 3 |
| `references/narrative-techniques/techniques-overview.md` | 8 narrative techniques with arc application matrix | Phase 3 |
| `references/phase-workflows/narrative-transformation.md` | General transformation pipeline | Phase 4 |
| `references/phase-workflows/phase-4b-synthesis-{arc_id}.md` | Arc-specific transformation workflow | Phase 4 |
| `references/language-templates.md` | Localized headers for en/de | Phase 4 (if `de`) |
