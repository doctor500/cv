---
description: Create branch and PR workflow (prevents direct commits to main/page-release)
---

# Git Branch & Pull Request Workflow

**Purpose:** Guide developers through proper git workflow using feature branches and pull requests. Prevents direct commits to protected branches (`main` and `page-release`).

## Important Principles

> [!WARNING]
> **Never commit directly to `main` or `page-release` branches!**
> Always create a feature branch and submit a pull request.

> [!IMPORTANT]
> **AI Agent Note:** Never auto-commit changes without explicit user approval. Always ask before executing git commit commands.

---

## Workflow Steps

### Step 1: Check Current Branch & Context

```bash
git branch --show-current
```

**Scenario A: On `main` or `page-release`**
Proceed to **Step 2** to create a new feature branch.

**Scenario B: On a feature branch (e.g., `feat/my-feature`)**
Ask user:
```
You are currently on branch `[branch_name]`.
Do you want to:
1. Continue working on this branch
2. Create a new branch
```

**If Option 2 (Create New):**
Proceed to **Step 2**.

**If Option 1 (Continue):**
Check for existing PR immediately:
```bash
gh pr list --head [branch_name]
```

**If PR exists:**
Ask user:
```
A PR already exists for this branch:
[List PR details]

Do you want to:
1. Modify existing PR (add new commits)
2. Create a new PR (target different branch)
```
- If **Modify**: Proceed to **Step 5** (Stage Changes).
- If **Create New**: Proceed to **Step 5**, but note to run Step 9A later.

**If NO PR exists:**
Proceed to **Step 5** (Stage Changes).

---

### Step 2: Determine Change Type

Ask user:
```
What type of change are you making?
1. Feature (new functionality)
2. Fix (bug fix)
3. Docs (documentation only)
4. Refactor (code improvement)
5. Chore (maintenance, dependencies)
```

Store as `$CHANGE_TYPE` (feat, fix, docs, refactor, chore).

---

### Step 3: Create Feature Branch

Ask user:
```
Brief description of the change (lowercase, hyphens, e.g., 'add-skills-section'):
```

Store as `$DESCRIPTION`.

Create branch name: `$CHANGE_TYPE/$DESCRIPTION`

Example: `feat/add-skills-section`, `fix/typo-in-experience`, `docs/update-readme`

```bash
git checkout -b $CHANGE_TYPE/$DESCRIPTION
```

Confirm:
```
✅ Created and switched to branch: $CHANGE_TYPE/$DESCRIPTION

You can now make your changes!
```

---

### Step 4: Make Changes

User makes changes to files (or AI helps make changes).

Show status:
```bash
git status
```

---

### Step 5: Stage Changes

Ask user which files to stage:
```
Modified files:
[list from git status]

Stage all files? (yes/no/select)
```

If "yes":
```bash
git add .
```

If "select":
```
Enter file numbers to stage (comma-separated):
```

Then:
```bash
git add [selected files]
```

---

### Step 6: Commit Changes (WITH APPROVAL)

> [!CAUTION]
> **AI must ask for approval before committing!**

Show preview:
```
Ready to commit with message:
"$CHANGE_TYPE: $DESCRIPTION"

Staged files:
[list staged files]

Proceed with commit? (yes/no/edit-message)
```

If "edit-message":
```
Enter custom commit message:
```

If "yes" (ONLY AFTER USER CONFIRMS):
```bash
git commit -m "$MESSAGE"
```

Confirm:
```
✅ Committed changes locally
```

---

### Step 7: Push to Remote

```bash
git push origin $CHANGE_TYPE/$DESCRIPTION
```

If first push:
```bash
git push -u origin $CHANGE_TYPE/$DESCRIPTION
```

---

### Step 8: Check for Existing Pull Request

Check if PR already exists for this branch:

**Method 1: GitHub CLI (if installed)**
```bash
gh pr list --head $CHANGE_TYPE/$DESCRIPTION
```

**Method 2: GitHub API**
```bash
curl -s "https://api.github.com/repos/[username]/[repo]/pulls?head=[username]:$CHANGE_TYPE/$DESCRIPTION&state=open"
```

**Method 3: Manual Check**
Ask user:
```
Does a PR already exist for this branch?
Check: https://github.com/[username]/[repo]/pulls

(yes/no)
```

---

### Step 9A: Create New Pull Request (if none exists)

Ask user:
```
Target branch for PR?
1. page-release (for deployment)
2. main (for staging/review)
```

Store as `$TARGET_BRANCH`.

**Option 1: GitHub CLI**
```bash
gh pr create --base $TARGET_BRANCH --head $CHANGE_TYPE/$DESCRIPTION --title "$CHANGE_TYPE: $DESCRIPTION" --body "## Changes\n\n[User describes changes]\n\n## Verification\n\n- [ ] Tested locally\n- [ ] Documentation updated\n- [ ] Ready for review"
```

**Option 2: GitHub Web UI**
```
Visit: https://github.com/[username]/[repo]/compare/$TARGET_BRANCH...$CHANGE_TYPE/$DESCRIPTION

Fill in:
- Title: $CHANGE_TYPE: $DESCRIPTION
- Description: 
  ## Changes
  [Describe what changed]
  
  ## Verification
  - [ ] Tested locally
  - [ ] Documentation updated
  - [ ] Ready for review

Click "Create pull request"
```

