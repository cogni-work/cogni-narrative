# Phase 4b: Arc-Specific Insight Summary (industry-transformation)

**Arc Framework:** Forces -> Friction -> Evolution -> Leadership

**Research Type:** All types | **Arc:** `industry-transformation` (Tier 3)

**Objective:** Generate insight-summary.md using Industry Transformation framework with journalistic storytelling patterns.

**Critical:** This phase generates `insight-summary.md` using the arc-specific framework.

**Inputs:** Dimension syntheses (Phase 3), arc entities (findings, sources, trends-all, megatrends), DIMENSION_REGISTRY, ENTITY_REGISTRIES

**Output:** `insight-summary.md` at project root (1,450-1,900 words)

---

## Step 4.1: Generate Insight Summary

**Objective:** Transform synthesis-cross-dimensional.md (400-600 words) into insight-summary.md (1,450-1,900 words) using industry-transformation journalistic patterns.

**Input:** `12-synthesis/synthesis-cross-dimensional.md` (just created)
**Output:** `insight-summary.md` at project root

**Arc-Specific Patterns:** Forces -> Friction -> Evolution -> Leadership
**Word Count:** 1,450-1,900 words (3-4x expansion)

### Step 4.1.1: Load Evidence Entities (Context Tier 3)

**Reasoning checkpoint before loading:** Tier 3 (Industry Transformation) uses ALL planning horizons for trends plus megatrends. This is broader than Tier 1-2 arcs. Before loading, think through what each entity type contributes to the arc framework:
- **Findings** ground claims in evidence -- these become inline citations (`<sup>[N]</sup>`)
- **Sources** establish credibility -- these validate the reliability of cited evidence
- **Trends (all horizons)** feed the Forces and Friction elements -- short-horizon trends reveal immediate friction, long-horizon trends reveal structural forces
- **Megatrends** are primary inputs for Forces -- they define the macro-level drivers reshaping the industry

**For evidence grounding:**
- Load top 20 findings from `04-findings/data/` (quality_score >= 0.65)
- Load top 15 sources from `07-sources/data/` (reliability_score >= 0.8)
- Load dimension-scoped trends from `11-trends/data/` (all planning horizons)

**Implementation:** Use Glob + Read to load entity files, parse frontmatter.

**After loading, mentally categorize each entity:** For each loaded finding, trend, and megatrend, determine which arc element(s) it will serve:
- Does it describe a macro driver or external pressure? -> Forces
- Does it describe a barrier, resistance, or adoption challenge? -> Friction
- Does it describe structural change, new models, or future state? -> Evolution
- Does it describe positioning, strategy, or competitive advantage? -> Leadership
- Some entities will serve multiple elements -- note these as cross-element connectors

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

**Language-aware labels:** Check `project_language` from sprint-log.json. Use German labels (Dimensionssynthesen, Konzepte, Erkenntnisse, Aussagen) for `de`, English labels (Dimension Syntheses, Concepts, Findings, Claims) for `en`.

### Step 4.1.3: Extended Thinking Transformation

**This is the most cognitively demanding step.** Before generating any output, complete the following reasoning sequence in extended thinking. Do NOT skip any sub-step -- each one builds the mental model needed for high-quality output.

#### Sub-step A: Absorb the Source Material

1. Read `synthesis-cross-dimensional.md` (400-600 words) in full
2. **Think:** What is the central thesis of this synthesis? State it in one sentence
3. **Think:** What are the 3-5 most important claims or patterns in the synthesis?
4. **Think:** What evidence from the loaded entities (Step 4.1.1) supports or challenges these claims?

#### Sub-step B: Map Content to Arc Elements

**Before writing anything, create a mental mapping of source content to the 4 arc elements.** For each element, identify:

**Forces (Transformation Drivers):**
- **Think:** What macro-level forces (regulatory, technological, social, economic) does the evidence reveal?
- **Think:** Which megatrends define the scope and magnitude of these forces? Identify 2-3 megatrends by name
- **Think:** Which trends across planning horizons show force timing? (short-horizon = imminent force, long-horizon = structural force)
- **Think:** How do these forces interact -- do they reinforce each other (convergent) or create tensions (divergent)?
- **Select:** 2-3 findings for citation, 1-2 megatrend wikilinks, 2-3 trend wikilinks

**Friction (Barriers to Change):**
- **Think:** What barriers or resistance points emerge from the evidence? Consider: incumbents, regulations, infrastructure gaps, cultural inertia, capital requirements, timing mismatches
- **Think:** For each friction point, is it temporary (will dissolve) or structural (will persist)?
- **Think:** What workarounds or navigation strategies does the evidence suggest?
- **Think:** How does friction interact with the forces identified above -- does it slow, distort, or redirect them?
- **Select:** 2-3 findings for citation, 2-3 trend wikilinks showing adoption barriers

**Evolution (Pathway Forward):**
- **Think:** What new industry structure emerges once forces overcome friction? Describe the future state, not the transition
- **Think:** Who gains power and who loses power in the transformed industry?
- **Think:** How does value creation change -- what new business models or competitive dynamics emerge?
- **Think:** What is the realistic timeline to the new equilibrium?
- **Select:** 2-3 findings for citation, 2-3 trend/concept wikilinks

**Leadership (Strategic Imperatives):**
- **Think:** Given the evolved industry structure, what positioning creates advantage?
- **Think:** What differentiates leaders from survivors in the new equilibrium?
- **Think:** What timing decisions are critical -- when must resources be committed?
- **Think:** What transition strategy connects current state to future positioning?
- **Select:** 1-2 findings for citation, 2-3 wikilinks to actionable entities

#### Sub-step C: Design the Narrative Arc

**Think through narrative flow before writing:**

1. **Title:** Generate 3 candidate titles. Each must be specific to this industry/topic (never generic like "Insight Summary" or "Industry Transformation Analysis"). Choose the most compelling one -- it should create curiosity or tension
2. **Hook paragraph:** What is the single most surprising or provocative data point from the evidence? This becomes the opening sentence. The hook should make the reader think "I need to understand this." Plan the hook as: [Surprising fact] -> [Why it matters] -> [What this summary will reveal]
3. **Element flow:** Verify the narrative logic of Forces -> Friction -> Evolution -> Leadership:
   - Forces establishes *what is driving change* (the "why now")
   - Friction establishes *what resists change* (the tension)
   - Evolution resolves the tension by showing *what emerges* (the new state)
   - Leadership converts understanding into *action* (the "so what")
   - Each transition should feel inevitable, not abrupt

#### Sub-step D: Plan Evidence Distribution

**Think about wikilink and citation distribution before writing:**

- **Target:** 40-50 total entity wikilinks across all 4 elements
- **Minimum:** 8 finding citations (aim for 10-12)
- **Distribution rule:** No element should have fewer than 8 wikilinks or more than 15
- **Variety rule:** Each element should reference at least 2 different entity types (findings + trends, or megatrends + concepts, etc.)
- **Wikilink format:** `[[entity-filename]]` (without path or .md extension)
- **Citation format:** Reference findings and sources as evidence anchors

**Think:** Review the entities selected in Sub-step B. Do they total 40-50 wikilinks? If not, identify additional entities from the loaded evidence to fill gaps. Prioritize entities that serve as connectors between arc elements.

#### Sub-step E: Generate the Output

**CRITICAL LANGUAGE CHECK:** If `language: de`, ALL generated text MUST use proper Unicode umlauts (ä, ö, ü, Ä, Ö, Ü, ß). Writing "fuer" instead of "für", "Aenderung" instead of "Änderung", or "Uebergangsfrist" instead of "Übergangsfrist" is a BLOCKING DEFECT. ASCII transliterations (ae, oe, ue) are ONLY for file names and slugs, NEVER for body text, titles, or headings.

