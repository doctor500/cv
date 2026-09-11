# Decision Log

Append-only. Newest at the bottom. One entry per decision with context + rationale.

## YYYY-MM-DD — <Title>

- **Context:** ...
- **Decision:** ...
- **Rationale:** ...
- **Alternatives:** ...

## 2026-09-11 — CV content synced to Woven by Toyota; PROJECT.md filled

- **Context:** David's role changed — Site Reliability Engineer at Woven by Toyota (Sep 2026 - Present, Tokyo), with the Rakuten "Software Engineer - CI/CD Platform" role closed at Aug 2026. `index.md` on `page-release` still presented Rakuten as current, and `.agents/PROJECT.md` was still an empty template.
- **Decision:** Sync `index.md` to the new role via a PR targeting `page-release` (doctor500/cv#51), and fill `.agents/PROJECT.md` via this PR against `main` — the two land separately because `.agents/` does not exist on `page-release` and `index.md` must never reach `main`. The Woven entry ships with no description or bullets, matching `branding-context/v1/`.
- **Rationale:** `main` carries the fork template, so a single PR cannot carry both changes without either leaking personal CV content onto `main` or creating a `.agents/` file that only exists on the deployment branch. Splitting by target branch keeps the sync rules in `.agents/workflows/git-branch-pr.md` intact. The empty Woven entry avoids inventing achievements for a role one month old; it renders as a period/role line that fills in later without restructuring.
- **Alternatives:** One PR to `page-release` containing both changes — rejected, it would put `.agents/PROJECT.md` on the deployment branch only and skip the sync path. Committing the CV change straight to `main` — rejected, `main` is protected and is the dummy CV for forks.

