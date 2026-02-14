# CV Project - Agent Directory

This directory contains AI agent configurations, workflows, and project context for the CV project.

## 📁 Directory Structure

```
.agent/
├── references/
│   ├── cv-construction-guide.md     # CV writing best practices & ATS tips
│   ├── cv-evaluation-framework.md   # 6-category scoring framework
│   └── benchmark-testing-framework.md # Workflow benchmarking standard
├── workflows/
│   ├── add-cv-section.md            # Workflow: Update index.md with new sections
│   ├── build-cv-wizard.md           # Workflow: Build CV from data sources
│   ├── evaluate-cv-quick.md         # Workflow: Quick CV evaluation (~2-4K tokens)
│   ├── evaluate-cv-deepdive.md      # Workflow: Deep dive evaluation (~8-15K tokens)
│   ├── generate-template.md         # Workflow: Generate CSS templates
│   └── git-branch-pr.md             # Workflow: Branch & PR management
├── PROJECT_CONTEXT.md               # Comprehensive project documentation
├── QUICK_REFERENCE.md               # Quick command reference
├── CHANGELOG.md                     # Project changes tracking
└── README.md                        # This file
```

## 🎯 Core Principle

**Single Source of Truth:** All CV content lives in `index.md`. Workflows update this file directly - no separate markdown files are created for CV content (except CSS template documentation).

---

## 📋 Files Overview

### PROJECT_CONTEXT.md
**Comprehensive project documentation**

Contains complete information about:
- Project architecture and structure
- Local development setup
- Deployment and CI/CD pipeline
- Templating system
- CV structure and formatting
- LinkedIn integration
- Technology stack
- Common workflows

**Use when:** First-time setup, understanding architecture, troubleshooting

---

### QUICK_REFERENCE.md
**Fast lookup for essential commands**

Compact reference with only the most important:
- Docker commands
- Key file locations
- CV formatting patterns
- Git deployment
- AI command shortcuts

**Use when:** Daily development, quick command lookup

---

### CHANGELOG.md
**Project history and updates**

Tracks:
- Project changes
- CV content updates
- New features
- Bug fixes

**Use when:** Tracking project evolution, release notes

---

### workflows/add-cv-section.md
**Workflow: Add content to index.md**

**Key Principle:** Updates only `index.md` - the single source of truth

**Features:**
- Manual data entry or LinkedIn fetch
- LinkedIn URL auto-extracted from `index.md` line 12
- Updates `index.md` directly (no new files)
- Automatic formatting
- Content validation

**Supported:** Professional Experience, Education, Activities, Projects

**Execute:** `/add-cv-section`

---

### workflows/generate-template.md
**Workflow: Create new CSS templates**

**Key Principle:** ONLY workflow that creates new files (CSS templates)

**Features:**
- Creates `media/[name]-screen.css` and `media/[name]-print.css`
- URL reference or description-based
- Template testing and validation
- Documentation generation

**Execute:** `/generate-template`

---

### workflows/build-cv-wizard.md
**Workflow: Build CV from user-provided data sources**

**Key Principle:** Curates multiple data sources into `index.md` content

**Features:**
- Multi-source data gathering (URLs, files, guided interview)
- 3-tier URL fallback (curl → browser → manual)
- Section ordering with default structure from `index.md`
- Syntax validation and render testing loop
- References `references/cv-construction-guide.md` for best practices

**Execute:** `/build-cv-wizard`

---

### workflows/evaluate-cv-quick.md
**Workflow: Quick CV evaluation (~2K-4K output tokens)**

**Key Principle:** Fast, lightweight scored analysis for iteration cycles

**Features:**
- 6-category scoring (Content, Structure, Impact, ATS, Relevance, Presentation)
- Dual output: narrative analysis + score card
- Optional job-targeted evaluation with keyword matching
- Optional interactive HTML dashboard (neo-brutalism theme)
- References `references/cv-evaluation-framework.md` for rubric

**Execute:** `/evaluate-cv-quick`

---

