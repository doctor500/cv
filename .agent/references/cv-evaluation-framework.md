# CV Evaluation Framework

**Purpose:** Structured analysis framework for evaluating CV quality. Loaded by the `/evaluate-cv` workflow when performing scored evaluations.

---

## Evaluation Categories

The framework evaluates CVs across **6 categories**, each with a defined weight and detailed criteria.

### Universal Scoring Rubric

Apply this rubric to each category below using the category-specific checklist.

| Score | Level | Meaning |
|-------|-------|---------|
| 9-10 | Exceptional | Top-tier, no issues |
| 7-8 | Strong | Solid, minor refinements only |
| 4-6 | Adequate | Functional, notable gaps |
| 1-3 | Needs Work | Significant problems |

### 1. Content Quality (Weight: 25%)

Accuracy, completeness, and professionalism of written content.

**Criteria checklist:**
- [ ] Grammar and spelling accuracy
- [ ] Professional and consistent tone
- [ ] No first-person pronouns (I, me, my)
- [ ] All core sections present (Summary, Experience, Education, Skills)
- [ ] Descriptions are specific, not generic
- [ ] No fabricated or misleading content

---

### 2. Structure & Formatting (Weight: 15%)

Logical flow, visual hierarchy, and formatting consistency.

**Criteria checklist:**
- [ ] Consistent date formatting (inline code blocks)
- [ ] Consistent position/institution formatting (underline/bold)
- [ ] Proper heading hierarchy (h1 → h2 → h3)
- [ ] Consistent bullet point style
- [ ] Appropriate use of page breaks
- [ ] Reverse chronological order within sections
- [ ] Proper spacing between entries

---

### 3. Impact & Metrics (Weight: 20%)

Strength of achievements and quantification of results.

**Criteria checklist:**
- [ ] Achievements include numbers (%, $, time, scale)
- [ ] Action verbs used consistently
- [ ] Results follow the pattern: Action + What + How + Result
- [ ] Metrics are bolded for emphasis
- [ ] Descriptions focus on outcomes, not just responsibilities

---

### 4. ATS Compatibility (Weight: 15%)

How well the CV will perform with Applicant Tracking Systems.

**Criteria checklist:**
- [ ] Standard section headings used
- [ ] Technical terms include both acronyms and full names
- [ ] No complex formatting that could confuse parsers
- [ ] Keywords from target industry/role are present
- [ ] Skills section is comprehensive and categorized

---

### 5. Relevance (Weight: 15%)

Alignment with career goals or target job.

**Criteria checklist:**
- [ ] Profile summary aligns with stated/intended career goals
- [ ] Skills section emphasizes relevant competencies
- [ ] Experience entries highlight relevant responsibilities
- [ ] Activities/certifications support professional direction
- [ ] Content tells a coherent career story

**Job-targeted evaluation addon** (when target job is provided):
- [ ] Keyword match percentage against job description
- [ ] Must-have requirements covered
- [ ] Preferred qualifications addressed
- [ ] Industry-specific terminology used
- [ ] Gap analysis: requirements not met

---

### 6. Professional Presentation (Weight: 10%)

Overall first impression, readability, and polish.

**Criteria checklist:**
- [ ] Appropriate CV length (1-2 pages for most roles)
- [ ] Contact information is complete and prominent
- [ ] Links are functional (LinkedIn, GitHub, Medium, etc.)
- [ ] White space is used effectively
- [ ] Information density is appropriate (not too sparse, not too crowded)

---

## Composite Score Calculation

```
Composite Score = (Content Quality × 0.25) +
                 (Structure & Formatting × 0.15) +
                 (Impact & Metrics × 0.20) +
                 (ATS Compatibility × 0.15) +
                 (Relevance × 0.15) +
                 (Professional Presentation × 0.10)
```

### Score Interpretation

| Composite | Rating | Summary |
|-----------|--------|---------|
| 9.0 - 10.0 | ⭐ Exceptional | Top-tier CV. Ready for competitive applications. |
| 7.0 - 8.9  | ✅ Strong | Solid CV. Minor refinements would enhance it. |
| 5.0 - 6.9  | ⚠️ Adequate | Functional but needs improvement in key areas. |
| 3.0 - 4.9  | 🔶 Needs Work | Significant gaps. Focused revision recommended. |
| 1.0 - 2.9  | 🔴 Major Revision | Fundamental restructuring required. |

---

## Job-Targeted Evaluation Addon

When the user provides a target job description, perform additional analysis:

### Keyword Match Analysis
1. Extract key terms from job description (skills, tools, methodologies)
2. Search for each term in the CV content
3. Calculate match percentage: `(matched keywords / total keywords) × 100`
4. Classify: **80%+** = strong match, **60-79%** = moderate, **<60%** = weak

### Requirement Gap Matrix

| Requirement | Status | CV Evidence | Recommendation |
|-------------|--------|-------------|----------------|
| [From JD]   | ✅ Met / ⚠️ Partial / ❌ Gap | [Where in CV] | [How to address] |

### Priority Recommendations
- **High**: Must-have requirements not addressed
- **Medium**: Preferred qualifications with partial evidence
- **Low**: Nice-to-have improvements

---

## Section-Specific Evaluation Criteria

### Profile Summary
- Does it clearly state the candidate's value proposition?
- Does it include years of experience and primary domain?
- Is it concise (3-5 sentences)?
- Does it mention key technical competencies?

### Technical Skills
- Are skills grouped by category?
- Are the most relevant skills listed first?
- Are both acronyms and full names included where appropriate?
- Is the list current (no outdated technologies without context)?

### Professional Experience
- Does each role have quantified achievements?
- Are action verbs used consistently?
- Is the most recent role the most detailed?
- Are responsibilities relevant to career direction?

### Education
- Is GPA included (if notable)?
- Is relevant coursework mentioned (for early career)?
- Are dates formatted consistently?

### Activities / Publications
- Are metrics included (views, reach)?
- Are links provided and functional?
- Do they support the professional narrative?

---

_This framework is loaded by the `/evaluate-cv` workflow when performing structured evaluations._
