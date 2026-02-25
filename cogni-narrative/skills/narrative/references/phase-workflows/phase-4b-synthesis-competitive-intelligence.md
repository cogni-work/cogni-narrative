# Phase 4b: Arc-Specific Insight Summary (competitive-intelligence)

**Arc Framework:** Landscape -> Shifts -> Positioning -> Implications

**Research Type:** All types | **Arc:** `competitive-intelligence` (Tier 1)

**Objective:** Generate insight-summary.md using Competitive Intelligence framework with journalistic storytelling patterns.

**Critical:** This phase generates `insight-summary.md` using the arc-specific framework.

**Inputs:** Dimension syntheses (Phase 3), arc entities (findings, sources), DIMENSION_REGISTRY, ENTITY_REGISTRIES

**Output:** `insight-summary.md` at project root (1,450-1,900 words)

---

## Step 4.1: Generate Insight Summary

**Objective:** Transform synthesis-cross-dimensional.md (400-600 words) into insight-summary.md (1,450-1,900 words) using competitive-intelligence journalistic patterns.

**Input:** `12-synthesis/synthesis-cross-dimensional.md` (just created)
**Output:** `insight-summary.md` at project root

**Arc-Specific Patterns:** Landscape -> Shifts -> Positioning -> Implications
**Word Count:** 1,450-1,900 words (3-4x expansion)

### Step 4.1.1: Load Evidence Entities (Context Tier 1)

**Tier 1 context note:** This arc uses minimal entity types (findings + sources only). No trends, megatrends, or concepts are loaded. This means the transformation must extract maximum competitive insight from fewer entity types. Think about this constraint before proceeding: every finding and source must work harder to provide evidence grounding across all 4 arc elements.

**For evidence grounding:**
- Load top 20 findings from `04-findings/data/` (quality_score >= 0.65)
- Load top 15 sources from `07-sources/data/` (reliability_score >= 0.8)

**Before loading, reason through the following:**
1. Identify which entity directories exist using Glob. If `04-findings/data/` or `07-sources/data/` contain fewer files than the requested counts, load all available files instead of failing.
2. For each loaded finding, extract: `title`, `quality_score`, `dimension_id`, and the body content. These will be used for evidence grounding and wikilink generation.
3. For each loaded source, extract: `title`, `reliability_score`, `source_type`, and any key claims. These provide citation authority.

**Implementation:** Use Glob + Read to load entity files, parse frontmatter. Sort findings by quality_score descending, sources by reliability_score descending. Retain the sorted lists in working memory -- you will reference them repeatedly during Step 4.1.3.

**After loading, mentally inventory what you have:**
- How many findings loaded? Which dimensions do they cover?
- How many sources loaded? What source types (academic, industry, news)?
- Are there any gaps -- dimensions with no findings coverage? Flag these now; you will need to handle them during the transformation by relying more heavily on synthesis content for those dimensions.

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

**Reasoning checkpoint:** Even though this is Tier 1 (only findings + sources loaded for evidence), you still count ALL entity types for the stats grid. The stats grid shows the full research corpus, not just what was loaded for grounding. Verify that each count is an integer (0 is valid). Record the exact values now -- you will insert them into both the YAML frontmatter AND the HTML stats grid, and these must match exactly.

**Language-aware labels:** Check `project_language` from sprint-log.json. Use German labels (Dimensionssynthesen, Konzepte, Erkenntnisse, Aussagen) for `de`, English labels (Dimension Syntheses, Concepts, Findings, Claims) for `en`.

### Step 4.1.3: Extended Thinking Transformation

**This is the most cognitively demanding step.** You are expanding a 400-600 word cross-dimensional synthesis into a 1,450-1,900 word competitive intelligence narrative. Follow the sub-steps below in exact order. Do not skip ahead. Complete each reasoning task before moving to the next.

---

#### Sub-step A: Read and Decompose the Source

