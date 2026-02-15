---
description: Deep dive CV evaluation with 10 mandatory insight quality standards (~8K-15K output tokens)
---

# CV Evaluation — Deep Dive

**Purpose:** Comprehensive, evidence-based critique of the CV in `index.md` with scored evaluation, 10 mandatory insight quality patterns, and optional HTML dashboard. Every claim is cited, every suggestion includes a rewrite example.

> [!TIP]
> **When to use Quick vs Deep Dive:**
>
> | Aspect | Quick (`/evaluate-cv-quick`) | Deep Dive (`/evaluate-cv-deepdive`) |
> |--------|-----|-----------|
> | Output tokens | ~2K-4K | ~8K-15K |
> | Insight patterns | Best-effort | 10/10 mandatory |
> | Evidence citations | Optional | Mandatory (every claim) |
> | Rewrite examples | Optional | Mandatory (every suggestion) |
> | Specificity | ~20-40% | ~80-95% |
> | Best for | Quick checks, iteration cycles | Comprehensive review, major revisions |

## Important Notes
- **Evaluation target:** Evaluates content in `index.md` (single source of truth)
- **Framework reference:** See `.agent/references/cv-evaluation-framework.md` for the complete scoring rubric and criteria
- **Output formats:** Narrative analysis, score card, and optional HTML dashboard
- **Content quality:** This workflow enforces 10 Insight Quality Standards — see Step 3.5

---

## Workflow Steps

### Step 1: Scope Definition

If the user has not already described their evaluation needs, ask questions one by one:

**Question 1 — Scope:**
```
What would you like to evaluate?
1. Entire CV
2. Specific section(s) only
```

If option 2:
```
Which section(s)? (comma-separated)
1. Profile Summary
2. Technical Skills
3. Professional Experience
4. Education
5. Activities (Articles/Certifications/Projects)
6. Language
```

**Question 2 — Improvement Goals:**
```
What kind of improvement are you looking for?
1. General review (overall quality check)
2. Targeted for a specific job/role
3. Career pivot / repositioning
4. ATS optimization focus
5. Specific concern (describe)
```

**Question 3 — Job Target (if applicable):**
```
Would you like to evaluate against a specific job posting?
1. Yes (provide URL or paste description)
2. No (general evaluation)
```

If yes, fetch and parse the job description using the same fallback chain:
- Attempt `curl` → retry → Browser tool → ask user to paste the description

---

### Step 2: Read CV Content

// turbo
1. Read `index.md` completely
2. If evaluating specific section(s), isolate the relevant content
3. If job target provided, extract key requirements, skills, and keywords

Present confirmation:
```
Evaluation scope:
- Target: [Entire CV / Specific section(s)]
- Goal: [General review / Job-targeted / etc.]
- Job target: [Job title at Company / None]
- Mode: Deep Dive (comprehensive evaluation with 10 insight standards)

Proceeding with evaluation...
```

---

### Step 3: Analyze with Framework

Load and apply the evaluation framework from `.agent/references/cv-evaluation-framework.md`.

**For each of the 6 categories, evaluate and score 1-10:**

1. **Content Quality** (25%) — Grammar, completeness, professional tone
2. **Structure & Formatting** (15%) — Logical flow, formatting consistency
3. **Impact & Metrics** (20%) — Quantified achievements, action verbs
4. **ATS Compatibility** (15%) — Keywords, standard headings, parseability
5. **Relevance** (15%) — Alignment to career goals or target job
6. **Professional Presentation** (10%) — First impression, readability, length

Calculate composite score:
```
Composite = (CQ × 0.25) + (SF × 0.15) + (IM × 0.20) + 
            (ATS × 0.15) + (REL × 0.15) + (PP × 0.10)
```

**If job target is provided**, also perform:
- Keyword match analysis (% of job keywords found in CV)
- Requirement gap matrix (must-have / preferred / nice-to-have)

---

### Step 3.5: Apply Insight Quality Standards

**MANDATORY.** After completing the framework analysis, verify your evaluation output includes ALL 10 standards defined in `.agent/references/cv-evaluation-framework.md` § Insight Quality Standards. If any are missing, add them before proceeding to Step 4.

