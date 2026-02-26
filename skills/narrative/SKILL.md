---
name: narrative
description: "Transform structured content into compelling executive narratives using story arc frameworks. Use when users ask to 'create a narrative', 'write a narrative from', 'transform to story arc', 'apply corporate visions arc', 'generate insight summary', 'narrative transformation', or when other plugins need arc-driven narrative generation from input files."
---

# Narrative Transformation

## Purpose

Transform input markdown files into a structured executive narrative (1,450-1,900 words) using one of 5 story arc frameworks. Each arc provides a distinct rhetorical progression -- mapping source evidence to arc elements, applying narrative techniques, and producing a citation-grounded insight summary.

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

## Immediate Action: Initialize TodoWrite

Before reading any workflow phase, initialize phase-level tracking:

```
TodoWrite:
- [ ] Phase 1: Setup & Content Loading
- [ ] Phase 2: Arc Selection
- [ ] Phase 3: Load Arc Patterns
- [ ] Phase 4: Narrative Transformation
- [ ] Phase 5: Validation
- [ ] Phase 6: Write Output
```

**Progressive expansion:** Each phase adds step-level todos when started. Initial count: 6 phase-level todos. Final count: ~18-24 step-level todos across all phases.

---

## Execution Protocol

⛔ **MANDATORY:** Read each phase reference file BEFORE executing that phase. Do NOT skip reference reads -- transformation quality depends entirely on loading arc patterns and techniques before writing.

**Workflow enforcement:**
1. Mark each phase todo as `[in_progress]` before starting
2. Read the required reference file(s) for that phase
3. Execute all phase steps
4. Answer self-verification questions (where present)
5. Complete the phase completion checklist
6. Mark the phase todo as `[completed]` before proceeding

---

## Core Workflow

```text
Phase 1      Phase 2        Phase 3          Phase 4            Phase 5       Phase 6
Setup  --->  Arc      --->  Pattern   --->   Transformation --> Validation -> Write
& Load       Selection      Loading          (arc-specific)
  |            |               |                  |                |            |
  v            v               v                  v                v            v
CONTENT     arc_id        arc-definition     insight-summary   quality      output
REGISTRY    confirmed     + patterns         draft             gates        file
                          + techniques                         pass         + JSON
```

### Phase 1: Setup & Content Loading

**Mark todo:** `Phase 1: Setup & Content Loading` → `[in_progress]`

**Expand phase todos:**
```
TodoWrite:
- [in_progress] Phase 1: Setup & Content Loading
  - [ ] 1.1: Validate source path
  - [ ] 1.2: Load source .md files
  - [ ] 1.3: Load narrative-config.json (if present)
  - [ ] 1.4: Load content-map files (if provided)
  - [ ] 1.5: Build CONTENT_REGISTRY
```

**Steps:**

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

**Content-based checkpoint -- verify comprehension:**

> After building CONTENT_REGISTRY, answer these questions from the loaded content:
> 1. How many source files were loaded? (must be >= 1)
> 2. What is the approximate total word count across all sources?
> 3. What are the 2-3 dominant themes or topics in the source material?
>
> If unable to answer any question, Phase 1 is INCOMPLETE -- re-read source files.

**Self-verification:**

- [ ] Source path validated and exists? (YES/NO)
- [ ] At least 1 `.md` file loaded? (YES/NO)
- [ ] CONTENT_REGISTRY built with file titles, word counts, and category tags? (YES/NO)

⛔ If any answer is NO: STOP. Fix the issue before proceeding to Phase 2.

**Mark todo:** `Phase 1` → `[completed]`

---

### Phase 2: Arc Selection

**Phase entry gate:**
> Verify Phase 1 completed: CONTENT_REGISTRY must exist with >= 1 loaded file.
> If CONTENT_REGISTRY is empty or undefined: STOP. Return to Phase 1.

**Mark todo:** `Phase 2: Arc Selection` → `[in_progress]`

⛔ **MANDATORY -- Read before executing:**

