---
description: Add new section to CV (updates index.md)
---

# Add CV Section Workflow

**Purpose:** Interactively add new sections to `index.md`. All CV data lives in this single file.

## Important Notes
- **Single source:** Only `index.md` contains CV content
- **LinkedIn URL:** Automatically extracted from `index.md` line 12 or user can provide manually
- **Direct editing:** This workflow modifies `index.md` directly

---

## Workflow Steps

### Step 1: Choose Section Type

Ask user:
```
What type of section would you like to add?
1. Professional Experience
2. Education
3. Activities (Article/Publication/Certification)
4. Project
5. Custom Section
```

### Step 2: Choose Data Source

Ask user:
```
How do you want to add data?
1. Manual entry (I'll provide details)
2. Fetch from LinkedIn
```

If option 2, extract LinkedIn URL from `index.md` line 12 or ask:
```
LinkedIn URL not found in index.md or want to use different profile?
1. Use profile from index.md: [extracted-url]
2. Enter different LinkedIn URL
```

---

### Step 3: Collect Data

#### For Professional Experience (Manual):
```
Start date (e.g., Jan 2024):
End date (e.g., Dec 2025 or Now):
Position:
Company:
Description:
Achievements (one per line, with metrics):
```

#### For Education (Manual):
```
Start year (e.g., 2016):
End year (e.g., 2020):
Institution:
Location:
Degree & major:
GPA (optional):
```

#### For Activities (Manual):
```
Date (e.g., Apr 2024):
Type (article/certification):
Title:
URL:
Additional info (metrics, issuer, etc.):
```

#### For Project (Manual):
```
Date range (e.g., May 2024 - Present):
Project name:
Organization:
Description:
```

#### For LinkedIn Fetch:
1. Launch browser to LinkedIn profile URL
2. Navigate to relevant section
3. Extract data
4. Present to user for confirmation/editing

---

### Step 4: Format Data

Use the CV formatting templates from `QUICK_REFERENCE.md` (CV Formatting section).

Key rules:
- Dates in backticks (`` `Jan 2024 - Now` ``)
- Positions/institutions in `__underline__` format
- Metrics in **bold**
- URLs as proper markdown links

**Reference:** See `.agent/references/cv-construction-guide.md` for achievement quantification patterns.

---

### Step 5: Show Preview

Display formatted content:
```
Here's how it will appear in index.md:

[formatted content]

Does this look correct? (yes/no/edit)
```

If "edit", allow modifications. If "no", restart or cancel.

---

### Step 6: Determine Insertion Point

Ask user:
```
Where in index.md should this be added?
1. Top of [Section Name] section (most recent)
2. Bottom of [Section Name] section (oldest)  
3. After specific entry (I'll show you options)
4. Custom line number
```

If option 3, show existing entries in that section.

---

### Step 7: Update index.md

1. Read `index.md`
2. Locate target section (e.g., `## Professional Experience`)
3. Insert formatted content at chosen position
4. Maintain proper spacing (blank lines between entries)
5. Save updated `index.md`

**Example:**
```markdown
## Professional Experience

[new entry inserted here if "top"]

`Jan 2024 - Now`
__Infrastructure Engineer__, GovTech Procurement
...

[new entry inserted here if "bottom"]

## Education
```

---

### Step 8: Optional - Preview Locally

Follow the render testing procedure from `build-cv-wizard.md` Step 8, or use commands in `QUICK_REFERENCE.md` (Local Development section).

---

### Step 9: Finalize Changes (Git Branch & PR)

Follow `.agent/workflows/git-branch-pr.md` to commit and create a PR.
Suggested branch name: `feat/[section-name]` or `fix/[update-type]`

---

## Example Session

```
🤖: What type of section would you like to add?
    1. Professional Experience
    2. Education
    3. Activities
    4. Project
    5. Custom Section

👤: 3

🤖: How do you want to add data?
    1. Manual entry
    2. Fetch from LinkedIn

👤: 1

🤖: Activities can be articles, certifications, etc.
    
    Date (e.g., Apr 2024):

👤: Jan 2026

🤖: Type (article/certification/other):

👤: article

🤖: Article title:

👤: Building Modern CI/CD Pipelines

🤖: Article URL:

👤: https://medium.com/@yourname/modern-cicd

🤖: Additional info (views, publication, etc.):

👤: Reach 5000+ viewers under DevOps Today publication

🤖: Here's how it will appear in index.md:

    `Jan 2026`
    [**Building Modern CI/CD Pipelines**](https://medium.com/@yourname/modern-cicd)
    
    Reach 5000+ viewers under DevOps Today publication
    
    Does this look correct? (yes/no/edit)

👤: yes

🤖: Where in index.md should this be added?
    1. Top of Activities section (most recent)
    2. Bottom of Activities section (oldest)

👤: 1

🤖: ✅ Successfully updated index.md!
    
    Would you like to:
    1. Preview locally
    2. Commit changes
    3. Done

👤: 2

🤖: Committing changes...
    ✅ Committed: "feat: add article - Building Modern CI/CD Pipelines"
```

---

## LinkedIn Data Extraction

### Get LinkedIn URL
1. Read `index.md` line 12
2. Extract href from LinkedIn link in webaddress section
3. Or ask user to provide URL

### Fetch Process
Use the same URL fallback chain from `build-cv-wizard.md` Step 1:
curl → retry with User-Agent → Browser tool → ask user to paste content

---

## Error Handling

- **Section not found:** Offer to create new section
- **Invalid format:** Prompt for correction
- **LinkedIn fetch fails:** Fall back to manual entry
- **File locked:** Retry after delay
- **Merge conflicts:** Show diff and ask user to resolve

---

## Validation Checklist

Before saving to `index.md`:
- [ ] Dates are in correct format (backticks)
- [ ] URLs are valid (see automated check below)
- [ ] Markdown syntax is correct
- [ ] Spacing is consistent (blank lines between entries)
- [ ] Metrics are bolded
- [ ] No trailing whitespace
- [ ] Section exists in index.md

**Automated URL check:**
// turbo
```bash
# Verify URLs in index.md respond
grep -oP 'https?://[^\s\)\]]+' index.md | while read url; do
  code=$(curl -sL -o /dev/null -w "%{http_code}" --max-time 5 "$url")
  [ "$code" -ge 400 ] && echo "⚠️ $code $url" || echo "✅ $code $url"
done
```

---

_This workflow only updates index.md - the single source of truth for CV content._
