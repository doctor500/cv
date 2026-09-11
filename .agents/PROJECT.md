# Project

## Overview

A markdown-driven CV generator. `index.md` is the single source of truth; Jekyll renders it
through `_layouts/cv.html` with the kramdown parser, and GitHub Pages publishes it at
https://doctor500.github.io/cv/. The same markdown is also built to PDF by a GitHub Actions
pipeline, so one file feeds both the web CV and the downloadable PDF.

## Ownership & Access

Owner: David Layardi (GitHub `doctor500`).

Deployment: GitHub Pages builds from `page-release`. `publish-pdf.yml` builds the PDF on manual
`workflow_dispatch` — it takes release notes and a `build_private` flag that injects the phone
number from the `CV_PHONE_NUMBER` secret into `index.md` before rendering. It does not run on
pull requests, so a PR here gets no automated check.

Branch model, from `.agents/workflows/git-branch-pr.md`:

| Branch | Purpose | `index.md` holds |
|---|---|---|
| `main` | Upstream template for forks | Dummy CV ("Alex Johnson") |
| `page-release` | Personal GitHub Pages deployment | Real CV data |
| feature branches | Active development | Either |

Both `main` and `page-release` are protected — every change arrives by pull request. CV content
PRs target `page-release`; template and infrastructure PRs target `main`. Infrastructure reaches
`main` from `page-release` through a file-specific sync, never a branch merge.

## Scope

In scope: CV content in `index.md`, the Jekyll layout and `media/*.css` stylesheets, the PDF
pipeline, and the workflows and references in this folder.

Out of scope: `index.md` must never be synced from `page-release` into `main`. `main` is the fork
template, and carrying personal content there creates breaking merge conflicts for every fork
user; `docs/evaluation/` is excluded from that sync for the same reason. Note also that
`branding-context/v1/*.json` holds canonical copies of this data for other projects
(landing-page, business-card), so a role or experience change here propagates onward and is worth
announcing when it lands.

## Key Links

- Live: https://doctor500.github.io/cv/
- Repo: https://github.com/doctor500/cv
- PDF pipeline: `.github/workflows/publish-pdf.yml`
- Branch & PR workflow: `.agents/workflows/git-branch-pr.md`
- Downstream canonical data: `branding-context/v1/*.json`
