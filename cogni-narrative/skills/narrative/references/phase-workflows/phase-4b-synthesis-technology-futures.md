# Phase 4b: Arc-Specific Insight Summary (technology-futures)

**Arc Framework:** What's Emerging -> Converging -> Possible -> Required

**Research Type:** All types | **Arc:** `technology-futures` (Tier 4)

**Objective:** Generate insight-summary.md using Technology Futures framework with journalistic storytelling patterns.

**Critical:** This phase generates `insight-summary.md` using the arc-specific framework.

**Inputs:** Dimension syntheses (Phase 3), arc entities (findings, sources, trends-watch+act, concepts), DIMENSION_REGISTRY, ENTITY_REGISTRIES

**Output:** `insight-summary.md` at project root (1,450-1,900 words)

---

## Step 4.1: Generate Insight Summary

**Objective:** Transform synthesis-cross-dimensional.md (400-600 words) into insight-summary.md (1,450-1,900 words) using technology-futures journalistic patterns.

**Input:** `12-synthesis/synthesis-cross-dimensional.md` (just created)
**Output:** `insight-summary.md` at project root

**Arc-Specific Patterns:** What's Emerging -> What's Converging -> What's Possible -> What's Required
**Word Count:** 1,450-1,900 words (3-4x expansion)

### Step 4.1.1: Load Evidence Entities (Context Tier 4)

**Tier 4 context:** This arc loads the RICHEST entity context of all arcs -- 4 entity types instead of the 2-3 used by lower tiers. Before loading, understand WHY each entity type serves this arc:

- **Findings** ground the narrative in verified research evidence. They answer "what did we discover?"
- **Sources** provide attribution credibility. They answer "who says so?"
- **Trends (watch + act)** supply the temporal dimension essential to technology-futures. They answer "what's moving and how fast?" Only load trends with `planning_horizon` of "watch" or "act" -- these are the actionable horizons for this arc. Exclude "monitor" horizon trends as they are too early-stage.
- **Concepts** provide the domain vocabulary and definitional precision that distinguishes expert analysis from superficial commentary. They answer "what exactly do we mean by X?"

**Think before loading:** Consider which dimensions from the synthesis are most prominent. This will help you prioritize entities that align with the narrative's strongest threads.

**For evidence grounding:**
- Load top 20 findings from `04-findings/data/` (quality_score >= 0.65)
- Load top 15 sources from `07-sources/data/` (reliability_score >= 0.8)
- Load dimension-scoped trends from `11-trends/data/` (planning_horizon = "watch" or "act")
- Load dimension-scoped concepts from `05-domain-concepts/data/`

**Implementation:** Use Glob + Read to load entity files, parse frontmatter.

**After loading, take stock:** Mentally catalog what you now have available. Which findings are strongest? Which trends have the most narrative potential? Which concepts need to be woven in as precise vocabulary? This inventory directly feeds the transformation in Step 4.1.4.

### Step 4.1.2: Count Entity Files for Stats Grid

**Count entity files** to populate `stats_*` frontmatter fields and the inline HTML stats grid:

```bash
# Count entities per type directory
stats_syntheses=$(ls 12-synthesis/data/*.md 2>/dev/null | wc -l | tr -d ' ')
stats_megatrends=$(ls 06-megatrends/data/*.md 2>/dev/null | wc -l | tr -d ' ')
stats_trends=$(ls 11-trends/data/*.md 2>/dev/null | wc -l | tr -d ' ')
stats_concepts=$(ls 05-domain-concepts/data/*.md 2>/dev/null | wc -l | tr -d ' ')
stats_findings=$(ls 04-findings/data/*.md 2>/dev/null | wc -l | tr -d ' ')
stats_claims=$(ls 10-claims/data/*.md 2>/dev/null | wc -l | tr -d ' ')
```

**Implementation:** Use Glob to count files in each entity `data/` directory. Store counts for use in frontmatter and stats grid HTML.