### workflows/evaluate-cv-deepdive.md
**Workflow: Comprehensive CV evaluation (~8K-15K output tokens)**

**Key Principle:** Evidence-based evaluation with 10 mandatory insight quality standards

**Everything in Quick mode, plus:**
- Positioning Diagnosis, Career Arc Analysis
- Evidence-based citations (every claim cites specific CV content)
- Actionable rewrite examples (every suggestion includes replacement text)
- Score impact estimates, contradiction detection
- Top 3 high-impact actions with combined score projection
- ≥80% specificity guarantee

**Execute:** `/evaluate-cv-deepdive`

---

### references/cv-construction-guide.md
**Reference: CV writing best practices**

Detailed guide covering ATS optimization, action verbs, career-type considerations, and section-specific writing tips. Loaded by `/build-cv-wizard` when needed.

---

### references/cv-evaluation-framework.md
**Reference: CV scoring framework**

Complete evaluation rubric with 6 weighted categories, composite scoring, job-targeted addon, and section-specific criteria. Loaded by `/evaluate-cv-quick` and `/evaluate-cv-deepdive` workflows.

---

### references/benchmark-testing-framework.md
**Reference: Workflow benchmarking standard**

Standardized procedure for comparing workflow performance. Provides methodology, metrics templates, Quality Index formula, execution protocol, and decision criteria for adopting workflow changes.

---

## 🚀 Quick Start

### For AI Agents
1. Read `PROJECT_CONTEXT.md` for complete understanding
2. Use `QUICK_REFERENCE.md` for fast lookups
3. Execute workflows:
   - `/add-cv-section` - Update `index.md`
   - `/build-cv-wizard` - Build CV from data sources
   - `/evaluate-cv-quick` - Quick CV evaluation
   - `/evaluate-cv-deepdive` - Deep dive evaluation
   - `/generate-template` - Create CSS templates

### For Developers
1. Review `PROJECT_CONTEXT.md`
2. Check `QUICK_REFERENCE.md` for commands
3. Explore workflows for automation

---

## 🔄 Workflow Execution

### AI Agent Execution
1. Read workflow markdown file
2. Follow step-by-step instructions
3. Use interactive prompts with user
4. Commands marked `// turbo` auto-execute
5. Validate results at each step

### Important Notes
- All CV content workflows update `index.md` only
- Only template workflow creates new files
- LinkedIn URL extracted from `index.md` or user input
- Always validate before committing
- **DRY pattern:** `build-cv-wizard` is the canonical source for render testing and URL fallback; other workflows reference it rather than duplicating

---

## 📝 Creating New Workflows

When creating workflows:

1. Create in `.agent/workflows/[name].md`
2. Add YAML frontmatter:
   ```yaml
   ---
   description: Brief description
   ---
   ```
3. **Remember:** CV content workflows must update `index.md` only
4. Mark safe commands with `// turbo`
5. Include examples and error handling
6. Update this README

---

## 🔄 Maintenance

### Update PROJECT_CONTEXT.md when:
- Architecture changes
- New features added
- Deployment process changes
- Dependencies updated

### Update Workflows when:
- Process improvements identified
- New features available
- Bugs fixed

### Update CHANGELOG.md when:
- Making significant changes
- Adding new features
- Fixing bugs
- Updating CV content

---

## 💡 Tips for AI Agents

1. **Read `PROJECT_CONTEXT.md` first** - understand the full picture
2. **Remember single source:** All CV data in `index.md` only
3. **Use workflows** for consistency
4. **Extract LinkedIn URL** from `index.md` line 12
5. **Validate changes** before committing
6. **Update documentation** after changes

---

## 📞 Support

1. Check `PROJECT_CONTEXT.md` first
2. Review relevant workflow
3. Consult `QUICK_REFERENCE.md`
4. Check main `README.md`

---

## 🔖 Version

- **Created:** 2025-12-20
- **Last Updated:** 2026-02-14
- **Status:** Active

---

_Keep this directory updated as the project evolves!_
