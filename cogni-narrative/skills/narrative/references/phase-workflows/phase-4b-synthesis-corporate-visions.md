# Phase 4b: Arc-Specific Insight Summary (corporate-visions)

**Arc Framework:** Why Change -> Why Now -> Why You -> Why Pay

**Research Type:** All types | **Arc:** `corporate-visions` (Tier 2)

**Objective:** Generate insight-summary.md using Corporate Visions' B2B acquisition framework with journalistic storytelling patterns.

**Critical:** This phase generates `insight-summary.md` using the arc-specific framework.

**Inputs:** Dimension syntheses (Phase 3), arc entities (findings, sources, trends-act), DIMENSION_REGISTRY, ENTITY_REGISTRIES

**Output:** `insight-summary.md` at project root (1,450-1,900 words)

---

## Step 4.1: Generate Insight Summary

**Objective:** Transform synthesis-cross-dimensional.md (400-600 words) into insight-summary.md (1,450-1,900 words) using corporate-visions journalistic patterns.

**Input:** `12-synthesis/synthesis-cross-dimensional.md` (just created)
**Output:** `insight-summary.md` at project root

**Arc-Specific Patterns:** Why Change -> Why Now -> Why You -> Why Pay
**Word Count:** 1,450-1,900 words (3-4x expansion)

### Step 4.1.1: Load Evidence Entities (Context Tier 2)

**Purpose of this step:** Before generating the narrative, you need a mental inventory of available evidence. The quality of the insight summary depends entirely on how well you ground claims in real entities. Think of this step as "loading ammunition" -- every finding and source you load here becomes a potential citation, wikilink, or data point in the final output.

**For evidence grounding:**
- Load top 20 findings from `04-findings/data/` (quality_score >= 0.65)
- Load top 15 sources from `07-sources/data/` (reliability_score >= 0.8)

**Implementation:** Use Glob + Read to load entity files, parse frontmatter.

**After loading, reason about the evidence before proceeding:**

> **Think:** Now that you have loaded the findings and sources, mentally categorize them:
> 1. Which findings reveal an *unconsidered need* (something counterintuitive or overlooked)? These feed **Why Change**.
> 2. Which findings contain *timelines, deadlines, or urgency indicators*? These feed **Why Now**.
> 3. Which findings suggest *strategic capabilities or competitive advantages*? These feed **Why You**.
> 4. Which findings contain *cost data, risk quantification, or financial impact*? These feed **Why Pay**.
> 5. Which sources are the most authoritative (highest reliability_score)? Prioritize these for high-impact citations.
>
> Hold this mental map -- you will use it in Step 4.1.3 to select evidence for each arc element.

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

> **Think:** Verify that all 6 counts are integers (not empty strings). If any directory is missing or empty, the count should be 0, not blank. These exact numbers will appear in both the YAML frontmatter AND the HTML stats grid -- they must match exactly.

### Step 4.1.3: Extended Thinking Transformation

This is the most cognitively demanding step. You are transforming a 400-600 word analytical synthesis into a 1,450-1,900 word executive narrative. This is NOT simply adding words -- it is a rhetorical transformation that reframes evidence through the Corporate Visions persuasion framework.

**Before you begin writing, complete these reasoning sub-steps in order:**

---

#### Sub-step A: Internalize the Source Material

Read `synthesis-cross-dimensional.md` (400-600 words) carefully. Before writing anything:

> **Think:** Answer these questions about the source material:
> 1. What is the single most surprising or counterintuitive finding? (This will become your narrative hook.)
> 2. What are the 2-3 cross-dimensional patterns or tensions? (These become structural threads across arc elements.)
> 3. What is the core research question or problem space? (This becomes your subtitle.)
> 4. What does the source material say explicitly, and what does it *imply* but not state? (You may develop implications, but you must not fabricate claims beyond what the evidence supports.)

---

#### Sub-step B: Plan the Arc Element Mapping

