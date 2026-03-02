# Phase 4b: Arc-Specific Insight Summary (trend-panorama)

**Arc Framework:** Forces -> Impact -> Horizons -> Foundations (TIPS: T -> I -> P -> S)

**Research Type:** `smarter-service` | **Arc:** `trend-panorama` (Tier 4)

**Objective:** Generate insight-summary.md using Trend Panorama framework with TIPS-dimension synthesis and horizon cascade.

**Critical:** This phase generates `insight-summary.md` using the TIPS-native arc framework specifically designed for trend-scout output.

**Inputs:** Trend-scout output (52 trend candidates), dimension syntheses, trend entities, TIPS trend report (if available), DIMENSION_REGISTRY, ENTITY_REGISTRIES

**Output:** `insight-summary.md` at project root (1,450-1,900 words)

---

## Step 4.1: Generate Insight Summary

**Objective:** Transform trend-scout candidates and supporting evidence into insight-summary.md (1,450-1,900 words) using TIPS-native narrative patterns.

**Input:** `trend-scout-output.json` (primary), `tips-trend-report.md` (if exists), dimension syntheses, trend entities
**Output:** `insight-summary.md` at project root

**Arc-Specific Patterns:** Forces (T) -> Impact (I) -> Horizons (P) -> Foundations (S) with Act/Plan/Observe cascade within each element
**Word Count:** 1,450-1,900 words

### Step 4.1.1: Load Trend Evidence (Context Tier 4)

**Tier 4 context:** This arc loads the RICHEST source context of all arcs because it synthesizes an entire trend landscape. Before loading, understand WHY each source type serves this arc:

- **Trend-scout output** provides the structured candidate data with scores, horizons, dimensions, and confidence tiers. It is the BACKBONE of this arc -- every narrative claim traces back to a trend candidate.
- **Trend entities** (11-trends/data/) provide the detailed trend narratives with evidence, implications, and recommended actions. They are the EVIDENCE layer.
- **TIPS trend report** (tips-trend-report.md) provides pre-synthesized TIPS dimension narratives. If available, it dramatically reduces synthesis effort -- extract rather than construct.
- **Dimension syntheses** (12-synthesis/data/) provide cross-trend patterns and interactions within each dimension. They reveal EMERGENT insights beyond individual trends.
- **Executive Summary** (research-hub.md or synthesis-cross-dimensional.md) provides cross-dimensional context for the hook and transitions.

**Think before loading:** This arc processes ~52 trends across 4 dimensions. You cannot give each trend individual attention in 1,450-1,900 words (~30 words per trend). The key skill is SYNTHESIS -- clustering trends into force narratives, impact stories, opportunity portfolios, and capability roadmaps.

**Loading priority:**
1. Load `trend-scout-output.json` from `.metadata/` or source directory (REQUIRED)
2. Load `tips-trend-report.md` from source/project directory (HIGH VALUE if exists)
3. Load dimension syntheses from `12-synthesis/data/` for each TIPS dimension (if available)
4. Load top trend entities from `11-trends/data/` by dimension and horizon (up to 20)
5. Load `research-hub.md` or `synthesis-cross-dimensional.md` for executive context

**Implementation:** Use Glob + Read to discover and load files. Parse trend-scout JSON for structured candidate data.

**After loading, build TREND INVENTORY:**

For each TIPS dimension, catalog:
- Number of trends per horizon (Act/Plan/Observe)
- Top 3 trends by score per horizon
- Subcategory distribution
- Average confidence tier
- Key cross-trend themes

This inventory directly feeds the synthesis in Step 4.1.4.

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

**Language-aware labels:** Check `project_language` from sprint-log.json or trend-scout-output.json. Use German labels (Dimensionen, Konzepte, Erkenntnisse, Aussagen) for `de`, English labels (Dimensions, Concepts, Findings, Claims) for `en`.

**Checkpoint:** Verify all 6 counts are integers (not empty strings). If any directory is missing or empty, the count should be 0, not omitted.