**Checklist (all required):**
- [ ] 1. Positioning Diagnosis — one-line gap framing (current vs. desired narrative)
- [ ] 2. Evidence Citations — every claim quotes specific CV content
- [ ] 3. Rewrite Examples — every suggestion includes replacement text
- [ ] 4. Score Impact Estimates — numerical improvement per recommendation
- [ ] 5. Contradiction Detection — inconsistencies between sections
- [ ] 6. Current Role Priority — deepest analysis on most recent role
- [ ] 7. Career Arc Analysis — map career trajectory explicitly
- [ ] 8. Top 3 Actions + Impact — ranked by estimated score improvement
- [ ] 9. ≥10 Actionable Items — specific enough to implement without guidance
- [ ] 10. ≥80% Specificity — self-check ratio before presenting

---

### Step 4: Generate Results — Narrative

Present a human-readable analysis incorporating all 10 insight standards:

```
# CV Evaluation Results — Deep Dive

## Positioning Diagnosis
[One-line framing of the gap — Standard 1]

## Career Arc Analysis
[Career trajectory mapping — Standard 7]

## Overall Assessment
[2-3 sentence overview citing specific CV content — Standard 2]

## Strengths
- ✅ [Strength 1 with EXACT evidence from CV — Standard 2]
- ✅ [Strength 2 with citation]
- ✅ [Strength 3 with citation]

## Areas for Improvement

### 🔴 High Priority
- [Issue with EXACT quote from CV — Standard 2]
  → Rewrite: [Complete replacement text — Standard 3]
  → Impact: [Score estimate — Standard 4]

### 🟡 Medium Priority
- [Issue with citation]
  → Rewrite: [Replacement text]
  → Impact: [Score estimate]

### 🟢 Low Priority
- [Minor issue with citation]
  → Rewrite: [Replacement text]

## Contradictions Found
[List of inconsistencies — Standard 5]

## Section-by-Section Analysis

### [Current Role — MOST DETAILED — Standard 6]
[In-depth analysis with citations and rewrites]

### [Previous Roles]
[Analysis with citations and rewrites]

### [Other Sections]
[Analysis]

## 🎯 Top 3 High-Impact Actions — Standard 8
| # | Action | Est. Impact |
|---|--------|-------------|
| 1 | [Action with context] | +X.X |
| 2 | [Action with context] | +X.X |
| 3 | [Action with context] | +X.X |

Combined: X.XX → X.XX ([Current Rating] → [Target Rating])
```

If job-targeted, add:
```
## Job Target Analysis: [Job Title] at [Company]

### Keyword Match: [X]%
[List of matched and missing keywords]

### Requirement Gap Analysis
| Requirement | Status | Evidence | Recommendation |
|-------------|--------|----------|----------------|
| [Req 1]     | ✅/⚠️/❌ | [Where] | [How to fix] |
```

**Self-check before presenting:** Count actionable items (Standard 9) and calculate specificity ratio (Standard 10). If below thresholds, enhance before presenting.

---

### Step 5: Generate Results — Score Card

Present the scored summary with score impact row:

```
## Score Card

| Category                | Score | Weight | Weighted | Justification |
|-------------------------|-------|--------|----------|---------------|
| Content Quality         | X/10  | 25%    | X.XX     | [1-line why]  |
| Structure & Formatting  | X/10  | 15%    | X.XX     | [1-line why]  |
| Impact & Metrics        | X/10  | 20%    | X.XX     | [1-line why]  |
| ATS Compatibility       | X/10  | 15%    | X.XX     | [1-line why]  |
| Relevance               | X/10  | 15%    | X.XX     | [1-line why]  |
| Professional Presentation | X/10 | 10%   | X.XX     | [1-line why]  |
|-------------------------|-------|--------|----------|---------------|
| **Composite Score**     |       |        | **X.XX** |               |

Rating: [⭐ Exceptional / ✅ Strong / ⚠️ Adequate / 🔶 Needs Work / 🔴 Major Revision]

**If all top 3 actions implemented:** X.XX → ~X.XX ([Target Rating])
```