1. Read `12-synthesis/synthesis-cross-dimensional.md` in full.
2. **Before writing anything, analyze the source by answering these questions:**
   - What is the core competitive insight? State it in one sentence.
   - What market structure is described or implied (fragmented, concentrated, oligopoly, duopoly)?
   - Which competitors or competitive forces are mentioned explicitly?
   - What shifts or changes in competitive dynamics are identified?
   - What strategic recommendations or positioning opportunities emerge?
   - What is the most surprising or counterintuitive finding? (This will seed your hook.)
3. **Map source content to arc elements.** For each of the 4 elements, identify which sentences or paragraphs from the synthesis provide raw material:
   - **Landscape** sources: market structure, current positions, competitive bases, established moats
   - **Shifts** sources: momentum indicators, strategic moves, capability races, emerging/declining threats
   - **Positioning** sources: uncontested spaces, capability gaps, timing advantages, differentiation axes
   - **Implications** sources: action items, urgency indicators, implementation timelines, response scenarios
4. **Identify content gaps.** If the synthesis provides strong material for Landscape and Shifts but thin material for Positioning and Implications, note this now. You will need to reason more deeply from evidence entities to fill those gaps during element drafting.

---

#### Sub-step B: Plan the Evidence Distribution

**Think about this before drafting any prose:** You have findings and sources as your only entity types (Tier 1). You need >= 8 finding citations and 40-50 total wikilinks across the entire narrative. Plan the distribution now.

1. **Assign findings to arc elements.** Review each loaded finding's `dimension_id` and content. Decide which element each finding best supports:
   - Findings about market structure, current state, competitive positions --> Landscape
   - Findings about changes, trends, momentum, strategic moves --> Shifts
   - Findings about gaps, opportunities, differentiation --> Positioning
   - Findings about actions, timelines, urgency, responses --> Implications
2. **Target at least 2 finding citations per element** (8+ total). If findings cluster heavily in one element, look for secondary relevance -- a finding about market structure may also imply a shift when contrasted with historical data.
3. **Plan wikilink distribution.** Target 10-13 wikilinks per element section (40-50 total). Wikilinks reference entity files: `[[finding-slug]]`, `[[source-slug]]`. Every wikilink must correspond to an actually loaded entity. Do not fabricate wikilinks to entities that do not exist.
4. **Identify your strongest evidence.** Which 3-5 findings contain quantified data (percentages, dollar amounts, growth rates)? Mark these as priority citations -- they will power your Number Plays and Comparative Anchoring techniques.

---

#### Sub-step C: Craft the Title and Hook

1. **Generate the title.** It must be arc-specific and compelling -- not generic like "Insight Summary" or "Competitive Intelligence Report." Think about:
   - What is the single most important competitive insight from the synthesis?
   - Can you frame it as a tension or paradox? (e.g., "The Fragmentation Paradox: How Market Leaders Are Losing by Winning")
   - Does it signal the arc's Landscape -> Implications progression?
   - Is it specific to this research domain, not interchangeable with any other report?
2. **Draft the hook paragraph (150-200 words).** Follow the competitive-intelligence hook pattern:
   - Open with a surprising competitive shift or market structure change
   - Challenge conventional wisdom about the competitive landscape
   - Pattern: "[Established player/assumption] [surprising data point] signals [structural shift]. [Conventional wisdom] no longer predicts [competitive outcome]."
   - Ground with at least 1 citation from your strongest finding
   - End the hook by previewing the 4-element arc progression without explicitly naming them

---

#### Sub-step D: Draft Each Arc Element

**For each element, follow this micro-reasoning sequence before writing prose:**

**D1. Landscape: Competitive Overview (350-450 words)**

*Think first:*
- What is the market structure? (fragmented, concentrated, oligopoly) State it.
- Who are the current leaders and what are their positions? Quantify where possible.
- What competitive bases operate? (cost leadership, differentiation, focus/niche)
- What moats or barriers exist?
- Which findings provide quantified evidence for these claims?