**Language-aware labels:** Check `project_language` from sprint-log.json. Use German labels (Dimensionen, Konzepte, Erkenntnisse, Aussagen) for `de`, English labels (Dimensions, Concepts, Findings, Claims) for `en`.

**Checkpoint:** Verify all 6 counts are integers (not empty strings). If any directory is missing or empty, the count should be 0, not omitted. These exact integers must appear both in the YAML frontmatter AND in the HTML stats grid -- any mismatch is a validation failure in Step 4.1.5.

### Step 4.1.3: OUTPUT TEMPLATE

**CRITICAL: Read this template BEFORE any extended thinking. This is the EXACT structure you must produce.**

**Rule: EXACTLY 4 `##` headers. No creative alternatives. No additional sections. No renaming.**

**Instruction: Fill in each section in order. Do NOT reorganize, rename, or add sections.**

**English headers:**
- `## What's Emerging: Technology Horizon`
- `## What's Converging: Integration Points`
- `## What's Possible: Application Scenarios`
- `## What's Required: Capability Development`

**German headers (if `language: de`):**
- `## Was Entsteht: Technologie-Horizont`
- `## Was Konvergiert: Integrationspunkte`
- `## Was Möglich Ist: Anwendungsszenarien`
- `## Was Erforderlich Ist: Kompetenzentwicklung`

**Structure:**
```markdown
---
title: "{Arc-Specific Compelling Title}"
subtitle: "{Research Question}"
arc_id: "technology-futures"
arc_display_name: "Technology Futures"
word_count: {1450-1900}
date_created: "{ISO 8601}"
stats_syntheses: {count from 12-synthesis/data/}
stats_megatrends: {count from 06-megatrends/data/}
stats_trends: {count from 11-trends/data/}
stats_concepts: {count from 05-domain-concepts/data/}
stats_findings: {count from 04-findings/data/}
stats_claims: {count from 10-claims/data/}
---

# {Arc-Specific Title}

*{Research Question subtitle}*

{Opening paragraph with narrative hook}

<div class="stats-grid" style="display:grid; grid-template-columns:repeat(3,1fr); gap:8px; margin:20px 0;">
  <div style="background:#f5f5f5; padding:14px 10px; border-radius:8px; text-align:center;">
    <div style="font-size:1.6em; font-weight:bold; color:#1a1a1a;">{stats_syntheses}</div>
    <div style="font-size:0.82em; color:#666;">{DE: Dimensionen | EN: Dimensions}</div>
  </div>
  <div style="background:#f5f5f5; padding:14px 10px; border-radius:8px; text-align:center;">
    <div style="font-size:1.6em; font-weight:bold; color:#1a1a1a;">{stats_megatrends}</div>
    <div style="font-size:0.82em; color:#666;">Megatrends</div>
  </div>
  <div style="background:#f5f5f5; padding:14px 10px; border-radius:8px; text-align:center;">
    <div style="font-size:1.6em; font-weight:bold; color:#1a1a1a;">{stats_trends}</div>
    <div style="font-size:0.82em; color:#666;">Trends</div>
  </div>
  <div style="background:#f5f5f5; padding:14px 10px; border-radius:8px; text-align:center;">
    <div style="font-size:1.6em; font-weight:bold; color:#1a1a1a;">{stats_concepts}</div>
    <div style="font-size:0.82em; color:#666;">{DE: Konzepte | EN: Concepts}</div>
  </div>
  <div style="background:#f5f5f5; padding:14px 10px; border-radius:8px; text-align:center;">
    <div style="font-size:1.6em; font-weight:bold; color:#1a1a1a;">{stats_findings}</div>
    <div style="font-size:0.82em; color:#666;">{DE: Erkenntnisse | EN: Findings}</div>
  </div>
  <div style="background:#f5f5f5; padding:14px 10px; border-radius:8px; text-align:center;">
    <div style="font-size:1.6em; font-weight:bold; color:#1a1a1a;">{stats_claims}</div>
    <div style="font-size:0.82em; color:#666;">{DE: Aussagen | EN: Claims}</div>
  </div>
</div>

---

## What's Emerging: Technology Horizon

{350-450 words with evidence grounding}

## What's Converging: Integration Points

{350-450 words with evidence grounding}

## What's Possible: Application Scenarios

{350-450 words with evidence grounding}

## What's Required: Capability Development

{350-450 words with evidence grounding}
```