**Read:** [references/story-arc/arc-registry.md](references/story-arc/arc-registry.md)

**Steps:**

1. If `--arc-id` provided, use it directly
2. If `narrative-config.json` contains `content_type`, apply detection algorithm from arc-registry
3. If neither, analyze loaded content for keyword density using detection algorithm
4. Fallback: `corporate-visions`
5. Present selected arc to user for confirmation using AskUserQuestion:
   - Show detected arc with detection reason
   - Offer alternatives
   - Accept user confirmation or override

**Store:** `arc_id`, `arc_display_name`, `detection_reason`

**Self-verification:**

- [ ] Arc registry reference read before executing detection? (YES/NO)
- [ ] `arc_id` resolved (explicit, detected, or fallback)? (YES/NO)
- [ ] `arc_display_name` and `detection_reason` stored? (YES/NO)
- [ ] User confirmation obtained (or arc was explicitly provided)? (YES/NO)

⛔ If any answer is NO: STOP. Fix before proceeding to Phase 3.

**Mark todo:** `Phase 2` → `[completed]`

---

### Phase 3: Load Arc Patterns

**Phase entry gate:**
> Verify Phase 2 completed: `arc_id` must be set to a valid arc identifier.
> Valid values: `corporate-visions`, `technology-futures`, `competitive-intelligence`, `strategic-foresight`, `industry-transformation`.
> If `arc_id` is unset or invalid: STOP. Return to Phase 2.

**Mark todo:** `Phase 3: Load Arc Patterns` → `[in_progress]`

**Expand phase todos:**
```
TodoWrite:
- [in_progress] Phase 3: Load Arc Patterns
  - [ ] 3.1: Read arc-definition.md
  - [ ] 3.2: Read techniques-overview.md
```

⛔ **MANDATORY -- Read the following before proceeding to Phase 4:**

1. `references/story-arc/{arc_id}/arc-definition.md` -- element definitions, word targets, quality gates
2. `references/narrative-techniques/techniques-overview.md` -- narrative techniques reference

**Note:** The 4 individual element pattern files (`{element}-patterns.md`) are NOT loaded here. Their guidance is already embedded in the arc-specific Phase 4b workflow file, which is loaded at the start of Phase 4. Loading both would create ~1,500 lines of overlapping reference material that dilutes rather than reinforces the structural constraint.

**Content-based checkpoint -- prove pattern comprehension:**

> After reading the reference files, answer from loaded content:
> 1. Name all 4 arc elements in order for the selected arc.
> 2. What is the word target for each element?
> 3. Which narrative techniques apply to which elements? (consult the technique-arc matrix)
>
> If unable to answer any question, re-read the reference files. Do NOT proceed to Phase 4 without this knowledge.

**Self-verification:**

- [ ] Arc definition file read for `{arc_id}`? (YES/NO)
- [ ] Techniques overview read? (YES/NO)
- [ ] Can name all 4 elements with word targets from memory? (YES/NO)

⛔ If any answer is NO: STOP. Read the missing file(s) before proceeding.

**Mark todo:** `Phase 3` → `[completed]`

---

### Phase 4: Narrative Transformation

**Phase entry gate:**
> Verify Phase 3 completed: arc-definition and techniques-overview must have been read.
> Quick check: Can you name all 4 arc elements and their word targets without re-reading? If NO: return to Phase 3.

**Mark todo:** `Phase 4: Narrative Transformation` → `[in_progress]`

**Expand phase todos:**
```
TodoWrite:
- [in_progress] Phase 4: Narrative Transformation
  - [ ] 4.1: Read arc-specific workflow (if exists)
  - [ ] 4.2: Map source content to arc elements
  - [ ] 4.3: Transform each element with patterns and techniques
  - [ ] 4.4: Assemble full narrative (title, hook, elements, closing)
```

⛔ **MANDATORY -- Read the arc-specific workflow before transforming:**

**Read:** `references/phase-workflows/phase-4b-synthesis-{arc_id}.md` (if exists)

