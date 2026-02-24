# Story Arc Registry

## Overview

This registry indexes all available story arcs for the cogni-narrative plugin. Each arc provides a different narrative framework for transforming structured content (research syntheses, analyses, reports) into compelling executive narratives.

## Arc Selection Logic

1. **Explicit selection**: Caller specifies `arc_id` directly (highest priority)
2. **Content type mapping**: Automatic detection based on `content_type` metadata
3. **Content analysis**: Keyword density analysis of input content
4. **Fallback default**: corporate-visions

## Available Story Arcs

### 1. Corporate Visions (Default)

**Arc ID:** `corporate-visions`
**Display Name:** Corporate Visions
**Elements:** Why Change → Why Now → Why You → Why Pay

**Best For:**
- Market research
- Competitive positioning
- Sales enablement
- B2B value propositions

**Detection Signals:**
- `content_type: "generic"` or `"market"`
- Default fallback when no other arc matches

**Word Targets:**
- Hook: 150-200 words
- Why Change: 400-500 words
- Why Now: 300-400 words
- Why You: 400-500 words
- Why Pay: 200-300 words
- **Total:** 1,450-1,900 words

**Definition File:** `story-arc/corporate-visions/arc-definition.md`

---

### 2. Technology Futures

**Arc ID:** `technology-futures`
**Display Name:** Technology Futures
**Elements:** What's Emerging → What's Converging → What's Possible → What's Required

**Best For:**
- Technology trend research
- Innovation scouting
- R&D strategy
- Capability roadmapping

**Detection Signals:**
- `content_type: "technology"`
- Keywords (>=15% density): "emerging", "innovation", "capability", "technology", "R&D", "breakthrough"

**Word Targets:**
- Hook: 150-200 words
- What's Emerging: 350-450 words
- What's Converging: 350-450 words
- What's Possible: 350-450 words
- What's Required: 200-350 words
- **Total:** 1,450-1,900 words

**Definition File:** `story-arc/technology-futures/arc-definition.md`

---

### 3. Competitive Intelligence

**Arc ID:** `competitive-intelligence`
**Display Name:** Competitive Intelligence
**Elements:** Landscape → Shifts → Positioning → Implications

**Best For:**
- Competitive analysis
- Market positioning
- Threat assessment
- Strategic differentiation

**Detection Signals:**
- `content_type: "competitive"`
- Keywords (>=12% density): "competitor", "market share", "positioning", "differentiation", "threat", "rivalry"

**Word Targets:**
- Hook: 150-200 words
- Landscape: 350-450 words
- Shifts: 300-400 words
- Positioning: 400-500 words
- Implications: 250-350 words
- **Total:** 1,450-1,900 words

**Definition File:** `story-arc/competitive-intelligence/arc-definition.md`

---

### 4. Strategic Foresight

**Arc ID:** `strategic-foresight`
**Display Name:** Strategic Foresight
**Elements:** Signals → Scenarios → Strategies → Decisions

**Best For:**
- Long-range planning
- Scenario analysis
- Uncertainty navigation
- Strategic options generation

**Detection Signals:**
- `content_type: "foresight"` or `"scenarios"`
- Keywords (>=10% density): "scenario", "future", "signal", "uncertainty", "planning", "foresight"

**Word Targets:**
- Hook: 150-200 words
- Signals: 300-400 words
- Scenarios: 400-500 words
- Strategies: 350-450 words
- Decisions: 250-350 words
- **Total:** 1,450-1,900 words

**Definition File:** `story-arc/strategic-foresight/arc-definition.md`

---

### 5. Industry Transformation

**Arc ID:** `industry-transformation`
**Display Name:** Industry Transformation
**Elements:** Forces → Friction → Evolution → Leadership

**Best For:**
- Industry analysis
- Sector transformation
- Regulatory impact
- Structural change analysis

**Detection Signals:**
- `content_type: "industry"`
- Keywords (>=12% density): "regulatory", "sector", "structural", "industry", "transformation", "policy"

**Word Targets:**
- Hook: 150-200 words
- Forces: 350-450 words
- Friction: 300-400 words
- Evolution: 400-500 words
- Leadership: 250-350 words
- **Total:** 1,450-1,900 words

**Definition File:** `story-arc/industry-transformation/arc-definition.md`

---

## Arc Detection Algorithm

### Step 1: Explicit Selection

If the caller provides `arc_id` directly, use it without detection.

### Step 2: Content Type Mapping

```javascript
const arcMap = {
  "technology": "technology-futures",
  "competitive": "competitive-intelligence",
  "foresight": "strategic-foresight",
  "scenarios": "strategic-foresight",
  "industry": "industry-transformation",
  "market": "corporate-visions",
  "generic": "corporate-visions"
}

if (content_type in arcMap) {
  detected_arc = arcMap[content_type]
  detection_reason = `content_type="${content_type}"`
}
```

### Step 3: Content Analysis (if no content_type match)

Analyze the input content for keyword density:

```javascript
keyword_sets = {
  "technology-futures": ["emerging", "innovation", "capability", "technology", "R&D", "breakthrough"],
  "competitive-intelligence": ["competitor", "market share", "positioning", "differentiation", "threat", "rivalry"],
  "strategic-foresight": ["scenario", "future", "signal", "uncertainty", "planning", "foresight"],
  "industry-transformation": ["regulatory", "sector", "structural", "industry", "transformation", "policy"]
}

thresholds = {
  "technology-futures": 0.15,
  "competitive-intelligence": 0.12,
  "strategic-foresight": 0.10,
  "industry-transformation": 0.12
}
```

### Step 4: Fallback Default

```javascript
if (!detected_arc) {
  detected_arc = "corporate-visions"
  detection_reason = "default (no specific signals detected)"
}
```

## Arc Directory Structure

Each arc follows this structure:

```
story-arc/{arc-id}/
├── arc-definition.md          # Metadata, elements, detection signals, translations
├── {element1}-patterns.md     # Element 1 transformation patterns
├── {element2}-patterns.md     # Element 2 transformation patterns
├── {element3}-patterns.md     # Element 3 transformation patterns
└── {element4}-patterns.md     # Element 4 transformation patterns
```

## Interactive Selection Format

When presenting arc selection to users:

```
Auto-detected: {arc_display_name} ({detection_reason})

Please select story arc:

Option 1: {Detected Arc} (Recommended)
  Elements: {element1} → {element2} → {element3} → {element4}
  Best for: {use_case_summary}

Option 2: {Alternative Arc 1}
  Elements: {element1} → {element2} → {element3} → {element4}
  Best for: {use_case_summary}

[... remaining options ...]
```

## Extension Guidelines

### Adding New Arcs

1. Choose unique arc_id (lowercase, hyphens)
2. Create directory: `story-arc/{arc-id}/`
3. Create 5 files: arc-definition.md + 4 element pattern files
4. Add entry to this registry
5. Update detection algorithm (content_type mapping, keywords)
6. Add arc-specific workflow in `phase-workflows/`
7. Update SKILL.md

### Quality Standards for New Arcs

- Total word target: 1,450-1,900 words
- 4 elements (consistent with existing arcs)
- Clear detection signals (content_type + keywords)
- German translations for all elements
- Transformation patterns documented
- Example transformations provided
- Quality gates defined