### Step 4.1.4: Extended Thinking Sub-steps

This is the most cognitively demanding step. Follow this decomposed reasoning sequence exactly. Do not skip sub-steps or combine them -- each one produces an intermediate output that the next sub-step depends on.

---

#### Sub-step A: Absorb the Source Material

Read `synthesis-cross-dimensional.md` (400-600 words) in full. Before proceeding, answer these questions internally:

1. **What is the central thesis?** Identify the single most important claim or insight in the synthesis. This becomes your narrative spine.
2. **What are the 2-3 strongest evidence threads?** These are the claims backed by the most findings. They will receive the deepest expansion.
3. **What tensions or contrasts exist?** Technology-futures narratives thrive on tension between emergence and maturity, possibility and constraint. Identify these pairs.
4. **What is the research question?** This becomes the subtitle and should be echoed in the hook paragraph.

---

#### Sub-step B: Map Content to Arc Elements

Before writing anything, explicitly assign source material to each of the 4 arc elements. Think through this mapping carefully -- it determines the entire narrative structure.

For each element, decide:
- **What's Emerging: Technology Horizon** -- Which findings and trends describe NEW technologies, capabilities, or approaches that are appearing? Look for trends with `planning_horizon: "watch"` -- these are the most emergent. Look for concepts that define novel technical vocabulary.
- **What's Converging: Integration Points** -- Which findings show technologies or approaches COMBINING or reinforcing each other? Look for trends with `planning_horizon: "act"` -- these are mature enough to converge. Look for concepts that bridge multiple domains.
- **What's Possible: Application Scenarios** -- Which findings suggest CONCRETE applications or use cases? This element translates the abstract (emerging + converging) into the tangible. Concepts help here by providing precise terminology for application domains.
- **What's Required: Capability Development** -- Which findings identify GAPS, prerequisites, or capabilities that need building? This element provides the strategic call-to-action. Trends inform the urgency (watch vs. act horizon).

**Decision rule:** If a finding could fit multiple elements, assign it to the element where it provides the STRONGEST evidence. Each finding should appear in exactly one element (though the same source may be cited across multiple elements).

**Wikilink distribution plan:** You need 40-50 total entity wikilinks. Plan the distribution now:
- Target ~10-13 wikilinks per element (to stay balanced)
- Mix entity types within each element: aim for at least 2 finding wikilinks, 1-2 source wikilinks, 1-2 trend wikilinks, and 1-2 concept wikilinks per element
- Tier 4 distinction: unlike lower tiers, you MUST include both trend AND concept wikilinks -- these are what make the technology-futures narrative analytically rich

---

#### Sub-step C: Generate the Title

Think about what makes this research DISTINCTIVE before choosing a title:
- What would make a technology executive stop scrolling?
- Does the title capture the core tension or opportunity identified in the synthesis?
- Is it specific to THIS research, or could it apply to any technology report?

**Constraints:** The title must NOT be generic (e.g., "Insight Summary", "Technology Futures Analysis"). It MUST reflect the specific subject matter and arc lens. Good titles often use a colon pattern: `{Specific Subject}: {Arc-Informed Angle}`.

---

#### Sub-step D: Write the Hook Paragraph

