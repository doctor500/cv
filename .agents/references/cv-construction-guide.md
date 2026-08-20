# CV Construction Guide

**Purpose:** Reference document for AI agents building or improving CV content. Loaded only when needed by the `/build-cv-wizard` workflow.

---

## CV Best Practices

### Do
- Quantify achievements with specific metrics (%, $, time saved, scale)
- Match the formatting patterns in `QUICK_REFERENCE.md` exactly
- Use inline code backticks for dates, `__underline__` for positions
- Bold all metrics and numbers
- Use reverse chronological order (most recent first)

### Don't
- Commit sensitive data (phone, address) — use GitHub Secrets for private builds
- Create separate content files — all content goes in `index.md` only
- Exceed 2 pages unless for a very senior role

---

## ATS (Applicant Tracking System) Optimization

ATS systems scan resumes before a human ever sees them. Optimize for both:

1. **Use standard section headings** — Professional Experience, Education, Skills, etc.
2. **Incorporate exact keywords** from the job description naturally
3. **Avoid complex formatting** — tables, graphics, headers/footers, or multi-column layouts in the raw content
4. **Use standard fonts** and bullet points
5. **Include both acronyms and full terms** — e.g., "Google Kubernetes Engine (GKE)"
6. **Match job title terminology** where truthful

---

## Achievement Quantification Patterns

Strong achievements follow this formula:

```
[Action Verb] + [What You Did] + [How/With What] + [Measurable Result]
```

### Examples
- ❌ "Managed cloud infrastructure"
- ✅ "Reduced production infrastructure costs by **90%** daily, saving over **$150,000** monthly through GKE migration"

- ❌ "Improved deployment process"
- ✅ "Decreased pipeline initialization build time by **85%** for mono-repo backend services"

- ❌ "Automated reporting"
- ✅ "Automated batch registration process, reducing completion time from **one week to under one hour**"

---

## Action Verbs

Use strong, varied action verbs (Led, Engineered, Optimized, Pioneered, etc.) appropriate to the achievement type.

---

## Career Context

Adapt structure for the user's career stage (new grad → lead with education; career changer → emphasize transferable skills; senior → focus on strategic impact).

---

## Section-Specific Writing Tips

### Profile Summary
- 3-5 sentences maximum
- Lead with years of experience and primary domain
- Include 3-4 top skills aligned to career goals
- Mention industry focus and unique value proposition
- Write in third person without pronouns

### Professional Experience
- Most recent role gets the most detail (4-6 bullets)
- Older roles can be condensed (2-3 bullets)
- Every bullet should ideally contain a measurable result
- Group related achievements logically
- Use consistent tense (past for previous roles, present for current)

### Technical Skills
- Group by category (Cloud, Containers, CI/CD, Languages, etc.)
- List most relevant/proficient skills first
- Only include skills you can discuss confidently in an interview

### Education
- Keep concise unless recent graduate
- Include GPA only if notable (3.5+)
- Relevant coursework only for early career

### Activities / Publications
- Include metrics (views, reach, citations)
- Link directly to the content
- Highlight the publication or platform

---

## Privacy & Data Handling

- Never commit sensitive personal data (phone, address) to the repository
- Phone number should only be included via GitHub Secrets in private PDF builds
- Warn users before processing data from external sources
- All extracted data goes into `index.md` only — no intermediate files

---

_This reference document is loaded by the `/build-cv-wizard` workflow when detailed CV construction guidance is needed._
