---
description: Build CV from user-provided data sources (links, files, manual input)
---

# CV Build Wizard Workflow

**Purpose:** Help the user construct a complete CV by curating resources they provide (links, files, or manual input). All generated content is written to `index.md`.

## Important Notes
- **Single source:** Only `index.md` contains CV content
- **Reference guide:** See `.agents/references/cv-construction-guide.md` for detailed CV writing best practices, ATS optimization, and section-specific tips
- **Direct editing:** This workflow generates/overwrites `index.md` content directly

---

## Workflow Steps

### Step 1: Gather Data Sources

Ask user:
```
Let's build your CV! How would you like to provide your information?

1. Provide a list of links/files (I'll extract the data)
2. Guided interview (I'll ask questions one by one)
3. Mix of both
```

#### Option 1 — Links/Files:
Ask user to provide their data sources:
```
Please list your CV data sources (one per line).
These can be URLs (LinkedIn, portfolio, personal site) or local file paths.

Example:
- https://linkedin.com/in/your-profile
- https://your-portfolio.com
- /path/to/resume.pdf
```

**Process each source with this fallback chain:**

```
For each link/file provided:
├── Is it a local file?
│   ├── Yes → Read and parse the file
│   └── No → Is it a URL?
│       ├── Attempt 1: Fetch with curl
│       │   ├── Success → Parse content
│       │   └── Fail → Attempt 2: Retry curl with browser-like User-Agent
│       │       │   Header: -H "User-Agent: Mozilla/5.0 (compatible)"
│       │       ├── Success → Parse content
│       │       └── Fail → Attempt 3: Use Browser tool (if available)
│       │           ├── Success → Parse content
│       │           └── Fail → Notify user:
│       │               "Could not access [URL] after multiple attempts.
│       │                Please provide:
│       │                1. An alternative URL
│       │                2. A file with the same information
│       │                3. Skip this source"
│       └── No → Invalid source, ask for correction
```

#### Option 2 — Guided Interview:
Ask questions one by one for each section. See Step 3 for section-specific questions.

#### Option 3 — Mix:
Process provided sources first, then fill gaps with guided questions.

---

### Step 2: Extract & Organize Data

After collecting all sources, organize extracted data into these categories:

1. **Personal Information** — Name, title, location, contact, links
2. **Profile Summary** — Professional overview
3. **Technical Skills** — Technologies and tools
4. **Professional Experience** — Roles, companies, dates, achievements
5. **Education** — Degrees, institutions, dates
6. **Activities** — Articles, certifications, projects
7. **Language** — Language proficiencies

Present the extracted data summary to user:
```
Here's what I extracted from your sources:

✅ Personal Info: [name, title, etc.]
✅ Experience: [X roles found]
⚠️ Skills: [partial — needs more detail]
❌ Education: [not found — will ask you]

Shall I proceed, or do you want to correct anything?
```

---

### Step 3: Fill Gaps (Guided Interview)

For any sections not covered by provided sources, ask questions one by one:

#### Personal Information:
```
Full name:
Professional title/headline:
Location (city, country):
Email:
GitHub URL:
LinkedIn URL:
Other links (Medium, portfolio, etc.):
```

#### Profile Summary:
```
Describe your professional focus in 2-3 sentences:
How many years of experience do you have?
What are your top 3-4 skills?
Any specific industry focus?
```

#### Technical Skills:
```
List your technical skills grouped by category.
Example:
- Cloud: AWS, GCP
- Containers: Docker, Kubernetes
- CI/CD: Jenkins, GitHub Actions
```

#### Professional Experience (per role):
```
Start date (e.g., Jan 2024):
End date (e.g., Dec 2025 or Present):
Position/title:
Company name:
Location/context:
Brief description (1-2 sentences):
Key achievements (one per line, include numbers/metrics):
```

#### Education (per entry):
```
Start year:
End year:
Institution name:
Location:
Degree and major:
GPA (optional):
```

#### Activities:
```
Do you have any of the following?
1. Published articles/blog posts
2. Certifications
3. Notable projects
4. Speaking engagements
```
For each, collect: date, title, URL, additional metrics.

#### Language:
```
List languages and proficiency levels:
Example:
- English - Business level
- Japanese - Native
```

---

### Step 4: Section Ordering

Present the default CV structure (from `index.md` on the `main` branch) and ask user to confirm or reorder:

```
Here's the default section order:

1. Header (Name, Title, Contact)
2. Profile Summary
3. Technical Skills
4. Professional Experience
5. Education
6. Activities
   - Medium Articles
   - Training & Certifications
   - Latest Professional Projects
7. Language

Would you like to:
1. Keep this order (recommended)
2. Customize the order
```

If option 2, allow user to reorder by number.

---

### Step 5: Generate CV Content

Build the complete `index.md` using the project's markdown syntax patterns.

**Format patterns:** Use the templates documented in `QUICK_REFERENCE.md` (CV Formatting section).
Key rules: dates in backticks, positions in `__underline__`, metrics in **bold**, page breaks with `<div style="page-break-after: always;"></div>`.

**Reference:** See `.agents/references/cv-construction-guide.md` for achievement quantification patterns and ATS optimization guidelines.

---

### Step 6: Show Preview

