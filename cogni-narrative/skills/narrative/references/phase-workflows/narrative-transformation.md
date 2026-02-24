# Narrative Transformation Workflow

## Overview

This document describes the general narrative transformation process. Arc-specific workflows in `phase-4b-synthesis-{arc_id}.md` provide detailed per-arc guidance.

## Transformation Pipeline

```text
Input .md Files → Content Analysis → Arc Pattern Loading → Element Construction → Assembly → Validation
```

## Step 1: Content Analysis

Read all input .md files. For each file:
1. Extract title from frontmatter or first `# heading`
2. Identify key sections (## headings)
3. Extract quantitative claims (numbers, percentages, currency amounts)
4. Build citation inventory: `{file_path, title, key_claims[]}`

## Step 2: Arc Pattern Loading

Load arc-specific references:
1. `arc-definition.md` -- element definitions, word targets, detection signals
2. 4 element pattern files -- transformation patterns per element
3. Map input content sections to arc elements using source content mapping from arc-definition

## Step 3: Element Construction

For each of the 4 arc elements:

### 3a: Content Mapping
- Identify which input sections map to this element (per arc-definition)
- Extract relevant claims, data points, and evidence
- Prioritize by citation quality and relevance

### 3b: Pattern Application
- Load the element's pattern file
- Select the most appropriate transformation pattern (e.g., Inverse Correlation Reframe, Hidden Variable Reframe)
- Apply the pattern structure to the mapped content

### 3c: Technique Application
- Apply arc-appropriate narrative techniques (per techniques-overview.md)
- Common: Number Plays on all quantitative data
- Element-specific: PSB for "problem" elements, IS-DOES-MEANS for "solution" elements, etc.

### 3d: Citation Embedding
- Embed citations in superscript format: `<sup>[N](source-file.md)</sup>`
- Sequential numbering across the full narrative
- Every quantitative claim must have a citation

### 3e: Word Count Calibration
- Check word count against element target (from arc-definition)
- Expand thin sections with additional evidence or technique application
- Trim verbose sections by consolidating or removing redundant evidence
- Tolerance: ±50 words per element

## Step 4: Narrative Assembly

1. **Title:** Generate arc-specific, compelling title (not generic)
2. **Hook:** 150-200 word opening using arc's hook construction pattern
3. **Elements:** Assemble 4 elements in arc order
4. **Transitions:** Insert arc-defined transitions between elements
5. **Closing:** Apply arc's closing pattern (final 1-2 sentences)

## Step 5: Frontmatter Generation

```yaml
---
title: "{Compelling Title}"
subtitle: "{Topic or Question}"
arc_id: "{arc-id}"
arc_display_name: "{Display Name}"
word_count: {actual}
language: "{en|de}"
date_created: "{ISO 8601}"
source_file_count: {N}
---
```

## Step 6: Validation

Run all quality gates from the arc-definition's Quality Gates section. Fix any failures before writing output.

## German Language Rules

When `language: "de"`:
- Body text MUST use proper umlauts (a, o, u, ss)
- Section headers from language-templates.md
- File names/slugs use ASCII transliterations (u→ue, a→ae)
- Shorter sentences than English equivalent
- More direct assertions, less hedging
- Gabor Steingart influence: punchy openings, data-driven assertions

## Evidence Grounding

- Every major claim traces to an input source file
- Quantitative data requires citation
- No fabricated statistics or evidence
- If input content lacks quantitative data for an element, use qualitative evidence with citations
- Graceful degradation: if input files are thin, note limitations rather than fabricating