This file contains detailed sub-steps, extended thinking prompts, and quality gates specific to the selected arc. If the file exists, follow its workflow instead of the summary below.

**STRUCTURAL CONSTRAINT (all arcs, all languages):**

The output MUST contain EXACTLY 4 `##` section headers matching the selected arc's
element names. No creative alternatives, no additional `##` sections, no renaming.
See `references/language-templates.md` section "Insight Summary (Arc Element Headers)"
for the exact header text per arc and language.

**Summary workflow (use when no arc-specific file exists):**

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

**Self-verification:**

- [ ] Arc-specific workflow file read (or confirmed non-existent)? (YES/NO)
- [ ] All 4 elements written with arc-specific headers? (YES/NO)
- [ ] Narrative techniques applied per technique-arc matrix? (YES/NO)
- [ ] Hook paragraph present (150-200 words)? (YES/NO)
- [ ] Title is arc-specific and compelling (not "Insight Summary")? (YES/NO)

⛔ If any answer is NO: STOP. Fix the narrative before proceeding to validation.

**Mark todo:** `Phase 4` → `[completed]`

---

### Phase 5: Validation

**Phase entry gate:**
> Verify Phase 4 completed: a complete narrative draft must exist with title, hook, 4 elements, and citations.
> If any structural component is missing: STOP. Return to Phase 4.

**Mark todo:** `Phase 5: Validation` → `[in_progress]`

All quality gates must pass. Check in priority order -- stop and fix before continuing if any gate fails.

**HARD STRUCTURAL GATE (check FIRST -- blocks all other gates):**

- [ ] EXACTLY 4 `##` headers in narrative body (below frontmatter)
- [ ] Headers match arc's exact element names from `references/language-templates.md` (language-specific)
- [ ] Headers in correct arc sequence
- [ ] No extra `##` headers outside the 4 arc elements

If this gate fails: Return to Phase 4. Do NOT rename sections to fix. REWRITE using the template.
The content was generated for the wrong structure and cannot be repaired by renaming.

**Critical gates (check after structural gate passes):**

- [ ] Total word count: 1,450-1,900
- [ ] Title is arc-specific (not generic "Insight Summary")
- [ ] Hook present (150-200 words)

**Evidence gates (narrative lacks credibility without these):**

- [ ] Citations: minimum 15, format `<sup>[N](file.md)</sup>`
- [ ] Every quantitative claim has a citation
- [ ] No fabricated references -- all cite loaded source files

**Structure gates:**

- [ ] Element word counts within targets (+/-50 words)
- [ ] Arc-specific techniques applied (check arc quality gates)
- [ ] Smooth transitions between elements
- [ ] Frontmatter contains all required fields

**Language gates (if `de`):**

- [ ] Proper umlauts throughout (ä, ö, ü, ß)
- [ ] ZERO ASCII fallbacks (ae, oe, ue, ss) in body text

**If any gate fails:** Report specific failures, fix, re-validate ALL gates (not just the failed one).

**Mark todo:** `Phase 5` → `[completed]`

---

### Phase 6: Write Output

**Phase entry gate:**
> Verify Phase 5 completed: all quality gates must have passed.
> If any critical or evidence gate still fails: STOP. Return to Phase 5.

**Mark todo:** `Phase 6: Write Output` → `[in_progress]`

1. Write narrative to output path (default: `insight-summary.md` in source directory)
2. Verify file created with correct word count
3. Return JSON summary (see Output section above)

**Phase completion:**

- [ ] File written to correct output path? (YES/NO)
- [ ] Word count verified after write? (YES/NO)
- [ ] JSON summary returned? (YES/NO)

**Mark todo:** `Phase 6` → `[completed]`

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
| `references/narrative-techniques/techniques-overview.md` | 8 narrative techniques with arc application matrix | Phase 3 |
| `references/phase-workflows/phase-4b-synthesis-{arc_id}.md` | Arc-specific transformation workflow | Phase 4 |
| `references/language-templates.md` | Localized headers for en/de | Phase 4 (if `de`) |
