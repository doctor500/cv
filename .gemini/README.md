# Gemini CLI Configuration

**Purpose:** Local configuration for Gemini CLI integration with CV project

**Location:** This directory is local-only and gitignored (not in repository)

---

## Files

### config.yml
Registers workflows from `.agent/workflows/` as custom Gemini CLI commands.

**Available commands:**
- `/add-cv-section` → `.agent/workflows/add-cv-section.md`
- `/build-cv-wizard` → `.agent/workflows/build-cv-wizard.md`
- `/evaluate-cv` → `.agent/workflows/evaluate-cv.md`
- `/generate-template` → `.agent/workflows/generate-template.md`
- `/git-branch-pr` → `.agent/workflows/git-branch-pr.md`

---

## Project Documentation

For project documentation, see the `.agent/` directory (committed to repository):
- `.agent/PROJECT_CONTEXT.md` - Comprehensive technical docs
- `.agent/QUICK_REFERENCE.md` - Quick command reference
- `.agent/workflows/` - Workflow implementations

---

## Notes

- This directory is specific to Gemini CLI
- Files here are not version controlled
- All project documentation lives in `.agent/` (in repository)
- Other AI assistants may use different local configuration