Before writing any element, decide which evidence goes where. Each piece of evidence should be used exactly once (do not repeat the same finding across multiple elements).

> **Think:** Create a mental allocation plan:
>
> **Why Change** (400-500 words) -- needs evidence of an unconsidered need:
> - Which 2-3 findings reveal something executives likely overlook or underestimate?
> - What is the "status quo assumption" you can challenge? Frame it as: "Most organizations think X. But research shows Y."
> - Which sources support this reframing?
>
> **Why Now** (300-400 words) -- needs forcing functions with timelines:
> - Which 2-3 findings or trends contain specific deadlines, regulatory dates, or market tipping points?
> - If no explicit timelines exist, which findings imply accelerating change or narrowing windows?
> - What quantified consequences can you extract (costs, penalties, market shifts)?
>
> **Why You** (400-500 words) -- needs strategic capabilities:
> - Which 2-3 findings suggest actionable strategic positions or capabilities?
> - Can you frame these as Power Positions using IS-DOES-MEANS? (Each position: what it IS concretely, what it DOES for the reader using "You" language, what it MEANS competitively -- why competitors cannot easily replicate.)
> - Which findings support the competitive moat argument?
>
> **Why Pay** (200-300 words) -- needs cost quantification:
> - Which findings contain financial data, risk metrics, or cost implications?
> - Can you stack 3-4 cost dimensions into a compound impact calculation?
> - What simple ratio can you end with? (e.g., "Action costs less than inaction by N-Mx.")
>
> **Wikilink allocation:** You need 40-50 total wikilinks. Plan roughly 10-13 per element. Wikilinks should reference entity files using `[[entity-slug]]` format, linking to findings, concepts, trends, or other entities loaded in Step 4.1.1.

---

#### Sub-step C: Craft the Title and Hook

> **Think:** The title must be arc-specific and compelling -- never "Insight Summary" or anything generic. It should:
> - Capture the unconsidered need (Why Change) in provocative language
> - Be 6-12 words, suitable as a report title
> - Create cognitive dissonance or curiosity
>
> **Think:** The hook paragraph (150-200 words) must open with the most surprising quantified finding. Follow this pattern:
> - Sentence 1: Surprising data point with citation. (e.g., "Organizations investing 40% more achieve 60% lower results.")
> - Sentence 2-3: Challenge the conventional wisdom this finding overturns.
> - Remaining sentences: Preview the arc's rhetorical progression (unconsidered need, urgency, strategic response, business case) without revealing all details.
> - Final sentence: Transition into Why Change.

---

#### Sub-step D: Write Each Arc Element

Now write each element in order. For each element, follow the specific reasoning process below.

**Element 1 -- Why Change: Unconsidered Needs (400-500 words)**

> **Think before writing:**
> - What is the status quo assumption? State it in one sentence.
> - What does the evidence reveal that contradicts or complicates this assumption?
> - How does this create a competitive gap between those who recognize it and those who do not?

Write using PSB (Problem-Solution-Benefit) structure:
- **Problem (~150 words):** State the current assumption and why it is incomplete. Use contrast structure: "Most organizations think X. But research shows Y." Ground with 2-3 citations.
- **Solution (~150 words):** Present the unconsidered reality the research reveals. This is NOT a product pitch -- it is a reframing of the problem space. Ground with 2-3 citations.
- **Benefit (~100-200 words):** Articulate the competitive advantage for early recognizers. End with a forward-looking implication that transitions toward urgency.

> **Self-check after writing:** Does this section make the reader uncomfortable about their current assumptions? If it merely confirms what they already believe, the unconsidered need is not strong enough -- revise.

**Element 2 -- Why Now: Forcing Functions (300-400 words)**

> **Think before writing:**
> - What are the 2-3 external pressures that make action time-sensitive?
> - For each forcing function: What is the specific deadline or timeline? What is the quantified consequence of missing it?
> - How do these forcing functions converge to create compounding urgency?