**Now write the complete insight-summary.md.** Follow this exact sequence:

1. Write frontmatter (use template below, fill all `stats_*` fields from Step 4.1.2)
2. Write the H1 title (from Sub-step C)
3. Write the subtitle (research question, italicized)
4. Write the hook paragraph (150-200 words, from Sub-step C)
5. Insert the stats grid HTML (use template below, fill counts from Step 4.1.2)
6. Insert horizontal rule (`---`)
7. Write **Forces: Transformation Drivers** (350-450 words)
   - Open with the dominant macro force -- frame it as structural and irreversible
   - Quantify force magnitude using megatrend scope and trend data
   - Show force interactions (how 2-3 forces converge or conflict)
   - Ground every major claim in a finding citation
   - Weave in 8-12 wikilinks to megatrends, trends, and findings
8. Write **Friction: Barriers to Change** (350-450 words)
   - Open with the most critical friction point -- the one that most directly opposes the dominant force
   - Distinguish temporary friction (will dissolve) from structural friction (will persist)
   - Show timing mismatches between forces and industry readiness
   - Describe workarounds or navigation strategies
   - Weave in 8-12 wikilinks
9. Write **Evolution: Pathway Forward** (350-450 words)
   - Open with a description of the new equilibrium -- what the industry looks like after transformation
   - Describe power shifts (who gains, who loses)
   - Explain new business models or value creation patterns
   - Provide a timeline to the new equilibrium
   - Weave in 8-12 wikilinks
10. Write **Leadership: Strategic Imperatives** (350-450 words)
    - Open with the core positioning question for industry participants
    - Specify differentiation sources in the new equilibrium
    - Define timing strategy (when to commit, what to sequence)
    - Close with a forward-looking statement about positioning for the new structure (not defending the old)
    - Weave in 8-12 wikilinks

**While writing each element, continuously verify:**
- Am I grounding claims in evidence (not making unsupported assertions)?
- Am I using journalistic storytelling (not academic report style)?
- Am I maintaining the transformation narrative (forces -> friction -> evolution -> leadership)?
- Am I distributing wikilinks across different entity types?

**Structure:**
```markdown
---
title: "{Arc-Specific Compelling Title}"
subtitle: "{Research Question}"
arc_id: "industry-transformation"
arc_display_name: "Industry Transformation"
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
    <div style="font-size:0.82em; color:#666;">{DE: Dimensionssynthesen | EN: Dimension Syntheses}</div>
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

## Forces: Transformation Drivers

{350-450 words with evidence grounding}

## Friction: Barriers to Change

{350-450 words with evidence grounding}

## Evolution: Pathway Forward

{350-450 words with evidence grounding}

## Leadership: Strategic Imperatives

{350-450 words with evidence grounding}
```

### Step 4.1.4: Validate Output

**Before declaring this step complete, systematically check every gate.** Do not batch-assess -- check each gate individually and note pass/fail.

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

**If any gate fails:** Identify the specific failure, determine the minimum edit to fix it, apply the fix, then re-check all gates. Do not proceed to Step 4.1.5 until all gates pass.

**Common failure modes to watch for:**
- Word count under 1,450: Usually caused by thin Evolution or Leadership sections -- expand with additional evidence and analysis
- Wikilinks under 40: Usually caused by concentrating links in Forces -- redistribute across all 4 elements
- Finding citations under 8: Review loaded findings and add citations to unsupported claims
- Stats grid mismatch: Re-verify counts from Step 4.1.2 against both frontmatter and HTML values

### Step 4.1.5: Write Output

**CRITICAL:** Write to `insight-summary.md` at project root (NOT in 12-synthesis/).

**Use Write tool with explicit instruction:**
- Call Write tool
- file_path: `insight-summary.md` (relative to project root)
- content: Complete insight summary with frontmatter (from Step 4.1.3)

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

**End of Phase 4 Industry Transformation Arc Workflow**
