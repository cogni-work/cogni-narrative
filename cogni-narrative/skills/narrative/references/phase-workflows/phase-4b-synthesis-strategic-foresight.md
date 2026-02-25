# Phase 4b: Arc-Specific Insight Summary (strategic-foresight)

**Arc Framework:** Signals -> Scenarios -> Strategies -> Decisions

**Research Type:** All types | **Arc:** `strategic-foresight` (Tier 3)

**Objective:** Generate insight-summary.md using Strategic Foresight framework with journalistic storytelling patterns.

**Critical:** This phase generates `insight-summary.md` using the arc-specific framework.

**Inputs:** Dimension syntheses (Phase 3), arc entities (findings, sources, trends-all, megatrends), DIMENSION_REGISTRY, ENTITY_REGISTRIES

**Output:** `insight-summary.md` at project root (1,450-1,900 words)

---

## Step 4.1: Generate Insight Summary

**Objective:** Transform synthesis-cross-dimensional.md (400-600 words) into insight-summary.md (1,450-1,900 words) using strategic-foresight journalistic patterns.

**Input:** `12-synthesis/synthesis-cross-dimensional.md` (just created)
**Output:** `insight-summary.md` at project root

**Arc-Specific Patterns:** Signals -> Scenarios -> Strategies -> Decisions
**Word Count:** 1,450-1,900 words (3-4x expansion)

### Step 4.1.1: Load Evidence Entities (Context Tier 3)

**For evidence grounding:**
- Load top 20 findings from `04-findings/data/` (quality_score >= 0.65)
- Load top 15 sources from `07-sources/data/` (reliability_score >= 0.8)
- Load dimension-scoped trends from `11-trends/data/` (all planning horizons)

**Implementation:** Use Glob + Read to load entity files, parse frontmatter.

**Reasoning guidance for entity loading:**

Before loading, think through what each entity type contributes to this specific arc:

1. **Findings** serve as the evidence backbone. When loading findings, mentally categorize each by which arc element it will support:
   - Findings about emerging patterns or anomalies -> Signals
   - Findings about structural drivers or macro forces -> Scenarios (as scenario axes)
   - Findings about organizational capabilities or actions -> Strategies
   - Findings about timing, urgency, or decision windows -> Decisions

2. **Sources** establish credibility. After loading, note which sources are most authoritative for forward-looking claims vs. current-state claims. Strategic foresight narratives require sources that credibly support future-oriented assertions, not just retrospective analysis.

3. **Trends across all planning horizons** are the primary differentiator for Tier 3 arcs. After loading trends, perform this mental sort:
   - Group trends by `planning_horizon` (or `urgency` field): "Watch" trends feed the Signals element, "Act" trends feed Decisions, and all trends inform Scenarios
   - Identify trend pairs that point in contradictory directions -- these become scenario axes
   - Note trends that span multiple dimensions -- these indicate convergence points useful for Scenarios and Strategies

4. **Megatrends** (if present in `06-megatrends/data/`) are scenario axis generators. Load them and think: "Which 2-3 megatrends create the most divergent futures when their extremes are combined?" These become the structural skeleton of the Scenarios element.

**After loading, verify you have sufficient material:**
- At least 3-4 "Watch" trends for the Signals element
- At least 2 megatrends or macro-level trends for scenario axis construction
- At least 8 findings with quality_score >= 0.65 for citation grounding across all 4 elements
- If any category is thin, note this -- you will need to lean more heavily on the cross-dimensional synthesis content for that element

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

**Before proceeding:** Confirm you have all 6 integer counts stored. These exact values must appear in both the YAML frontmatter `stats_*` fields AND the inline HTML stats grid. Any mismatch between frontmatter and grid will fail validation.

### Step 4.1.3: OUTPUT TEMPLATE

**CRITICAL: Read this template BEFORE any extended thinking. This is the EXACT structure you must produce.**

**Rule: EXACTLY 4 `##` headers. No creative alternatives. No additional sections. No renaming.**