Write by stacking 2-3 forcing functions:
- **Forcing function 1 (~100 words):** External pressure + specific deadline from evidence. Use exact dates/quarters, not "soon" or "rapidly."
- **Forcing function 2 (~100 words):** Second converging pressure + quantified consequence. Use specific costs/penalties, not "financial risk."
- **Window of opportunity (~100-150 words):** Contrast early movers vs. late starters. Show the window closing by comparing timelines across forcing functions. End with transition toward strategic response.

> **Self-check after writing:** Does every forcing function have a specific date or timeline AND a quantified consequence? Vague urgency ("the market is changing") fails this quality gate -- revise with specifics.

**Element 3 -- Why You: Unique Positioning (400-500 words)**

> **Think before writing:**
> - Given the unconsidered need (Element 1) and urgency (Element 2), what strategic capabilities should the reader build?
> - Can you define 2-3 Power Positions? A Power Position is a capability that: (a) is specific and concrete, (b) delivers quantified outcomes, and (c) creates a moat competitors cannot easily replicate.
> - For each Power Position, how will you structure the IS-DOES-MEANS layers?

Write 2-3 Power Positions using IS-DOES-MEANS for each:
- **IS (1-2 sentences):** What this capability is -- concrete, specific, not a buzzword. Give it a name.
- **DOES (3-5 sentences):** What it does for the reader. Use You-Phrasing throughout ("You reduce...", "Your teams...", "You achieve..."). Quantify outcomes with Number Plays where evidence supports it.
- **MEANS (2-3 sentences):** Why competitors struggle to replicate this. Explain the moat: time investment, tacit knowledge, institutional learning, integration complexity. This is what makes the position defensible.

> **Self-check after writing:** For each Power Position, verify: (a) IS is concrete enough to act on, (b) DOES uses "You/Your" at least twice, (c) MEANS explains a specific barrier to replication. If any Power Position reads like generic advice ("invest in training"), it is not a true Power Position -- revise.

**Element 4 -- Why Pay: ROI Justification (200-300 words)**

> **Think before writing:**
> - What are 3-4 distinct cost dimensions of inaction? (e.g., regulatory penalties, talent premium, lost revenue, opportunity cost)
> - For each cost dimension, what specific number can you extract or derive from the evidence?
> - What is the total compound cost over a 3-year horizon?
> - What simple ratio compares action vs. inaction?

Write a compound impact calculation:
- **Cost dimension stacking (~150-200 words):** Present 3-4 cost dimensions, each with a specific number and a citation. Use a consistent time horizon (3 years, which is standard executive planning).
- **Simple ratio conclusion (~50-100 words):** Reduce the entire business case to one undeniable comparison. End with a closing sentence in the format: "Action costs less than inaction by N-Mx" or "The choice: invest X strategically, or lose Y reactively."

> **Self-check after writing:** Does the final sentence of this element -- and of the entire narrative -- pass the "boardroom test"? Would an executive nod and say "that is clear"? If the comparison requires explanation, simplify it further.

---

#### Sub-step E: Review Narrative Coherence

Before finalizing, verify the rhetorical flow across all four elements:

> **Think:** Read through the full narrative from hook to closing and verify:
> 1. **Hook -> Why Change:** Does the hook's surprising finding naturally set up the unconsidered need?
> 2. **Why Change -> Why Now:** Does the unconsidered need (the problem) make the forcing functions feel inevitable?
> 3. **Why Now -> Why You:** Does the urgency make the reader *want* the strategic capabilities you describe?
> 4. **Why You -> Why Pay:** Do the Power Positions make the cost comparison feel like a natural conclusion?
> 5. **Transitions:** Is there a clear transitional sentence between each element? (Use the arc-definition transition patterns: "This gap between X and Y defines the challenge," "Three converging forces make action urgent," "Organizations that thrive don't just react -- they build capabilities," "The cost of delay compounds.")
>
> If any transition feels forced or if an element could stand alone without the preceding one, the rhetorical chain is broken -- revise the transition or adjust the element's framing to create dependency.

