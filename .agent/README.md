# CV Project - Agent Directory

This directory contains AI agent configurations, workflows, and project context for the CV project.

## 📁 Directory Structure

```
.agent/
├── PROJECT_CONTEXT.md           # Comprehensive project documentation
├── QUICK_REFERENCE.md           # Quick command reference  
├── CHANGELOG.md                 # Project changes tracking
├── workflows/
│   ├── add-cv-section.md        # Workflow: Update index.md with new sections
│   └── generate-template.md     # Workflow: Generate CSS templates
└── README.md                    # This file
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

## 🚀 Quick Start

### For AI Agents
1. Read `PROJECT_CONTEXT.md` for complete understanding
2. Use `QUICK_REFERENCE.md` for fast lookups
3. Execute workflows:
   - `/add-cv-section` - Update `index.md`
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
- **Last Updated:** 2025-12-20
- **Status:** Active

---

_Keep this directory updated as the project evolves!_