---

### Step 6: Optional HTML Dashboard

Ask user:
```
Would you like to generate an interactive web dashboard for these results?
1. Yes
2. No
```

If yes:

**Check for existing dashboard:**
// turbo
```bash
ls docs/evaluation/*.html 2>/dev/null
```

If files exist:
```
Found existing evaluation dashboard(s):
- [filename(s)]

Would you like to:
1. Replace the existing dashboard
2. Create a new file (with timestamp suffix)
```

**Generate the dashboard:**

Create an HTML file at `docs/evaluation/cv-evaluation.html` (or timestamped variant) with:

**Design: Neo-Brutalism Theme**
- Bold black borders (3-4px)
- Vibrant accent colors (bright yellow, electric blue, hot pink)
- Strong typography (large headings, monospace accents)
- Offset box shadows for depth
- Minimal gradients — flat, bold color blocks
- Interactive hover states with transform effects

**Dashboard sections:**
1. **Header** — "CV Evaluation Report — Deep Dive" with date and target info
2. **Positioning Diagnosis** — Highlighted one-liner at top
3. **Career Arc** — Visual career timeline
4. **Composite Score** — Large, prominent score display with rating badge + projected score
5. **Category Breakdown** — Visual score bars with justification tooltips
6. **Strengths & Improvements** — Expandable cards with evidence citations and rewrite examples
7. **Contradictions** — Highlighted contradiction cards
8. **Section Analysis** — Tabbed or accordion view per CV section (current role first, deepest)
9. **Top 3 Actions** — Action cards with impact estimates
10. **Job Match** (if applicable) — Keyword match visualization, gap table
11. **Personal Touch Section** (at bottom):
    - A motivational quote (rotate from a curated list, or select contextually)
    - Generated by: [AI Agent Model Name]
    - "Brought to you by [doctor500](https://github.com/doctor500) • [doctor500/cv](https://github.com/doctor500/cv)"

**Requirements:** Single self-contained HTML file (inline CSS + JS), responsive, print-friendly.

After generation, offer preview in browser or continue.

---

### Step 7: Iteration

After presenting results:
```
What would you like to do next?
1. Re-evaluate after making changes to index.md
2. Deep-dive into a specific section
3. Get rewrite suggestions for specific bullets
4. Evaluate against a (different) job posting
5. Switch to /evaluate-cv-quick for faster iteration
6. Done
```

If option 1: Return to Step 2 with updated content.
If option 2-4: Perform additional targeted analysis.
If option 5: Run the quick workflow.

---

## Example Output (abbreviated)

> **Positioning Diagnosis:** Your CV reads as a **strong 2023-era DevOps engineer**, but not as a **2026 infrastructure engineer leveraging AI**.

> **Career Arc:** Helpdesk → Sysadmin → DevOps → [desired: AI-Infra]. Progression visible but not articulated.

> **🔴 High Priority:** Profile Summary says "passion for automation" but omits AI/ML despite Agentic AI goal.
> → Rewrite: "Infrastructure engineer with 6+ years building cloud-native platforms, now applying AI agents to automate operational workflows."
> → Impact: Est. +1.0 to +1.5

> **Top 3 Actions:**
> | # | Action | Est. Impact |
> |---|--------|-------------|
> | 1 | Reposition summary | +1.0-1.5 |
> | 2 | Add AI project | +0.5-1.0 |
> | 3 | Quantify current role | +0.5 |
> Combined: 6.85 → 8.5-9.0 (✅ Strong → ⭐ Exceptional)

---

## Error Handling

- **Job URL unreachable:** Fallback chain (curl → browser → paste)
- **Section not found in index.md:** Notify user, list available sections
- **Empty CV:** Suggest using `/build-cv-wizard` workflow first
- **Dashboard generation fails:** Fall back to markdown-only output
- **Previous dashboard exists:** Always ask before overwriting
- **Insight standard missing:** Self-check before presenting (Step 4 self-check)

---

_This is the comprehensive evaluation workflow with 10 mandatory insight quality standards. For lightweight, fast iteration, use `/evaluate-cv-quick`._