---

#### Sub-step F: Verify Quantitative Requirements

> **Think:** Before moving to validation, do a quick count:
> - **Word count:** Is the total between 1,450 and 1,900 words? If under, identify which element is thin and add evidence grounding. If over, trim the least essential supporting sentences.
> - **Wikilinks:** Count all `[[entity-slug]]` references. Target: 40-50. If under 40, add entity references where claims lack grounding. If over 50, consolidate where multiple wikilinks appear in the same sentence.
> - **Citations:** Count all `<sup>[N](source.md)</sup>` references. Target: >= 8 finding citations. If under 8, identify unsupported claims and add citations from your evidence inventory (Step 4.1.1).
> - **Element word counts:** Why Change 400-500, Why Now 300-400, Why You 400-500, Why Pay 200-300, Hook 150-200. Each must be within target (+/-50 words).

---

**Structure:**
```markdown
---
title: "{Arc-Specific Compelling Title}"
subtitle: "{Research Question}"
arc_id: "corporate-visions"
arc_display_name: "Corporate Visions"
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

## Why Change: Unconsidered Needs

{350-450 words with evidence grounding}

## Why Now: Forcing Functions

{350-450 words with evidence grounding}

## Why You: Unique Positioning

{350-450 words with evidence grounding}

## Why Pay: ROI Justification

{350-450 words with evidence grounding}
```

### Step 4.1.4: Validate Output

**Before checking the list, reason about overall quality:**

> **Think:** Step back from the details and evaluate the narrative as a whole:
> 1. Would an executive who reads only the title and hook want to continue reading?
> 2. Does the narrative *build* rhetorically -- each element increasing the reader's conviction -- or does it merely list information under four headings?
> 3. Is there a single, clear "so what?" that the reader walks away with?
>
> If the answer to any of these is "no," identify the weakest element and revise it before proceeding to the checklist.

**Quality gates (check in this priority order -- stop and fix before continuing if any critical gate fails):**

**Critical gates (must pass -- narrative is unusable without these):**
- [ ] Word count: 1,450-1,900 words
- [ ] All 4 arc elements present with arc-specific headers
- [ ] Title: Arc-specific (not "Insight Summary")
- [ ] Arc framework consistent with synthesis-cross-dimensional.md

**Evidence gates (must pass -- narrative lacks credibility without these):**
- [ ] Evidence grounding: >= 8 finding citations
- [ ] Entity wikilinks: 40-50 total

**Presentation gates (must pass -- output is incomplete without these):**
- [ ] Frontmatter contains all 6 `stats_*` fields with integer values
- [ ] Inline HTML stats grid present between opening paragraph and first `---`
- [ ] Stats grid values match `stats_*` frontmatter fields
- [ ] Stats grid labels match project language (DE/EN)

**If any gate fails:**

> **Think:** Identify the specific failure, then determine the minimal fix:
> - Word count too low? Add evidence grounding to the thinnest element (do NOT pad with filler).
> - Word count too high? Trim the least essential supporting sentences from the longest element.
> - Missing citations? Return to your evidence inventory from Step 4.1.1 and identify unsupported claims.
> - Wikilink count off? Scan for entity mentions that lack `[[]]` wrapping, or remove gratuitous links.
> - Stats mismatch? Re-count using Step 4.1.2 and update both frontmatter and HTML grid.
>
> After fixing, re-validate ALL gates (not just the failed one) to ensure the fix did not break something else.

### Step 4.1.5: Write Output

**CRITICAL:** Write to `insight-summary.md` at project root (NOT in 12-synthesis/).

> **Think before writing:** Confirm the file path. The output file is `insight-summary.md` at the project root directory -- the same level as directories like `04-findings/`, `12-synthesis/`, etc. It is NOT inside any subdirectory. It has NO dot prefix.

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

**End of Phase 4 Corporate Visions Arc Workflow**
