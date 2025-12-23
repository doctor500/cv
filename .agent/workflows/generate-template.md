---
description: Generate new CV template (CSS files only)
---

# Generate CV Template Workflow

**Purpose:** Create new CSS templates for CV styling. This is the ONLY workflow that creates new files - all other workflows update `index.md`.

---

## Key Principle

**This workflow creates CSS template files only.** It does NOT create or modify CV content. All CV content lives in `index.md`.

---

## Workflow Steps

### Step 1: Template Name

Ask user:
```
Let's create a new CV template!

Template name (lowercase, no spaces, e.g., 'modern', 'elegant', 'minimal'):
```

Store as `$TEMPLATE_NAME`.

---

### Step 2: Design Input

Ask user:
```
How would you like to define the design?
1. Provide reference URL (website, design, inspiration)
2. Describe the design in detail
3. Both (URL + additional customization)
```

---

### Step 3: Collect Design Information

#### Option 1 - Reference URL:
```
Please provide the reference URL:
```

Fetch and analyze:
- Color scheme
- Typography
- Layout structure
- Spacing/margins

#### Option 2 - Description:
Ask detailed questions:
```
1. Color scheme (e.g., navy blue #2C3E50, gray accents):

2. Font style (serif/sans-serif/monospace):

3. Layout:
   a. Single column
   b. Two column (sidebar + content)
   c. Custom

4. Special features (icons, borders, etc.):
```

#### Option 3 - Both:
Execute Option 1, then collect customizations.

---

### Step 4: Generate CSS Files

Create two files:

#### `media/$TEMPLATE_NAME-screen.css`
For web display with:
- CSS reset
- Base typography
- Layout structure
- Color scheme
- Responsive adjustments
- Interactive elements (hover, links)

#### `media/$TEMPLATE_NAME-print.css`
For PDF generation with:
- Print-optimized typography
- Simplified layout
- Print-safe colors
- Page break controls
- Black & white fallbacks

---

### Step 5: Preview Template

Show summary:
```
Template: $TEMPLATE_NAME
━━━━━━━━━━━━━━━━━━━━━━━━━━━━

Screen CSS:
  - Layout: [description]
  - Colors: [palette]
  - Fonts: [typefaces]

Print CSS:
  - Optimized for: A4/Letter
  - Page breaks: [strategy]
  - Color mode: [setting]

Files to create:
  ✓ media/$TEMPLATE_NAME-screen.css
  ✓ media/$TEMPLATE_NAME-print.css

Proceed? (yes/no/edit)
```

---

### Step 6: Create Files

If confirmed:
1. Generate `media/$TEMPLATE_NAME-screen.css`
2. Generate `media/$TEMPLATE_NAME-print.css`
3. Confirm creation

---

### Step 7: Update Configuration

Ask user:
```
Set as active template?
1. Yes - update _config.yml now
2. No - I'll switch manually later
```

If yes, update `_config.yml`:
```yaml
markdown: kramdown
style: $TEMPLATE_NAME
```

---

### Step 8: Test Template

Ask user:
```
Would you like to test the template locally?
1. Yes, using Docker (recommended)
2. Yes, using Ruby directly
3. No
```

**Option 1: Docker**
```bash
# Restart container to load new template
docker-compose down
docker-compose up -d
```
Verify with:
```bash
curl http://localhost:4000
```

**Option 2: Ruby (Direct)**
```bash
# Start server
bundle exec jekyll serve
```
Verify with:
```bash
curl http://localhost:4000
```

Ask user:
```
Template is live!

Does it look good? (yes/no/adjust)
```

---

### Step 9: Refinement (if needed)

If "adjust":
```
What needs adjustment?
1. Colors
2. Typography
3. Layout/spacing
4. Print formatting
```

Update respective CSS, restart container (or wait for reload), preview again.

---

### Step 10: Optional PDF Test

