---
name: narrative
description: "Transform structured content into compelling executive narratives using story arc frameworks. Use when users ask to 'create a narrative', 'write a narrative from', 'transform to story arc', 'apply corporate visions arc', 'generate insight summary', 'narrative transformation', or when other plugins need arc-driven narrative generation from input files."
---

# Narrative Transformation

Transform a set of input markdown files into a structured executive narrative using one of 5 story arc frameworks.

---

## Input Contract

The skill accepts a **source directory** containing:

1. **Markdown files** (`.md`) -- the content to transform into a narrative
2. **Optional metadata file** (`narrative-config.json` or frontmatter in input files) with:
   - `arc_id` -- explicit arc selection (overrides auto-detection)
   - `content_type` -- for auto-detection (e.g., "market", "technology", "competitive")
   - `language` -- output language: `en` (default) or `de`
   - `title` -- narrative title (auto-generated if omitted)
   - `subtitle` -- narrative subtitle
   - `output_path` -- where to write the output (defaults to `insight-summary.md` in source directory)

**Parameters:**
- `--source-path` (required) -- directory containing input .md files, or path to a single .md file
- `--arc-id` (optional) -- explicit arc selection, overrides auto-detection
- `--language` (optional) -- `en` or `de`, defaults to `en`
- `--output-path` (optional) -- output file path

---

## Output Contract

A single markdown file (`insight-summary.md` by default) containing:

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

---

## Core Workflow

```text
Setup → Arc Selection → Load Content → Narrative Transformation → Validation → Write Output
```

**MANDATORY:** Read each phase reference file BEFORE executing that phase.

### Phase 1: Setup & Content Loading

1. Validate `--source-path` exists
2. Load all `.md` files from source directory using Read tool
3. Look for `narrative-config.json` in source directory; if found, load it
4. Build CONTENT_REGISTRY: list of loaded files with titles, word counts, key sections

### Phase 2: Arc Selection

**Read:** [references/story-arc/arc-registry.md](references/story-arc/arc-registry.md)

1. If `--arc-id` provided, use it directly
2. If `narrative-config.json` contains `content_type`, apply detection algorithm from arc-registry
3. If neither, analyze loaded content for keyword density using detection algorithm
4. Fallback: `corporate-visions`
5. Present selected arc to user for confirmation using AskUserQuestion:
   - Show detected arc with reason
   - Offer alternatives
   - User confirms or overrides

**Store:** `arc_id`, `arc_display_name`, `detection_reason`

### Phase 3: Load Arc Patterns

**Read the arc-specific files:**
1. `references/story-arc/{arc_id}/arc-definition.md` -- element definitions, word targets, quality gates
2. `references/story-arc/{arc_id}/{element}-patterns.md` -- transformation patterns for each element (4 files)
3. `references/narrative-techniques/techniques-overview.md` -- narrative techniques reference

### Phase 4: Narrative Transformation

**Read arc-specific workflow:** `references/phase-workflows/phase-4b-synthesis-{arc_id}.md` (if exists)

For each arc element (4 elements):

1. **Map source content** to element using arc-definition source content mapping
2. **Apply arc-specific transformation patterns** from loaded pattern files
3. **Apply narrative techniques** (PSB, IS-DOES-MEANS, Number Plays, etc.) as specified by the arc
4. **Construct element** with:
   - Arc-specific header (localized if German)
   - Evidence-grounded body text
   - Inline citations: `<sup>[N](source-file.md)</sup>` format
   - Word count within element target (±50 words tolerance)
5. **Build transitions** between elements using arc-definition transition patterns

**Construct the full narrative:**
1. Generate compelling arc-specific title (not generic)
2. Write hook paragraph (150-200 words) using arc's hook construction pattern
3. Assemble 4 elements with transitions
4. Write closing using arc's closing pattern

### Phase 5: Validation

**Quality gates (all must pass):**

- [ ] Word count: 1,450-1,900 words total
- [ ] All 4 arc elements present with arc-specific headers
- [ ] Hook present (150-200 words)
- [ ] Element word counts within targets (±50 words tolerance)
- [ ] Arc-specific techniques applied (check arc quality gates)
- [ ] Citations present: minimum 15 total, format `<sup>[N](file.md)</sup>`
- [ ] Transitions between elements are smooth
- [ ] Title is arc-specific (not generic "Insight Summary")
- [ ] Frontmatter contains required fields
- [ ] Language correct (German umlauts if `de`, not ASCII fallbacks)

### Phase 6: Write Output

1. Write narrative to output path (default: `insight-summary.md` in source directory)
2. Verify file created with correct word count

**Return JSON summary:**
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

## Available Story Arcs

| Arc ID | Elements | Best For |
|--------|----------|----------|
| `corporate-visions` | Why Change → Why Now → Why You → Why Pay | Market research, B2B, sales enablement |
| `technology-futures` | Emerging → Converging → Possible → Required | Innovation, R&D, technology trends |
| `competitive-intelligence` | Landscape → Shifts → Positioning → Implications | Competitive analysis, threat assessment |
| `strategic-foresight` | Signals → Scenarios → Strategies → Decisions | Long-range planning, scenario analysis |
| `industry-transformation` | Forces → Friction → Evolution → Leadership | Industry analysis, regulatory impact |

See [references/story-arc/arc-registry.md](references/story-arc/arc-registry.md) for full details.

---

## Output Language

Generate ALL content in the specified language.

### German (de) Requirements

| Element | Requirement | Example |
|---------|-------------|---------|
| Body text | Proper umlauts | "fur" is WRONG, use "fur" |
| Section headings | Proper umlauts | "Ubersicht" is WRONG |
| File names/slugs | ASCII transliterations | u→ue, a→ae, o→oe, ss→ss |

**Reference:** See [references/language-templates.md](references/language-templates.md) for localized headers per arc.

---

## Citation Strategy

- Cite input source files, NOT fabricated references
- Format: `Claim text<sup>[N](source-file.md)</sup>`
- Every quantitative claim needs a citation
- Target: 15-25 total citations across the narrative
- Sequential numbering starting at 1

---

## Narrative Techniques

See [references/narrative-techniques/techniques-overview.md](references/narrative-techniques/techniques-overview.md) for the full technique library:

- **Pyramid Principle** -- Answer First architecture
- **PSB** -- Problem-Solution-Benefit for unconsidered needs
- **IS-DOES-MEANS** -- Power Position structure
- **Number Plays** -- 6 quantification techniques
- **Forcing Functions** -- Urgency through external pressures
- **Contrast Structure** -- Cognitive dissonance patterns
- **You-Phrasing** -- Direct address to reader
- **Compound Impact** -- Cost of inaction stacking

---

## Error Handling

| Phase | Failure | Action |
|-------|---------|--------|
| 1 | Source path not found | HALT with clear error |
| 1 | No .md files in source | HALT with error |
| 2 | Unknown arc_id | HALT with available arcs list |
| 3 | Arc pattern files missing | HALT with file list |
| 4 | Transformation fails | HALT with error JSON |
| 5 | Validation fails | Report failures, attempt fix, re-validate |

---

## Constraints

- DO NOT fabricate citations or evidence
- DO NOT generate content beyond what input files support
- ALWAYS cite back to input source files
- ALWAYS apply the selected arc's complete element structure
- ALWAYS respect word count targets per element