### Step 4.1.3: OUTPUT TEMPLATE

**CRITICAL: Read this template BEFORE any extended thinking. This is the EXACT structure you must produce.**

**Rule: EXACTLY 4 `##` headers. No creative alternatives. No additional sections. No renaming.**

**Instruction: Fill in each section in order. Do NOT reorganize, rename, or add sections.**

**English headers:**
- `## Forces: External Pressures & Market Signals`
- `## Impact: Value Chain Disruption`
- `## Horizons: Strategic Possibilities`
- `## Foundations: Capability Requirements`

**German headers (if `language: de`):**
- `## Kräfte: Externe Einflüsse & Marktsignale`
- `## Wirkung: Wertschöpfungsdynamik`
- `## Horizonte: Strategische Möglichkeiten`
- `## Fundamente: Kompetenzanforderungen`

**Structure:**
```markdown
---
title: "{Arc-Specific Compelling Title}"
subtitle: "{Research Question or Industry/Subsector Focus}"
arc_id: "trend-panorama"
arc_display_name: "Trend Panorama"
word_count: {1450-1900}
date_created: "{ISO 8601}"
total_trends: {52 or actual count}
horizon_distribution:
  act: {count}
  plan: {count}
  observe: {count}
stats_syntheses: {count from 12-synthesis/data/}
stats_megatrends: {count from 06-megatrends/data/}
stats_trends: {count from 11-trends/data/}
stats_concepts: {count from 05-domain-concepts/data/}
stats_findings: {count from 04-findings/data/}
stats_claims: {count from 10-claims/data/}
---

# {Arc-Specific Title}

*{Research Question or Industry Focus subtitle}*

{Opening paragraph with panoramic hook -- 150-200 words}

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

## Forces: External Pressures & Market Signals

{350-450 words: T-dimension synthesis with Act->Plan->Observe cascade}

## Impact: Value Chain Disruption

{350-450 words: I-dimension synthesis with Act->Plan->Observe cascade}

## Horizons: Strategic Possibilities

{350-450 words: P-dimension synthesis with Act->Plan->Observe cascade}

## Foundations: Capability Requirements

{250-350 words: S-dimension synthesis with Act->Plan->Observe cascade}
```

### Step 4.1.4: Extended Thinking Sub-steps

This is the most cognitively demanding step. The trend-panorama arc requires SYNTHESIS across 52 trends -- not listing, not summarizing, but weaving individual signals into a coherent strategic narrative. Follow this decomposed reasoning sequence exactly.

---

#### Sub-step A: Build the Trend Inventory

From loaded trend-scout output, construct a mental map:

1. **For each TIPS dimension (T, I, P, S):**
   - How many trends in Act/Plan/Observe?
   - What are the top 3 by score?
   - What subcategory patterns emerge?
   - What confidence distribution? (high/medium/low)

2. **Cross-dimensional patterns:**
   - Which Act-horizon trends across dimensions reinforce each other?
   - Which Plan-horizon trends build on Act-horizon foundations?
   - What Observe-horizon signals connect across dimensions?

3. **Narrative spine identification:**
   - What single cross-dimensional insight makes this landscape DISTINCTIVE?
   - This becomes the hook and the thread connecting all 4 elements.

---

#### Sub-step B: Map Trend Clusters to Elements

For each element, CLUSTER trends into narrative themes. Do NOT plan to mention all 13 trends per dimension individually. Instead:

- **Forces (T):** Identify 3-4 force clusters from externe-effekte trends. Example: "regulatory convergence" (3 regulatory trends), "economic pressure" (2 economic trends), "societal shift" (2 society trends). Remaining trends provide supporting evidence.

- **Impact (I):** Identify 3-4 disruption themes from digitale-wertetreiber trends. Example: "CX transformation" (4 CX trends), "product evolution" (3 product trends), "process automation" (3 process trends). Show cascading effects between subcategories.

