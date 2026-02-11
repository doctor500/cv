# CV Project - Quick Reference

⚡ **Fast lookup for essential commands and patterns**

---

## 🚀 Local Development

**Docker (recommended):**
```bash
# Start server
docker-compose up -d

# Verify
curl http://localhost:4000

# Stop server
docker-compose down
```

**Ruby server (alternative):**
```bash
bundle install          # First time only
bundle exec jekyll serve

# Verify (new terminal)
curl http://localhost:4000
```

---

## 📁 Key Files

| File | Purpose |
|------|---------|
| `/index.md` | **Main CV content** (single source of truth) |
| `/_config.yml` | Template selection |
| `/media/[template]-*.css` | Templates (screen/print) |

---

## 🎨 Switch Template

Edit `_config.yml`:
```yaml
style: kjhealy  # or davewhipp
```

Then restart: `docker-compose restart`

---

## 📝 CV Formatting

### Professional Experience
```markdown
`Jan 2024 - Now`
__Position__, Company

Description text.

- 1) Achievement with **metrics**
- 2) Another achievement
```

### Education
```markdown
`2016 - 2020`
__Institution, Location__

Degree details, GPA: 3.8 of 4.0
```

### Activities/Articles
```markdown
`Mon YYYY`
[**Article Title**](url)

Metrics information
```

### Page Break
```markdown
<div style="page-break-after: always;"></div>
```

---

## 🔄 Git Workflow

> **⚠️ Never commit directly to `main` or `page-release`!**

```bash
# Create feature branch
git checkout -b feat/my-feature

# Make changes, then push
git push origin feat/my-feature

# Create PR (or use /git-branch-pr workflow)
# Merge after approval
```

---

## 📄 Generate PDF

1. GitHub > Actions > Build PDF
2. Run workflow
3. Enter release notes
4. Download from Releases

---

## 🤖 AI Commands

- `/add-cv-section` - Add content to index.md
- `/build-cv-wizard` - Build CV from data sources
- `/evaluate-cv` - Evaluate CV with scoring & dashboard
- `/generate-template` - Create new CSS template
- `/git-branch-pr` - Branch & PR workflow

---

## 🔗 LinkedIn Profile

Extracted from `index.md` webaddress section (auto-detected by workflows)

---

_For details see PROJECT_CONTEXT.md_