**Instruction: Fill in each section in order. Do NOT reorganize, rename, or add sections.**

**English headers:**
- `## Signals: Early Indicators`
- `## Scenarios: Future States`
- `## Strategies: Adaptive Approaches`
- `## Decisions: Action Framework`

**German headers (if `language: de`):**
- `## Signale: Frühindikatoren`
- `## Szenarien: Zukunftsbilder`
- `## Strategien: Adaptive Ansätze`
- `## Entscheidungen: Handlungsrahmen`

**Structure:**
```markdown
---
title: "{Arc-Specific Compelling Title}"
subtitle: "{Research Question}"
arc_id: "strategic-foresight"
arc_display_name: "Strategic Foresight"
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

## Signals: Early Indicators

{350-450 words with evidence grounding}

## Scenarios: Future States

{350-450 words with evidence grounding}

## Strategies: Adaptive Approaches

{350-450 words with evidence grounding}

## Decisions: Action Framework

{350-450 words with evidence grounding}
```

### Step 4.1.4: Extended Thinking Sub-steps

**This is the most cognitively demanding step.** Follow the sub-steps below in sequence. Do not skip ahead -- each sub-step produces an intermediate reasoning artifact that the next sub-step depends on.

#### Sub-step A: Internalize the Source Material

Read `synthesis-cross-dimensional.md` (400-600 words). Before doing anything else, answer these questions in your reasoning:

1. **What is the core thesis?** State it in one sentence. This becomes the anchor for all 4 arc elements.
2. **What research question does this address?** This becomes the subtitle and frames the narrative hook.
3. **What are the 2-3 most surprising or counterintuitive findings?** These are candidates for Signals and for the opening hook.
4. **What tensions or contradictions exist in the evidence?** These tensions become scenario axes.
5. **What practical implications are mentioned or implied?** These feed Strategies and Decisions.

#### Sub-step B: Map Evidence to Arc Elements

Now, with the source material internalized, explicitly assign your loaded entities to the 4 arc elements. Think through each mapping before writing:

**Signals (Weak Signals and Early Indicators):**
- Which findings describe *emerging* patterns (not established ones)?
- Which trends have urgency="Watch" or a distant planning_horizon? These are your primary signals.
- Which data points are counterintuitive or contradictory? Weak signals often appear as anomalies.
- Target: 3-5 distinct signals, each grounded in at least one finding or trend entity.

**Scenarios (Alternative Future States):**
- Which megatrends create the highest-leverage scenario axes? A good axis has two plausible extremes (e.g., "centralized vs. decentralized," "rapid adoption vs. resistance").
- Select 2 megatrends (or major macro-trends) as your scenario axes. Their cross-product yields 2-3 distinct scenarios.
- For each scenario: which trends from *different planning horizons* reinforce its plausibility? A scenario is credible when short-term "Act" trends and long-term "Watch" trends both point toward it.
- Each scenario must be internally consistent -- signals must reinforce each other within each scenario.
- Scenarios must be genuinely divergent (incompatible futures), not variations on the same theme.

**Strategies (Robust Actions):**
- Which actions or recommendations from the synthesis work across *all* scenarios you constructed?
- Identify "no-regret moves" -- actions valuable regardless of which future materializes.
- Identify "option-creating moves" -- actions that increase flexibility without committing to one scenario.
- Strategies must reference the scenarios by name to demonstrate robustness analysis.

**Decisions (Near-Term Choices):**
- What decisions must be made *now* (before uncertainty resolves)?
- For each decision, identify the trigger signal -- what observable development would indicate it is time to escalate or change course?
- Assess reversibility: flag which decisions are easily reversible (low risk to act now) vs. irreversible (worth waiting for more signal clarity).
- Decisions must connect back to the Strategies -- they are the concrete "first moves" of the strategic playbook.

#### Sub-step C: Construct the Narrative Hook (Opening Paragraph)

Before writing the hook, reason about what makes strategic foresight hooks compelling:

- The hook must establish *genuine uncertainty* -- not a known trend, but an open question about which future will unfold.
- Use the pattern: "[Signal 1] suggests [future A]. [Signal 2] suggests [future B]. [Signal 3] makes both plausible. The future is [uncertainty dimension] -- and that creates strategic opportunity."
- Ground with 1-2 citations from your strongest findings.
- Target: 150-200 words.
- The hook must make the reader feel that *not thinking about multiple futures* is a strategic risk.

#### Sub-step D: Draft Each Arc Element

For each of the 4 elements, follow this atomic sequence:

1. **Write the section header** using the exact arc-specific format:
   - `## Signals: Early Indicators`
   - `## Scenarios: Future States`
   - `## Strategies: Adaptive Approaches`
   - `## Decisions: Action Framework`

2. **Write the opening sentence** that establishes the element's purpose in the narrative flow. Think: "How does this element logically follow from the previous one?" Use the transition patterns:
   - Hook -> Signals: "Four weak signals suggest divergent futures."
   - Signals -> Scenarios: "These signals combine into [N] plausible scenarios."
   - Scenarios -> Strategies: "Robust strategies create value regardless of which scenario unfolds."
   - Strategies -> Decisions: "Executing robust strategies requires near-term decisions about [areas]."

3. **Draft the body** using these element-specific reasoning guides:

   **For Signals (350-450 words):**
   - Present each signal as an observed data point, not an opinion. Use "Watch" trends and findings.
   - Quantify directional change where possible (e.g., "X% in 2020 to Y% in 2026").
   - Frame signals as "stress fractures" or "early indicators," not established trends.
   - Show at least one pair of contradictory signals to establish genuine uncertainty.
   - Include 8-12 wikilinks to trend and finding entities.

   **For Scenarios (350-450 words):**
   - Name each scenario distinctively (e.g., "Platform Capitalism," not "Scenario 1").
   - Build each scenario from the signal combinations established in the previous section.
   - Use megatrends as the structural axes -- state the axis explicitly (e.g., "Axis: Data Sovereignty Stringency -- Low vs. High").
   - Show how trends from *different planning horizons* converge in each scenario.
   - For each scenario, state one concrete implication for the organization/reader.
   - Include 8-12 wikilinks to megatrend, trend, and finding entities.

   **For Strategies (350-450 words):**
   - Categorize strategies as: no-regret moves, option-creating moves, and scenario-specific hedges.
   - For each strategy, explicitly state which scenarios it serves (e.g., "This strategy creates value in both [Scenario A] and [Scenario B]").
   - Ground strategies in findings that demonstrate feasibility or precedent.
   - Include 8-12 wikilinks to finding, source, and trend entities.

   **For Decisions (350-450 words):**
   - Frame decisions as time-bound choices with explicit decision triggers.
   - For each decision, state: what to decide, when to decide, what signal triggers escalation, and whether the decision is reversible.
   - Connect each decision to the strategies it enables.
   - End with a closing sentence that emphasizes decision-making under uncertainty, not prediction. Use the closing pattern: "The goal isn't predicting which future arrives -- it's building capability to thrive in any of them."
   - Include 8-12 wikilinks to finding, source, and trend entities.

4. **Count wikilinks** after drafting each element. Running total must reach 40-50 across all 4 elements. If you are below pace after an element (e.g., fewer than 10 after Signals), increase density in subsequent elements. If you are above pace, reduce to avoid clutter.

#### Sub-step E: Generate the Title

Think about what makes a strategic foresight title compelling:

- It should reference the *domain* of uncertainty, not just the topic (e.g., "Navigating the Identity Crossroads" rather than "Digital Identity Analysis").
- It should imply multiple futures (foresight framing), not a single prediction.
- It must NOT be generic (never use "Insight Summary" or "Strategic Analysis").
- Test: does the title make a reader curious about *which* future will unfold? If not, revise.

#### Sub-step F: Assemble the Complete Document