The opening paragraph (150-200 words) must accomplish three things simultaneously:
1. **Create narrative tension** -- open with a concrete, surprising, or provocative observation from the findings. Do not open with a generic statement about technology change.
2. **Establish the research question** -- the subtitle (research question) should be echoed or paraphrased naturally within the hook.
3. **Signal the arc trajectory** -- hint at the Emerging -> Converging -> Possible -> Required progression without naming it explicitly. The reader should feel the narrative momentum.

**Think before writing:** What is the single most compelling data point, trend, or contrast from your loaded entities? Lead with that.

---

#### Sub-step E: Expand Each Arc Element (350-450 words each)

For EACH of the 4 elements, follow this micro-sequence:

1. **Lead with the element's core assertion** -- one sentence that captures the main point of this element. This is the "answer first" (Pyramid Principle).
2. **Ground it immediately** -- within the first 2-3 sentences, cite a finding with a wikilink. Do not let more than 50 words pass without evidence.
3. **Develop with evidence layering** -- weave in additional findings, trends, and concepts. Each new evidence point should BUILD on the previous one, not merely list alongside it. Use transitions like "this convergence accelerates because...", "the emerging pattern becomes clearer when...", "what makes this possible is..."
4. **Integrate concepts as precision vocabulary** -- when you reference a domain concept, use its wikilink and briefly activate its meaning in context. Do not just drop concept names -- show why the precise term matters.
5. **Weave in trends for temporal grounding** -- trends with "watch" horizon belong primarily in What's Emerging; trends with "act" horizon belong primarily in What's Converging and What's Required. When citing a trend, connect it to the TIME dimension: how fast, how soon, how urgent.
6. **Close the element with a forward-facing sentence** -- each element should end by creating momentum toward the NEXT element. What's Emerging should end by hinting at convergence. What's Converging should end by hinting at possibility. What's Possible should end by hinting at requirements.

**Word count discipline:** Each element targets 350-450 words. After drafting each element, check your count. If under 350, you likely need another evidence-grounded paragraph. If over 450, tighten by removing redundant transitions or combining similar points.

**Journalistic techniques to apply:**
- Use specific numbers and metrics from findings (Number Plays technique)
- Create contrast structures: "While X is emerging, Y remains..." (Contrast Structure technique)
- Address the reader where appropriate: "Organizations that..." (You-Phrasing technique)
- Stack implications: "This means not only X, but also Y, and ultimately Z" (Compound Impact technique)

---

#### Sub-step F: Assemble the Stats Grid

Insert the HTML stats grid between the opening paragraph and the first `---` separator. Use the exact counts from Step 4.1.2. Double-check:
- Each `{stats_*}` placeholder is replaced with the actual integer from Step 4.1.2
- Labels match the project language (DE or EN)
- The 6 values in the HTML grid match the 6 values in the YAML frontmatter exactly

---

#### Sub-step G: Assemble and Self-Review

**CRITICAL LANGUAGE CHECK:** If `language: de`, ALL generated text MUST use proper Unicode umlauts (ä, ö, ü, Ä, Ö, Ü, ß). Writing "fuer" instead of "für", "Aenderung" instead of "Änderung", or "Uebergangsfrist" instead of "Übergangsfrist" is a BLOCKING DEFECT. ASCII transliterations (ae, oe, ue) are ONLY for file names and slugs, NEVER for body text, titles, or headings.

Before finalizing, review the complete draft against these quality checks:

1. **Narrative flow:** Read the transition from each element to the next. Does What's Emerging flow naturally into What's Converging? Does What's Possible set up What's Required? If any transition feels abrupt, add a bridging sentence at the element boundary.
2. **Evidence balance:** Count wikilinks per element. If any element has fewer than 8 wikilinks, add more entity references. If the total is under 40 or over 50, adjust.
3. **Entity type diversity:** Verify that ALL 4 entity types (findings, sources, trends, concepts) appear across the narrative. Tier 4's value comes from this diversity -- a technology-futures narrative that only cites findings and sources is operating at Tier 1 quality.
4. **Word count:** Verify total is 1,450-1,900. Verify each element is 350-450.
5. **Title check:** Re-read the title. Is it specific and compelling? Would it work as a headline?