Display the generated content:
```
Here's the complete CV content for index.md:

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
[full generated content]
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

Review checklist:
✓ All sections present
✓ Dates formatted correctly
✓ Metrics bolded
✓ URLs valid
✓ Markdown syntax correct

Does this look correct? (yes / no / edit specific section)
```

If "edit", allow section-by-section modifications.

---

### Step 7: Syntax Validation

Before writing to `index.md`, validate:
- [ ] YAML front matter is valid
- [ ] All dates use inline code format (`backticks`)
- [ ] Positions/institutions use `__underline__` format
- [ ] Metrics are **bolded**
- [ ] URLs are properly formatted as markdown links
- [ ] HTML elements (`<div>`, `<a>`, `<text>`) are properly closed
- [ ] Page breaks use correct `<div style="page-break-after: always;"></div>` syntax
- [ ] Consistent spacing: blank line between entries
- [ ] No trailing whitespace

**Automated YAML front matter check:**
// turbo
```bash
# Validate YAML front matter exists and is well-formed
head -10 index.md | grep -q "^---" && echo "✅ YAML front matter found" || echo "⚠️ Missing YAML front matter"
```

**Automated HTML check:**
// turbo
```bash
# Count opening vs closing tags for common elements
for tag in div a text u; do
  open=$(grep -co "<${tag}[ >]" index.md)
  close=$(grep -co "</${tag}>" index.md)
  [ "$open" != "$close" ] && echo "⚠️ <${tag}>: $open open, $close close" || echo "✅ <${tag}>: balanced ($open)"
done
```

**Automated page break check:**
// turbo
```bash
# Verify page break positions
grep -n "page-break" index.md | while read line; do
  echo "📄 Page break at: $line"
done
echo "Active: $(grep -c 'page-break-after' index.md), Commented: $(grep -c '<!-- .*page-break' index.md || echo 0)"
```

If any issues found, fix automatically and notify user of changes made.

---

### Step 8: Render Testing

**First, detect available tools:**
// turbo
```bash
echo "Docker: $(which docker > /dev/null 2>&1 && echo '✅ Available' || echo '❌ Not found')"
echo "Ruby: $(which ruby > /dev/null 2>&1 && echo '✅ Available' || echo '❌ Not found')"
```

**Present only available options:**
```
Content validated! Would you like to test the rendering locally?
1. Yes, using Docker (recommended) [if Docker detected]
2. Yes, using Ruby directly [if Ruby detected]
3. No, skip testing
```

If neither Docker nor Ruby is detected:
```
⚠️ No local rendering tools found (Docker/Ruby).
Skip to finalization, or install Docker first?
1. Skip rendering test
2. I'll install Docker first (pause here)
```

**Test locally** using the commands in `QUICK_REFERENCE.md` (Local Development section).

Verify rendering:
// turbo
```bash
curl -s -o /dev/null -w "%{http_code}" http://localhost:4000
```
Expected: HTTP 200

**If rendering fails:** Check Jekyll output for invalid YAML, unclosed HTML tags, or special characters. Fix and re-validate (return to Step 7).

**If rendering succeeds:**
```
✅ CV renders successfully at http://localhost:4000

Would you like to:
1. Open in browser for visual inspection
2. Continue to finalization
3. Make adjustments first
```

---

### Step 9: Iteration

If user wants changes:
```
What would you like to adjust?
1. Rewrite a specific section
2. Reorder sections
3. Add missing information
4. Remove a section
5. Adjust formatting/style
```

After changes, re-run Steps 7-8 (validate + render) before finalizing.

---

### Step 10: Finalize (Git Branch & PR)

Follow `.agents/workflows/git-branch-pr.md` to commit and create a PR.
Suggested branch name: `feat/build-cv` or `feat/cv-rebuild`

---

## Example Session

```
🤖: Let's build your CV! How would you like to provide your information?
    1. Provide a list of links/files
    2. Guided interview
    3. Mix of both

👤: 1

🤖: Please list your CV data sources (one per line):

👤: - https://linkedin.com/in/john-doe
    - /home/user/old-resume.pdf

🤖: Processing sources...
    ✅ LinkedIn: Successfully extracted profile data
    ✅ resume.pdf: Successfully parsed

    Here's what I extracted:
    ✅ Personal Info: John Doe, Software Engineer, San Francisco
    ✅ Experience: 3 roles found
    ✅ Education: 1 entry found
    ⚠️ Skills: partial list
    ❌ Activities: not found

    Shall I proceed, or correct anything?

👤: Proceed

🤖: Let me fill in the gaps.
    
    List your technical skills grouped by category:

👤: - Languages: Python, Go, JavaScript
    - Cloud: AWS, GCP
    - Tools: Docker, Kubernetes, Terraform

🤖: Do you have any activities (articles, certifications, projects)?

👤: Yes, I have an AWS certification from Jan 2024

[... continues through all steps ...]

🤖: ✅ CV renders successfully!
    
    Proceed with Git Branch & PR workflow? (yes/no)
```

---

## Error Handling

- **URL unreachable:** 3-tier fallback (curl → retry → browser → ask user)
- **File unreadable:** Notify user, ask for alternative format
- **Invalid data:** Ask user to clarify or correct
- **Jekyll build fails:** Show error, attempt automatic fix, re-validate
- **Section conflict:** Show diff, ask user to choose version

---

_This workflow generates content for `index.md`._
