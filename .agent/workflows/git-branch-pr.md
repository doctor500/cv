---
description: Create branch and PR workflow (prevents direct commits to main/page-release)
---

# Git Branch & Pull Request Workflow

// turbo-all

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

### Step 2: Determine Change Type & Branch Name

> [!TIP]
> **Smart Inference:** If the change type and description are obvious from context (e.g., user already made changes or described the task), the AI may propose a branch name directly and ask for confirmation in a single prompt, rather than asking two separate questions.

**If context is clear**, propose directly:
```
Based on your changes, I suggest:
  Branch: fix/update-cert-url
  
Proceed? (yes/no/edit)
```

**If context is ambiguous**, ask:
```
What type of change are you making?
1. Feature (new functionality)
2. Fix (bug fix)
3. Docs (documentation only)
4. Refactor (code improvement)
5. Chore (maintenance, dependencies)

Brief description (lowercase, hyphens, e.g., 'add-skills-section'):
```

Store as `$CHANGE_TYPE` (feat, fix, docs, refactor, chore) and `$DESCRIPTION`.

---

### Step 3: Create Feature Branch

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

> [!TIP]
> **Single File Shortcut:** If only one file is modified, stage it automatically and include it in the commit approval prompt (Step 6). No need to ask separately.

**If multiple files modified**, ask user:
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

> [!TIP]
> **Skip this step** if the branch was just created in Step 3 — there cannot be an existing PR for a freshly created branch. Proceed directly to Step 9A.

Check if PR already exists for this branch:

```bash
gh pr list --head $CHANGE_TYPE/$DESCRIPTION
```

---

### Step 9A: Create New Pull Request (if none exists)

> [!TIP]
> **Smart Inference:** If the user already specified the base branch (e.g., "branch from page-release"), use that as the PR target without asking.

**If target is not clear**, ask user:
```
Target branch for PR?
1. page-release (for deployment)
2. main (for staging/review)
```

Store as `$TARGET_BRANCH`.

**Create PR using GitHub CLI:**
```bash
gh pr create --base $TARGET_BRANCH --head $CHANGE_TYPE/$DESCRIPTION --title "$CHANGE_TYPE: $DESCRIPTION" --body "## Changes\n\n[Describe changes]\n\n## Verification\n\n- [ ] Tested locally\n- [ ] Ready for review"
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

**Preferred method (GitHub CLI):**
```bash
gh pr merge --squash --delete-branch
```

This will squash-merge the PR and automatically delete both local and remote feature branches.

**Fallback (GitHub Web UI):**
```
1. Go to PR page
2. Click "Merge pull request"
3. Choose merge strategy (squash recommended)
4. Confirm merge
```

If using Web UI, clean up manually:
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

# Merge PR (preferred)
gh pr merge --squash --delete-branch
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

## Branch Architecture

> [!CAUTION]
> **`main` is the upstream template for fork users.** Never sync personal CV content (`index.md`) from `page-release` to `main`. Doing so introduces breaking merge conflicts for every fork user.

| Branch | Purpose | `index.md` Contains | Protection |
|--------|---------|---------------------|------------|
| `main` | Upstream template for forks | Example/dummy CV ("Alex Johnson") | No direct commits, no full page-release sync |
| `page-release` | Personal GitHub Pages deployment | Personal CV data | No direct commits |
| Feature branches | Active development | Either | Free to commit |

### Content Flow

```
feature-branch → PR → page-release    (CV content changes)
feature-branch → PR → main            (template/infrastructure changes)
page-release → SELECTIVE sync → main   (infrastructure only, never index.md)
```

### Sync Rules (page-release → main)

| Sync? | Files |
|-------|-------|
| ✅ Always sync | `README.md`, `.agent/`, `media/`, `_layouts/`, `_config.yml`, `.github/`, `docker-compose.yml`, `Gemfile`, `.gitignore` |
| ❌ Never sync | `index.md`, `docs/evaluation/` |
| ⚠️ Case-by-case | `docs/governance.md`, other docs |

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
git merge origin/page-release  # or origin/main

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
🤖: You're on page-release with 1 modified file (index.md).
    Based on your changes, I suggest:
      Branch: fix/update-cert-url
    
    Proceed? (yes/edit)

👤: yes

🤖: ✅ Created branch: fix/update-cert-url
    
    Ready to commit with message:
    "fix: update certificate URL"
    
    Staged files:
    - index.md
    
    Proceed with commit? (yes/no/edit-message)

👤: yes

🤖: ✅ Committed and pushed to remote
    
    Creating PR targeting page-release...
    ✅ PR created: https://github.com/[user]/[repo]/pull/42

👤: lets merge it

🤖: $ gh pr merge --squash --delete-branch
    ✅ PR merged! Branch cleaned up.
    Your changes are now in page-release!
```

---

## Step 11: Sync Infrastructure to Main (File-Specific)

> [!WARNING]
> **Never do a full branch merge from `page-release` → `main`!**
> Use this file-specific approach instead.

When infrastructure changes on `page-release` need to reach `main`:

```bash
# 1. Create sync branch from main
git checkout main && git pull origin main
git checkout -b docs/sync-infra-to-main

# 2. Copy ONLY allowed files from page-release
git checkout page-release -- README.md .agent/ media/ _layouts/ _config.yml .github/ docker-compose.yml Gemfile .gitignore

# 3. Verify index.md is NOT staged
git diff --cached --name-only | grep -q '^index.md$' && echo '❌ STOP: index.md detected!' || echo '✅ Safe to proceed'

# 4. Commit, push, and PR to main
git commit -m "sync: infrastructure files from page-release"
git push origin docs/sync-infra-to-main
gh pr create --base main --title "sync: infrastructure from page-release"
```

**Pre-merge checklist:**
- [ ] `index.md` is NOT in the changed files list
- [ ] `docs/evaluation/` files are NOT included
- [ ] PR description lists which files are being synced

---

## AI Agent Guidelines

When using this workflow as an AI agent:

1. ✅ **DO:**
   - Always check current branch first
   - Create feature branches for all changes
   - Infer change type and branch name when context is clear
   - Ask for commit approval BEFORE executing
   - Skip redundant prompts (single file staging, fresh branch PR check)
   - Use `gh pr merge --squash --delete-branch` for clean merges
   - Provide clear branch and commit suggestions
   - **Use Step 11 (file-specific sync) for page-release → main syncs**
   - **Verify `index.md` is never included in PRs targeting `main`**

2. ❌ **DON'T:**
   - Never auto-commit without user permission
   - Never commit directly to main or page-release
   - Never skip the PR process
   - Never force push without warning
   - **Never do a full branch merge from page-release → main**
   - **Never sync `index.md` or `docs/evaluation/` to main**

---

_This workflow ensures clean git history and prevents accidental commits to protected branches._