*Then write:*
- Open with the competitive structure overview (use Pyramid Principle: answer first)
- Decompose aggregate numbers into segment-level detail (key Landscape technique: move from top-line to segment-level divergence)
- Identify competitive clusters and explain the competitive logic of each
- Apply Number Plays: use Specific Quantification and Comparative Anchoring for market share or capability data
- Ground every quantitative claim with a finding citation
- Place 10-13 wikilinks naturally throughout
- Target: 350-450 words

*Transition to Shifts:* Use pattern -- "Three [momentum shifts / strategic moves / competitive dynamics] are reshaping this landscape."

**D2. Shifts: Market Dynamics (350-450 words)**

*Think first:*
- What momentum indicators exist? (market share trends, investment patterns, growth rates)
- What strategic moves are underway? (M&A, partnerships, pivots, capability building)
- Which threats are emerging and which are declining?
- How does the synthesis describe change over time?
- Which findings quantify rate-of-change rather than static state?

*Then write:*
- Open by contrasting static top-line metrics with dynamic segment trajectories (key Shifts technique: velocity analysis)
- Show momentum attribution: where is growth actually coming from?
- Project future trajectory using evidence data (use Compound Impact technique where data supports it)
- Identify the competitive logic shift (e.g., breadth -> specialization, cost -> value, scale -> agility)
- Apply Forcing Functions technique: what external pressures accelerate these shifts?
- Ground with finding citations, especially those showing temporal data
- Place 10-13 wikilinks naturally throughout
- Target: 350-450 words

*Transition to Positioning:* Use pattern -- "These shifts create strategic gaps in [specific areas]."

**D3. Positioning: Strategic Options (350-450 words)**

*Think first:*
- What uncontested spaces exist? Where are competitors NOT playing?
- What capability gaps exist? What do competitors lack?
- What timing advantages exist? What windows are open before competitors move?
- What differentiation axes are available? How can one compete differently rather than harder?
- Which findings support these gaps, and which gaps must you infer from the absence of competitive activity?