```
Test PDF generation?
1. Yes - I'll generate via GitHub Actions
2. No - skip for now

If yes, go to:
GitHub > Actions > Build PDF > Run workflow (private build)
Download from artifacts to test print CSS.
```

---

### Step 11: Documentation (Optional)

Create `media/$TEMPLATE_NAME-README.md`:
```markdown
# $TEMPLATE_NAME Template

**Created:** [date]
**Inspired by:** [source]

## Features
- [List features]

## Color Palette
- Primary: [color]
- Accent: [color]

## Usage
In _config.yml:
\`\`\`yaml
style: $TEMPLATE_NAME
\`\`\`
```

---

### Step 12: Finalize Changes (Git Branch & PR)

Instead of committing directly, follow the Git Branch & PR Workflow:

1. **Check branch:** Ensure not on `main` or `page-release`.
2. **Create branch:** `feat/add-template-$TEMPLATE_NAME`.
3. **Commit:** Stage and commit CSS files and `_config.yml` (after user approval).
4. **PR:** Push and create/update Pull Request.

**Refer to:** `.agent/workflows/git-branch-pr.md` for detailed steps.

Ask user:
```
✅ Template created successfully!

Proceed with Git Branch & PR workflow? (yes/no)
```

If "yes", execute the steps from `git-branch-pr.md`.

---

## Example Session

```
🤖: Let's create a new CV template!
    
    Template name:

👤: modern-blue

🤖: How would you like to define the design?
    1. Reference URL
    2. Describe design
    3. Both

👤: 2

🤖: 1. Color scheme:

👤: Professional navy blue #1a365d with light gray #f7fafc

🤖: 2. Font style:

👤: Sans-serif, modern and clean

🤖: 3. Layout:
    a. Single column
    b. Two column
    c. Custom

👤: b

[... continues ...]

🤖: Template: modern-blue
    ━━━━━━━━━━━━━━━━━━━━━━━
    
    Screen CSS:
      - Layout: Two-column
      - Colors: Navy #1a365d, Gray #f7fafc
      - Fonts: Sans-serif
    
    Proceed? (yes/no)

👤: yes

🤖: ✅ Created media/modern-blue-screen.css
    ✅ Created media/modern-blue-print.css
    
    Set as active template?
    1. Yes
    2. No

👤: 1

🤖: ✅ Updated _config.yml
    🐳 Restarting container...
    
    Open http://localhost:4000
    
    Does it look good? (yes/no/adjust)

👤: yes

🤖: ✅ Template created successfully!
    
    Commit? (yes/no)

👤: yes

🤖: ✅ Committed: "feat: add modern-blue template"
```

---

## Design Best Practices

### Color Schemes
- **Professional:** Navy, dark gray, minimal accent
- **Creative:** Bold colors, gradients
- **Academic:** Classic black/gray
- **Tech:** Blue/purple palette

### Typography
- Headings: 1.5-3em with weight hierarchy
- Body: 14-16px, line-height 1.4-1.6em
- Code/Dates: Monospace, 80-90% size

### Layout
- Sidebar: 25-33% for labels
- Content: 65-75% for information
- Consistent margins: 1-2em

### Print Optimization
- Page size: A4 or Letter
- Safe margins: 1-2cm
- Test in grayscale
- Avoid breaking sections

---

## File Locations

Created files:
- `media/$TEMPLATE_NAME-screen.css` - Web display
- `media/$TEMPLATE_NAME-print.css` - PDF generation
- `media/$TEMPLATE_NAME-README.md` - Documentation (optional)

Updated files:
- `_config.yml` - Template selection (if activated)

**Important:** This workflow does NOT modify `index.md` - only creates template files.

---

## Error Handling

- URL fetch fails → Fall back to description
- CSS syntax errors → Validate before saving
- Container won't start → Check ports, conflicts
- Template name exists → Ask to overwrite or rename

---

_This workflow creates CSS templates only. For CV content updates, use `/add-cv-section`._