- **Horizons (P):** Identify 3-4 opportunity portfolios from neue-horizonte trends. Example: "platform positioning" (3 strategy trends), "leadership innovation" (2 leadership trends), "governance advantage" (2 governance trends). Quantify opportunity windows.

- **Foundations (S):** Identify capability roadmap from digitales-fundament trends. Show dependency chain: culture -> workforce -> technology. Fewer clusters, more sequencing logic.

**Decision rule:** Each trend should contribute to exactly one cluster. High-scoring trends (>0.75) get explicit mention; lower-scoring trends strengthen cluster evidence without individual citation.

---

#### Sub-step C: Generate the Title

Think about what makes this trend landscape DISTINCTIVE before choosing a title:
- What industry/sector is this for?
- What's the dominant cross-dimensional pattern?
- What would make a strategic executive stop and read?

**Constraints:** Title MUST NOT be "Trend Panorama" or "Trend Analysis." It MUST reflect the specific industry/topic and the arc's panoramic nature.

**Good patterns:**
- "{Industry}: {Number} Trends Reshaping {Specific Area}"
- "{Sector} {Year}: Where Forces Converge"
- "The {Industry} Shift: From {Current State} to {Future State}"

---

#### Sub-step D: Write the Hook Paragraph

The hook (150-200 words) must accomplish:
1. **Establish panoramic scale** -- reference total trend count and dimension breadth
2. **Create urgency** -- highlight Act-horizon trend concentration
3. **Reveal cross-dimensional insight** -- the narrative spine from Sub-step A
4. **Signal the TIPS progression** -- hint at Forces -> Impact -> Horizons -> Foundations

**Think before writing:** What is the single most surprising cross-dimensional finding? Lead with that.

---

#### Sub-step E: Synthesize Each Arc Element (word targets per element)

For EACH element, follow this micro-sequence:

1. **Lead with the element's core assertion** -- what is the dominant pattern across this TIPS dimension? Answer first (Pyramid Principle).

2. **Cascade through horizons:**
   - **Act (lead, ~40% of words):** Highest-scoring, highest-confidence trends. What demands immediate response?
   - **Plan (bridge, ~35% of words):** Emerging trends building on Act foundations. What to prepare for?
   - **Observe (close, ~25% of words):** Weak signals worth monitoring. What could change everything?

3. **Synthesize, don't list:** Each cluster should be a NARRATIVE thread, not a bullet point. Show interactions between trends, not just individual trend descriptions.

4. **Ground in trend data:** Use scores, confidence tiers, and signal intensities as narrative anchors. "Score: 0.84, confidence: high" is more compelling than "very important."

5. **Connect forward:** Each element's closing should create momentum toward the next element:
   - Forces -> "These forces translate into..."
   - Impact -> "Disruption creates openings..."
   - Horizons -> "Capturing these requires..."

6. **Apply arc-specific techniques per element:**
   - Forces: PSB, Forcing Functions, Contrast Structure
   - Impact: Contrast Structure, Compound Impact, Before/After
   - Horizons: You-Phrasing, IS-DOES-MEANS, Opportunity Windows
   - Foundations: IS-DOES-MEANS, Compound Impact, Dependency Sequencing

**Word count discipline:**
- Forces: 350-450 words
- Impact: 350-450 words
- Horizons: 350-450 words
- Foundations: 250-350 words

---

#### Sub-step F: Assemble the Stats Grid

Insert the HTML stats grid between the opening paragraph and the first `---` separator. Use the exact counts from Step 4.1.2.

---

#### Sub-step G: Assemble and Self-Review

**CRITICAL LANGUAGE CHECK:** If `language: de`, ALL generated text MUST use proper Unicode umlauts. See SKILL.md for complete umlaut rules.

Before finalizing, review against these trend-panorama-specific checks:

1. **TIPS coherence:** Does Forces -> Impact -> Horizons -> Foundations tell a connected story? Is there a clear causal chain? (Forces CAUSE Impact, Impact CREATES Horizons, Horizons REQUIRE Foundations)