**Option 3: Manual Instructions**
```
1. Go to https://github.com/[username]/[repo]
2. Click "Pull requests" > "New pull request"
3. Base: $TARGET_BRANCH
4. Compare: $CHANGE_TYPE/$DESCRIPTION
5. Add title and description
6. Create pull request
```

Confirm:
```
✅ Pull request created!

Next steps:
1. Review the PR
2. Address any feedback
3. Merge when approved
```

---

### Step 9B: Update Existing Pull Request (if exists)

```
PR already exists for this branch!

The new commits will automatically appear in the existing PR after pushing.

Would you like to:
1. View PR in browser
2. Update PR description
3. Continue
```

If option 1:
```bash
gh pr view --web
```

If option 2:
```bash
gh pr edit --body "Updated description..."
```

---

### Step 10: Merge Pull Request (After Approval)

> [!IMPORTANT]
> Only merge PRs that have been reviewed and approved!

**Option 1: GitHub CLI**
```bash
gh pr merge --squash
```

**Option 2: GitHub Web UI**
```
1. Go to PR page
2. Click "Merge pull request"
3. Choose merge strategy (squash recommended)
4. Confirm merge
```

After merging:
```bash
# Switch back to target branch
git checkout $TARGET_BRANCH

# Pull latest changes
git pull origin $TARGET_BRANCH

# Delete feature branch
git branch -d $CHANGE_TYPE/$DESCRIPTION
git push origin --delete $CHANGE_TYPE/$DESCRIPTION
```

Confirm:
```
✅ PR merged successfully!
✅ Cleaned up feature branch

Your changes are now in $TARGET_BRANCH!
```

---

## Quick Reference

### Common Commands

```bash
# Check current branch
git branch --show-current

# Create new branch
git checkout -b feat/my-feature

# Stage all changes
git add .

# Commit (after user approval!)
git commit -m "feat: description"

# Push to remote
git push origin feat/my-feature

# Create PR (GitHub CLI)
gh pr create --base page-release

# List open PRs
gh pr list

# View PR
gh pr view --web
```

---

## Branch Naming Convention

Format: `<type>/<description>`

**Types:**
- `feat/` - New features
- `fix/` - Bug fixes
- `docs/` - Documentation changes
- `refactor/` - Code refactoring
- `chore/` - Maintenance tasks

**Description:**
- Lowercase
- Hyphens instead of spaces
- Brief but descriptive

**Examples:**
- `feat/add-certifications-section`
- `fix/pdf-generation-layout`
- `docs/update-quick-reference`
- `refactor/simplify-css-structure`
- `chore/update-dependencies`

---

## Protected Branches

| Branch | Purpose | Protection |
|--------|---------|------------|
| `main` | Primary development, staging | No direct commits |
| `page-release` | GitHub Pages deployment | No direct commits |
| Feature branches | Active development | Free to commit |

---

## Troubleshooting

### Already committed to main/page-release?

```bash
# Move commits to new branch
git checkout -b feat/my-changes

# Reset main to remote state
git checkout main
git reset --hard origin/main

# Push feature branch
git push origin feat/my-changes
```

### PR conflicts?

```bash
# Update feature branch with latest target
git checkout feat/my-feature
git fetch origin
git merge origin/main  # or origin/page-release

# Resolve conflicts, then:
git add .
git commit -m "fix: resolve merge conflicts"
git push origin feat/my-feature
```

### Need to update PR after feedback?

```bash
# Make changes
# Stage changes
git add .

# Commit (after approval!)
git commit -m "fix: address review feedback"

# Push - will update PR automatically
git push origin feat/my-feature
```

---

## Example Session

```
🤖: What type of change are you making?
    1. Feature
    2. Fix
    3. Docs
    4. Refactor
    5. Chore

👤: 1

🤖: Brief description (lowercase, hyphens):

👤: add-language-skills

🤖: Creating branch: feat/add-language-skills
    
    $ git checkout -b feat/add-language-skills
    
    ✅ Ready to make changes!

[User edits files]

🤖: Stage all modified files? (yes/no)

👤: yes

🤖: Ready to commit with message:
    "feat: add-language-skills"
    
    Staged files:
    - index.md
    
    Proceed with commit? (yes/no)

👤: yes

🤖: $ git commit -m "feat: add language skills section"
    ✅ Committed
    
    $ git push -u origin feat/add-language-skills
    ✅ Pushed to remote
    
    Checking for existing PR...
    No existing PR found.
    
    Target branch for PR?
    1. page-release (deploy)
    2. main (staging)

👤: 2

🤖: Creating PR targeting main...
    
    $ gh pr create --base main --head feat/add-language-skills
    
    ✅ PR created: https://github.com/[username]/[repo]/pull/123
    
    Next: Review and merge when ready!
```

---

## AI Agent Guidelines

When using this workflow as an AI agent:

1. ✅ **DO:**
   - Always check current branch first
   - Create feature branches for all changes
   - Ask for commit approval BEFORE executing
   - Check for existing PRs before creating new ones
   - Provide clear branch and commit suggestions

2. ❌ **DON'T:**
   - Never auto-commit without user permission
   - Never commit directly to main or page-release
   - Never skip the PR process
   - Never force push without warning

---

_This workflow ensures clean git history and prevents accidental commits to protected branches._
