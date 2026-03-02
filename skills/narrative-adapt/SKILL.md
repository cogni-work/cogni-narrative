---
name: narrative-adapt
description: "Transform existing narratives into derivative formats: executive briefs, talking points, and one-pagers. This skill should be used when the user asks to 'adapt a narrative', 'create executive brief', 'generate talking points', 'make a one-pager', 'shorten narrative', 'condense narrative', 'summarize the narrative', 'narrative to bullets', 'convert narrative to brief', 'prepare briefing notes', or when other plugins need derivative formats from a full narrative."
---

# Narrative Adapt

## Purpose

Transform a full cogni-narrative output (1,450-1,900 word insight summary) into one of three derivative formats, preserving the arc structure and key evidence while adapting length, style, and layout for different communication contexts.

## When to Use

- Condense a full narrative into an executive brief for email or messaging
- Extract key messages as talking points for verbal briefings or presentations
- Create a structured one-pager for print or quick reference

**Not for:**
- Generating new narratives from source files (use `cogni-narrative:narrative`)
- Reviewing or scoring narratives (use `cogni-narrative:narrative-review`)
- Creating slide decks (use `cogni-visual:story-to-slides`)

---

## Parameters

| Parameter | Required | Description |
|-----------|----------|-------------|
| `--source-path` | Yes | Path to the narrative `.md` file to adapt |
| `--format` | Yes | Target format: `executive-brief`, `talking-points`, or `one-pager` |
| `--output` | No | Output file path; defaults to `{source-dir}/{format}.md` |
| `--language` | No | Override language (uses source frontmatter by default) |

---

## Output

A markdown file in the requested derivative format, plus a JSON summary.

### JSON Summary

```json
{
  "success": true,
  "source_path": "insight-summary.md",
  "output_path": "executive-brief.md",
  "format": "executive-brief",
  "arc_id": "corporate-visions",
  "word_count": 420,
  "language": "en"
}
```

---

## Execution Protocol

### Step 1: Load Source Narrative

1. Read the narrative file from `--source-path`
2. Extract YAML frontmatter: `title`, `subtitle`, `arc_id`, `arc_display_name`, `word_count`, `language`
3. Parse the narrative structure:
   - Hook paragraph (text between `#` title and first `##`)
   - 4 arc element sections (each `##` section with body text)
   - Citations (all `<sup>[N](file.md)</sup>` references)
4. If frontmatter is missing `arc_id`, attempt to detect from section headers
5. Determine language from: explicit parameter > frontmatter > default `en`

### Step 2: Extract Key Content

For each of the 4 arc element sections, extract:
- **Core claim:** The lead sentence or paragraph's central argument
- **Key evidence:** Up to 3 most impactful quantitative claims with their citations
- **Transition logic:** How this element connects to the next

Also extract from the hook:
- **Opening tension:** The central problem or insight that opens the narrative
- **Scope statement:** What the narrative covers

### Step 3: Transform to Target Format

Load and follow the format-specific template in [references/format-templates.md](references/format-templates.md).

| Format | Word Target | Key Feature |
|--------|-------------|-------------|
| Executive Brief | 300-500 | Condensed arc with citations, for email/Slack |
| Talking Points | N/A (bullets) | Answer-first bullets, no citations, verbal delivery |
| One-Pager | 400-600 | Metrics table + next steps, for print |

Each format preserves all 4 arc element headers in order. The reference file contains full template structures, word budgets, condensation strategies, and format-specific rules.

### Step 4: Validate Output

Check the generated derivative:
- Word count within format target range
- All 4 `##` headers present with correct arc element names
- Language matches source (including umlauts for `de`)
- No fabricated content beyond what the source narrative contains

### Step 5: Write Output

1. Write to output path (default: `{source-dir}/{format}.md`)
2. Verify file written correctly
3. Return JSON summary

---

## Language Rules

Derivative formats inherit the source narrative's language:
- **English (`en`):** Standard output
- **German (`de`):** All umlaut rules from the narrative skill apply. Proper Unicode umlauts (ä, ö, ü, ß) throughout. Zero ASCII fallbacks.

---

## Constraints

- DO NOT add information beyond what the source narrative contains
- DO NOT change the arc structure or reorder elements
- DO NOT fabricate statistics, claims, or evidence
- ALWAYS preserve the arc's rhetorical progression
- ALWAYS use the exact arc element header names from the source

---

## Bundled Resources

| File | Purpose | Load When |
|------|---------|-----------|
| `references/format-templates.md` | Detailed templates and examples for each format | Step 3 |