2. **Horizon cascade consistency:** Does each element follow Act -> Plan -> Observe? Is Act given the most weight? Are Observe signals appropriately speculative?

3. **Synthesis quality:** Count how many individual trend names appear in the narrative. Target: 12-18 named trends out of 52 (remainder contribute to cluster evidence). If >25 trends are named, you're listing not synthesizing. If <8, you're abstracting too much.

4. **Score/confidence usage:** Are trend scores and confidence tiers used as narrative evidence? They should appear naturally: "high-confidence signals (score: 0.84)" not as a data dump.

5. **Evidence balance:** Count citations per element. Target: Forces 5-8, Impact 4-7, Horizons 4-6, Foundations 3-5. Total: 15-25.

6. **Word count:** Verify total 1,450-1,900 and per-element targets.

7. **Title check:** Is the title specific to this industry/research question?

---

**Now fill in the template from Step 4.1.3. Write each section in sequence.**

### Step 4.1.5: Validate Output

**Before checking gates, think about common failure modes for trend-panorama:**
- Trend-panorama narratives often OVER-LIST because 52 trends tempt enumeration. If the narrative reads like a catalog, tighten by replacing individual trend mentions with cluster synthesis.
- The horizon cascade (Act -> Plan -> Observe) can feel mechanical if all 4 elements use identical transition language. Vary the horizon bridge language per element.
- Forces and Impact can blur because external trends and value chain impacts are related. Maintain the distinction: Forces = external pressures ON the organization; Impact = changes WITHIN the value chain.

**HARD STRUCTURAL GATE (check FIRST):**

Count the `##` headers in the narrative body:
- [ ] EXACTLY 4 `##` headers
- [ ] Headers match: Forces, Impact, Horizons, Foundations (exact text from template)
- [ ] Headers in correct TIPS sequence (T -> I -> P -> S)
- [ ] No `##` headers exist outside the 4 arc elements

If this gate fails: STOP. REWRITE using the template from Step 4.1.3.

**Quality gates:**
- [ ] Word count: 1,450-1,900 words
- [ ] All 4 elements present with correct headers
- [ ] Evidence grounding: >= 15 citations
- [ ] Entity wikilinks/citations: total 15-25
- [ ] Title: Arc-specific (not "Trend Panorama")
- [ ] TIPS dimensions correctly mapped (T->Forces, I->Impact, P->Horizons, S->Foundations)
- [ ] Horizon cascade present in each element (Act -> Plan -> Observe)
- [ ] Trend synthesis (not listing): <= 18 individually named trends
- [ ] Frontmatter contains all required fields including `total_trends` and `horizon_distribution`
- [ ] Stats grid present and matching frontmatter values
- [ ] **German umlaut check (if `de`):** ZERO ASCII fallbacks

**If any gate fails:** Identify which sub-step (A-G) produced the defect, return to that sub-step, and fix.

### Step 4.1.6: Write Output

**CRITICAL:** Write to `insight-summary.md` at project root (NOT in 12-synthesis/).

**Before writing, final sanity check:**
1. Confirm file starts with `---` (YAML frontmatter)
2. Confirm `arc_id` is `"trend-panorama"`
3. Confirm `word_count` matches actual word count
4. Confirm `total_trends` matches loaded trend count

**Use Write tool:**
- file_path: `insight-summary.md` (relative to project root)
- content: Complete insight summary with frontmatter

**Verification:**
```bash
if [[ ! -f "insight-summary.md" ]]; then
  echo "ERROR: insight-summary.md not created"
  exit 1
fi
word_count=$(wc -w < insight-summary.md | tr -d ' ')
echo "insight-summary.md created (${word_count} words) at project root"
```

---

## Next Phase

**Phase 5:** Validation verifies all citations reference loaded entities.

---

**End of Phase 4 Trend Panorama Arc Workflow**
