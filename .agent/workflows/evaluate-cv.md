---
description: Evaluate and critique CV with scoring framework and optional web dashboard
---

# CV Evaluation Workflow

**Purpose:** Provide fair critique, actionable suggestions, and scored evaluation for the CV in `index.md`. Optionally generate an interactive HTML dashboard to visualize results.

## Important Notes
- **Evaluation target:** Evaluates content in `index.md` (single source of truth)
- **Framework reference:** See `.agent/references/cv-evaluation-framework.md` for the complete scoring rubric and criteria
- **Output formats:** Narrative analysis, score card, and optional HTML dashboard

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

### Step 4: Generate Results — Narrative

Present a human-readable analysis:

```
# CV Evaluation Results

## Overall Assessment
[2-3 sentence overview of CV quality and key findings]

## Strengths
- ✅ [Strength 1 with specific evidence from CV]
- ✅ [Strength 2]
- ✅ [Strength 3]

## Areas for Improvement

### 🔴 High Priority
- [Issue description]
  → Suggestion: [Specific, actionable improvement with example]

### 🟡 Medium Priority
- [Issue description]
  → Suggestion: [Improvement recommendation]

### 🟢 Low Priority
- [Minor issue]
  → Suggestion: [Optional enhancement]

## Section-by-Section Analysis

### [Section Name]
[Detailed analysis of this section with specific line references]
- What works: [specifics]
- What to improve: [specifics with rewrite examples]
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

---

### Step 5: Generate Results — Score Card

Present the scored summary:

```
## Score Card

| Category                | Score | Weight | Weighted |
|-------------------------|-------|--------|----------|
| Content Quality         | X/10  | 25%    | X.XX     |
| Structure & Formatting  | X/10  | 15%    | X.XX     |
| Impact & Metrics        | X/10  | 20%    | X.XX     |
| ATS Compatibility       | X/10  | 15%    | X.XX     |
| Relevance               | X/10  | 15%    | X.XX     |
| Professional Presentation | X/10 | 10%   | X.XX     |
|-------------------------|-------|--------|----------|
| **Composite Score**     |       |        | **X.XX** |

Rating: [⭐ Exceptional / ✅ Strong / ⚠️ Adequate / 🔶 Needs Work / 🔴 Major Revision]
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
1. **Header** — "CV Evaluation Report" with date and target info
2. **Composite Score** — Large, prominent score display with rating badge
3. **Category Breakdown** — Visual score bars for each category with color coding
4. **Strengths & Improvements** — Expandable cards with details
5. **Section Analysis** — Tabbed or accordion view per CV section
6. **Job Match** (if applicable) — Keyword match visualization, gap table
7. **Personal Touch Section** (at bottom):
   - A motivational quote (rotate from a curated list, or select contextually)
   - Generated by: [AI Agent Model Name]
   - "Brought to you by [doctor500](https://github.com/doctor500) • [doctor500/cv](https://github.com/doctor500/cv)"

**Implementation notes:**
- Single self-contained HTML file (inline CSS + JS, no external dependencies)
- Responsive design (mobile + desktop)
- Print-friendly view option
- Dark/light mode toggle optional

**File management note:**
- Dashboard is generated at `docs/evaluation/cv-evaluation.html`
- This file is for local/personal review — add `docs/evaluation/` to `.gitignore` if not tracking evaluations in git
- Alternatively, commit to track evaluation history over time

After generation:
```
✅ Dashboard generated at docs/evaluation/cv-evaluation.html

You can view it by opening the file in your browser.
Optionally, you can customize the dashboard theme (colors, fonts, layout)
after generation by editing the CSS variables at the top of the file.

Would you like to:
1. Preview in browser
2. Customize the theme
3. Continue
```

---

### Step 7: Iteration

After presenting results:
```
What would you like to do next?
1. Re-evaluate after making changes to index.md
2. Deep-dive into a specific section
3. Get rewrite suggestions for specific bullets
4. Evaluate against a (different) job posting
5. Done
```

If option 1: Return to Step 2 with updated content.
If option 2-4: Perform additional targeted analysis.

---

## Example Session

```
🤖: What would you like to evaluate?
    1. Entire CV
    2. Specific section(s) only

👤: 1

🤖: What kind of improvement are you looking for?
    1. General review
    2. Targeted for a specific job
    3. Career pivot
    4. ATS optimization
    5. Specific concern

👤: 2

🤖: Please provide the job posting URL or paste the description:

👤: https://company.com/jobs/senior-devops-engineer

🤖: Evaluating your CV against: Senior DevOps Engineer at Company...

    # CV Evaluation Results

    ## Overall Assessment
    Your CV presents a strong technical profile with excellent
    quantified achievements. The infrastructure cost reduction
    metrics are particularly impactful. However, some recent
    role descriptions could be strengthened with more specific
    outcomes.

    ## Score Card
    | Category                 | Score | Weight | Weighted |
    |--------------------------|-------|--------|----------|
    | Content Quality          | 8/10  | 25%    | 2.00     |
    | Structure & Formatting   | 7/10  | 15%    | 1.05     |
    | Impact & Metrics         | 8/10  | 20%    | 1.60     |
    | ATS Compatibility        | 7/10  | 15%    | 1.05     |
    | Relevance                | 9/10  | 15%    | 1.35     |
    | Professional Presentation| 8/10  | 10%    | 0.80     |
    | **Composite Score**      |       |        | **8.05** |

    Rating: ✅ Strong

    [... detailed analysis follows ...]

🤖: Would you like to generate an interactive web dashboard?
    1. Yes
    2. No

👤: 1

🤖: ✅ Dashboard generated at docs/evaluation/cv-evaluation.html
    
    You can customize the theme by editing CSS variables in the file.
    
    What would you like to do next?
```

---

## Error Handling

- **Job URL unreachable:** Fallback chain (curl → browser → paste)
- **Section not found in index.md:** Notify user, list available sections
- **Empty CV:** Suggest using `/build-cv-wizard` workflow first
- **Dashboard generation fails:** Fall back to markdown-only output
- **Previous dashboard exists:** Always ask before overwriting

---

_This workflow evaluates content in `index.md`._