**CRITICAL LANGUAGE CHECK:** If `language: de`, ALL generated text MUST use proper Unicode umlauts (ä, ö, ü, Ä, Ö, Ü, ß). Writing "fuer" instead of "für", "Aenderung" instead of "Änderung", or "Uebergangsfrist" instead of "Übergangsfrist" is a BLOCKING DEFECT. ASCII transliterations (ae, oe, ue) are ONLY for file names and slugs, NEVER for body text, titles, or headings.

Now assemble all components into the final structure. Before writing, verify you have:

- [ ] Title (from Sub-step E)
- [ ] Research question for subtitle
- [ ] All 6 stats counts (from Step 4.1.2)
- [ ] Opening hook paragraph (from Sub-step C)
- [ ] All 4 arc element sections (from Sub-step D)
- [ ] Closing sentence in the Decisions section

Assemble all components into the final document, using the exact template from Step 4.1.3. Pay careful attention to the frontmatter field names and the stats grid HTML placement.

**Now fill in the template from Step 4.1.3. Write each section in sequence. Do NOT deviate from the template structure.**

### Step 4.1.5: Validate Output

**HARD STRUCTURAL GATE (check FIRST -- blocks all other gates):**

Count the `##` headers in the narrative body (below frontmatter):
- [ ] EXACTLY 4 `##` headers (not more, not fewer)
- [ ] Headers match the arc's exact element names from the template in Step 4.1.3
- [ ] Headers appear in the correct arc sequence (Signals → Scenarios → Strategies → Decisions)
- [ ] No `##` headers exist outside the 4 arc elements

If this gate fails: STOP. Do NOT rename sections to fix. REWRITE using the template from Step 4.1.3.
The content was generated for the wrong structure and cannot be repaired by renaming.

**Before checking the gates below, re-read your complete output and ask yourself these diagnostic questions:**

1. **Narrative coherence:** Does each section logically flow from the previous one? Would a reader who skips to Strategies still understand what scenarios they reference?
2. **Foresight integrity:** Are the scenarios genuinely divergent (incompatible futures), or are they variations on the same theme? If you can merge two scenarios without contradiction, they are not divergent enough.
3. **Evidence density:** Can you point to a specific finding or trend entity for every major claim? If any paragraph lacks evidence grounding, it will read as opinion rather than analysis.
4. **Wikilink distribution:** Are wikilinks spread across all 4 elements, or clustered in one? Clustering suggests uneven evidence grounding.
5. **Word budget:** Did any element significantly overshoot or undershoot its 350-450 word target? Overshooting usually means unfocused writing; undershooting usually means missing evidence or reasoning.

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

**If any gate fails, fix before proceeding.** Common failure patterns and their fixes:

| Failure | Likely Cause | Fix |
|---------|-------------|-----|
| Word count too low | Elements lack sufficient evidence grounding | Add more finding/trend citations with contextual sentences |
| Word count too high | Redundant content across elements | Check for repeated claims between Signals and Scenarios; each element must serve a distinct purpose |
| Wikilinks below 40 | Entity names not linked | Re-scan loaded entities and link every mention of a finding, trend, or source by its entity name |
| Wikilinks above 50 | Over-linking common terms | Remove wikilinks on second+ mentions of the same entity within a section |
| Scenarios not divergent | Axes too similar | Revisit megatrend selection in Sub-step B; choose axes with higher orthogonality |
| Stats mismatch | Frontmatter and grid created independently | Copy exact integer values from Step 4.1.2 into both locations |

### Step 4.1.6: Write Output

**CRITICAL:** Write to `insight-summary.md` at project root (NOT in 12-synthesis/).

**Before writing, confirm the file path:** The output must be written to `{project_root}/insight-summary.md`. Think: "What is the project root directory?" It is the directory that contains the numbered entity directories (04-findings/, 06-megatrends/, 11-trends/, 12-synthesis/, etc.). The file goes at that level, NOT inside any subdirectory.

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

**End of Phase 4 Strategic Foresight Arc Workflow**