*Then write:*
- Open by connecting shifts to the gaps they create (don't repeat Shifts content -- build on it)
- Apply IS-DOES-MEANS pattern for each strategic positioning option:
  - IS: What the positioning option concretely is
  - DOES: What quantified advantage it provides (use You-Phrasing: "Your organization gains...")
  - MEANS: Why competitors will struggle to replicate it
- Use Contrast Structure: "Most organizations pursue X. The evidence suggests Y creates advantage."
- Apply PSB (Problem-Solution-Benefit) for at least one unconsidered positioning opportunity
- Ground with finding citations, especially those revealing gaps or white spaces
- Place 10-13 wikilinks naturally throughout
- Target: 350-450 words

*Transition to Implications:* Use pattern -- "Capturing these [gaps / positions] requires time-bound action."

**D4. Implications: Action Priorities (350-450 words)**

*Think first:*
- What immediate actions (0-6 months) does the evidence support?
- What near-term moves (6-18 months) build on those actions?
- What capability building (18-36 months) is required for sustained positioning?
- What competitive response scenarios should be anticipated?
- Which findings provide urgency data (closing windows, accelerating competition)?

*Then write:*
- Structure around time horizons: immediate, near-term, capability-building
- Apply Forcing Functions technique: link each action to the external pressure that makes timing critical
- Use Compound Impact to show cost of inaction (what happens if these actions are delayed?)
- Include competitive response scenarios: "If [competitor action], then [required response]"
- Close with the competitive window pattern: "Strategic gaps close by [timeframe]. Organizations moving by [date] capture positions. Delay means competing for crowded spaces."
- Ground with finding citations that provide timeline or urgency data
- Place 10-13 wikilinks naturally throughout
- Target: 350-450 words

---

#### Sub-step E: Assemble and Self-Review

Before finalizing, review the complete draft against these checkpoints:

1. **Word count check:** Count words across all sections. Total must be 1,450-1,900. If under, identify which elements are thin and add evidence-grounded depth. If over, trim redundant phrasing (not evidence).
2. **Arc coherence check:** Read the element headers in sequence: Landscape -> Shifts -> Positioning -> Implications. Does each element build on the previous? Does the narrative create a logical progression from "what is" to "what's changing" to "where to play" to "what to do"?
3. **Evidence grounding check:** Count finding citations. Must be >= 8 total. Are they distributed across elements (at least 2 per element)?
4. **Wikilink count:** Count total `[[entity-slug]]` references. Must be 40-50. Are they distributed roughly evenly (10-13 per element)?
5. **Wikilink validity:** Every `[[slug]]` must reference an entity file that was actually loaded in Step 4.1.1. Cross-check against your loaded entity list. Remove any fabricated wikilinks.
6. **Technique verification:** Confirm you applied: Number Plays (at least 3 instances), Contrast Structure (at least 2 instances), Forcing Functions (at least 1 in Shifts or Implications), and You-Phrasing (at least 2 instances in Positioning or Implications).
7. **Tier 1 quality check:** Since you had only findings + sources (no trends, concepts, or megatrends), verify that you maximized insight density from these limited entities. Each finding should be cited or wikilinked at least once. If any loaded findings went unused, consider where they could strengthen thin sections.

---

**Use extended thinking to:**
1. Read synthesis-cross-dimensional.md (400-600 words)
2. Apply competitive-intelligence patterns from loaded references
3. Expand each of 4 arc elements with:
   - Journalistic storytelling techniques
   - Evidence grounding (findings + sources)
   - Entity wikilinks (40-50 total)
4. Generate arc-specific title (compelling, not generic)
5. Expand to 1,450-1,900 words

**Structure:**
```markdown
---
title: "{Arc-Specific Compelling Title}"
subtitle: "{Research Question}"
arc_id: "competitive-intelligence"
arc_display_name: "Competitive Intelligence"
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

## Landscape: Competitive Overview

{350-450 words with evidence grounding}

## Shifts: Market Dynamics

{350-450 words with evidence grounding}

## Positioning: Strategic Options

{350-450 words with evidence grounding}

## Implications: Action Priorities

{350-450 words with evidence grounding}
```

### Step 4.1.4: Validate Output

**Before checking gates, re-read the complete generated output from start to finish.** Do not validate from memory -- validate from the actual text.

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

**If any gate fails, fix it now before proceeding to Step 4.1.5.** Common failure modes and fixes:
- **Word count too low:** Identify the thinnest arc element and add evidence-grounded depth. Do not pad with generic filler -- add another finding citation with analysis, or apply an unused narrative technique.
- **Word count too high:** Trim redundant transition phrases or duplicated points across elements. Preserve all citations and wikilinks during trimming.
- **Wikilinks below 40:** Check which loaded entities have no wikilinks yet. Add references where they naturally support claims.
- **Wikilinks above 50:** Remove wikilinks that feel forced or are clustered too densely. Aim for natural distribution.
- **Finding citations below 8:** Review your loaded findings list. Find findings you haven't cited and identify where in the narrative they provide supporting evidence.
- **Stats grid values mismatch frontmatter:** Copy the exact integer values from the frontmatter into the HTML grid. They must be identical.

### Step 4.1.5: Write Output

**CRITICAL:** Write to `insight-summary.md` at project root (NOT in 12-synthesis/).

**Before writing, verify these three things:**
1. The file path is `insight-summary.md` at the project root -- no dot prefix, no subdirectory.
2. The content includes the complete YAML frontmatter block (delimited by `---`).
3. The content includes the HTML stats grid between the opening paragraph and the first horizontal rule.

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

**End of Phase 4 Competitive Intelligence Arc Workflow**