---

**Now fill in the template from Step 4.1.3. Write each section in sequence. Do NOT deviate from the template structure.**

### Step 4.1.5: Validate Output

**Before checking the gates, think about common failure modes for this arc:**
- Technology-futures narratives often run LONG because of the rich Tier 4 context. If over 1,900 words, tighten the weakest element first (the one with the fewest strong findings).
- Wikilink counts often fall short in the "What's Required" element because it tends toward prescriptive language rather than evidence-grounded claims. If under target, add concept and trend wikilinks to capability statements.
- The arc progression (Emerging -> Converging -> Possible -> Required) must feel like a JOURNEY, not 4 independent essays. If validation reveals abrupt transitions, revisit the closing sentences of each element (see Sub-step E, point 6).

**HARD STRUCTURAL GATE (check FIRST -- blocks all other gates):**

Count the `##` headers in the narrative body (below frontmatter):
- [ ] EXACTLY 4 `##` headers (not more, not fewer)
- [ ] Headers match the arc's exact element names from the template in Step 4.1.3
- [ ] Headers appear in the correct arc sequence (What's Emerging → What's Converging → What's Possible → What's Required)
- [ ] No `##` headers exist outside the 4 arc elements

If this gate fails: STOP. Do NOT rename sections to fix. REWRITE using the template from Step 4.1.3.
The content was generated for the wrong structure and cannot be repaired by renaming.

**Quality gates:**
- [ ] Word count: 1,450-1,900 words
- [ ] All 4 arc elements present with arc-specific headers
- [ ] Evidence grounding: >= 8 finding citations
- [ ] Entity wikilinks: 40-50 total
- [ ] Title: Arc-specific (not "Insight Summary")
- [ ] Arc framework consistent with synthesis-cross-dimensional.md
- [ ] Frontmatter contains all 6 `stats_*` fields with integer values
- [ ] Inline HTML stats grid present between opening paragraph and first `---`
- [ ] Stats grid values match `stats_*` frontmatter fields
- [ ] Stats grid labels match project language (DE/EN)
- [ ] **German umlaut check (if `de`):** Body text contains ZERO instances of ASCII fallbacks (fuer, ueber, Aenderung, Uebersicht, etc.) -- all must use ä, ö, ü, ß

**If any gate fails:** Identify which sub-step (A-G) produced the defect, return to that sub-step's reasoning, and fix the specific issue. Do not regenerate the entire document -- make targeted repairs.

### Step 4.1.6: Write Output

**CRITICAL:** Write to `insight-summary.md` at project root (NOT in 12-synthesis/).

**Before writing, perform a final sanity check:**
1. Confirm the file content starts with `---` (YAML frontmatter opening)
2. Confirm `arc_id` in frontmatter is `"technology-futures"` (not another arc)
3. Confirm `word_count` in frontmatter matches the actual word count of the body text
4. Confirm the file path does NOT start with a dot (`.insight-summary.md` is wrong)

**Use Write tool with explicit instruction:**
- Call Write tool
- file_path: `insight-summary.md` (relative to project root)
- content: Complete insight summary with frontmatter (from Step 4.1.4)

**Verification:**
```bash
# Verify file created without dot prefix
if [[ ! -f "insight-summary.md" ]]; then
  echo "ERROR: insight-summary.md not created"
  exit 1
fi

word_count=$(wc -w < insight-summary.md | tr -d ' ')
echo "insight-summary.md created (${word_count} words) at project root"
```

**IMPORTANT:** File must be created as `insight-summary.md` (NO dot prefix, NO subdirectory).

---

## Next Phase

**Phase 5:** Validation verifies all wikilinks reference loaded entities.

---

**End of Phase 4 Technology Futures Arc Workflow**
