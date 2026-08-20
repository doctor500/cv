# Iterative Job-Targeted CV Optimization — Reusable Prompt Template

A copy-paste prompt for asking an AI assistant to run the full job-targeted CV
optimization loop used in this repository: inspect → read → assess JD & company →
deep-dive evaluate → plan → revise → re-evaluate → iterate until ready for submission.

---

## How to use

1. Open a new conversation in a coding-agent tool (e.g. Opencode, Cursor, Claude Code)
   with this repository as the working directory.
2. Copy the **Reusable Prompt Template** block below.
3. Fill in the placeholders in `{{double_braces}}`. Leave any placeholder you do
   not have as `none`, and the assistant will ask follow-ups.
4. Paste the filled prompt as your first message.
5. Review the assistant's plan, answer its clarifying questions, and approve
   each major step (JD assessment, revision path, iteration stops).

The prompt assumes the assistant has access to the repo files, especially:

- `.agent/PROJECT_CONTEXT.md`
- `.agent/workflows/optimize-cv-iterative.md`
- `.agent/workflows/evaluate-cv-deepdive.md`
- `.agent/references/cv-evaluation-framework.md`
- `index.md`

---

## Reusable Prompt Template

````text
I want you to run an iterative, job-targeted CV optimization for a specific
application, following this repository's workflow. Do not start editing until
you have inspected the repo and produced a plan I approve.

================================================================
INPUTS
================================================================
- Target role:              {{job_title}}
- Target company:           {{company_name_or_unknown}}
- Job description source:   {{jd_path_or_url_or_text_or_pdf}}
- Supporting resources:     {{list_of_paths_urls_or_none}}
    (e.g. branding-context folder, portfolio, prior CV variants,
     project notes, LinkedIn URL, interview prep docs)
- Revision output preference:
    {{one_of: update_index_md | create_job_specific_variant | you_decide}}
    If variant: suggested filename = {{variant_filename_or_auto}}
- Artifact regeneration:
    - Regenerate web/PDF artifacts when CV changes? {{yes|no|only_at_end}}
- Evaluation report format: {{html|markdown|both}}
- Stop condition:
    - Iterate until you can confidently say the CV is submission-ready,
      OR I explicitly tell you to stop.

================================================================
PROCESS YOU MUST FOLLOW
================================================================
Follow `.agent/workflows/optimize-cv-iterative.md` as the top-level loop and
`.agent/workflows/evaluate-cv-deepdive.md` (with the 10 Insight Quality
Standards from `.agent/references/cv-evaluation-framework.md`) as the
evaluation engine.

Concretely:

1. Inspect the repo & context first
   - Read `.agent/PROJECT_CONTEXT.md` to confirm structure and conventions.
   - Read the two workflow files and the evaluation framework reference.
   - Confirm the default CV source (`index.md`) and any existing variants.
   - Do NOT edit anything yet.

2. Read the CV and supporting sources
   - Read `index.md` (or the variant I named) completely.
   - Read every supporting resource I listed (branding-context, portfolio
     files, prior CV versions, etc.). Prefer reading full files over guessing.
   - Build a synthesis of strongest proven achievements, recurring technical
     strengths, real scope vs role titles, and evidence not yet surfaced.

3. Ingest and assess the job description
   - Fetch/parse the JD from the source I gave (PDF, URL, markdown, or text).
     Use curl → browser → ask-me-to-paste as fallback chain if needed.
   - Extract: role scope, must-haves, preferred requirements, keywords,
     collaboration/culture signals, location/language requirements.

4. Assess the company & team context (if company is identifiable)
   - Research: company profile and core business, relevant product/BU tied
     to the role, team/division, approximate org scale, engineering culture,
     product stage, technical stack/tooling signals.
   - Flag how AI/ML, platform ownership, reliability, or speed affect the
     positioning of this CV.

5. Present a brief plan and wait for my approval
   - Summarize JD + company findings.
   - Summarize the candidate profile synthesis.
   - State the initial hypothesis for how to position me.
   - Propose the revision path (`index.md` vs variant) with a recommendation.
   - Ask any clarifying questions one at a time.
   - STOP and wait for me to approve before editing anything.

6. Run a deep-dive CV evaluation (Round 1)
   - Execute the full `/evaluate-cv-deepdive` procedure against the current
     CV, using the JD + company assessment as job-target context.
   - Enforce all 10 Insight Quality Standards.
   - Produce: positioning diagnosis, career arc, score card, keyword match,
     requirement gap matrix, top 3 high-impact actions with score deltas,
     and rewrite examples with exact replacement text.
   - SAVE this as an HTML (or markdown) report under `docs/evaluation/`
     with a filename that includes the company/role and round number,
     e.g. `docs/evaluation/cv-eval-{{company}}-{{role}}-round1.html`.

7. Propose a revision strategy
   - Translate the evaluation into a concrete improvement plan, separating:
     high-value missing signals, weak/misleading framing, evidence available
     in supporting docs but not surfaced, and nice-to-have vs real blockers.
   - For each item: what is wrong, why it matters for THIS role/company,
     is it safely supportable, estimated score/fit impact.
   - Confirm with me: update `index.md` vs create a job-specific variant.
     Honor my preference from INPUTS unless I said `you_decide`.

8. Apply the revision
   - Keep all claims truthful and supportable.
   - Prefer stronger signal and clearer alignment over length.
   - Preserve overall CV structure/formatting conventions from this repo.
   - If using a variant, create a new markdown file and keep `index.md` intact.
   - If artifact regeneration is enabled, regenerate web/PDF outputs as needed
     (docker-compose, bundle exec jekyll, or the Build PDF GitHub Action, per
     `.agent/PROJECT_CONTEXT.md`). Otherwise defer to the final round.

9. Re-run the deep-dive evaluation (Round N+1)
   - Re-evaluate the revised CV against the same JD + company context.
   - Save a new report with the incremented round number.
   - Compare to the previous round: score delta, what improved, what
     still lags, whether remaining items are blockers or polish.

10. Decide whether to iterate again
    - Iterate again if important JD/company-fit gaps remain, or the score
      is not yet strong, or key signals are still under-explained, or the
      revision introduced new issues.
    - Stop if the CV is strongly aligned and remaining issues are optional
      polish, or if I tell you to stop.
    - Present the decision explicitly before continuing or stopping.

11. Finalize when ready for submission
    - State the final CV file path, final report path(s), composite score,
      and a clear "ready to submit" verdict.
    - Optionally regenerate the PDF/web artifacts for the final version.
    - List any remaining non-blocking gaps I should be ready to address
      at interview stage.

================================================================
RULES
================================================================
- Always evaluate before revising; never edit the CV blindly.
- Use supporting resources aggressively but do not invent claims that
  cannot survive interview scrutiny.
- Distinguish value from length — prefer stronger signal over more words.
- Save an evaluation report for at least Round 1 and the final round.
- Never auto-commit to git. If commits are needed, ask me first and use
  a feature branch per this repo's git conventions.
- Respect protected branches (`main`, `page-release`) — do not push to them.

================================================================
FIRST RESPONSE EXPECTATIONS
================================================================
In your first reply:
1. Confirm you have read the four context files listed above.
2. Show me the parsed JD summary + company/team assessment.
3. Show me the candidate-profile synthesis from my CV and supporting docs.
4. Propose the initial positioning hypothesis and revision path.
5. Ask any clarifying questions, ONE at a time.
6. Do NOT modify any files yet.
````

---

## Optional notes & customization

- **If you only have a JD URL and no company name:** set `company_name_or_unknown`
  to `unknown` and the assistant will attempt to infer it from the JD or skip
  company research gracefully.
- **If the JD is a PDF:** give an absolute path; the assistant should read it
  directly. If extraction fails, it will ask you to paste the text.
- **If you want a private/confidential variant:** choose
  `create_job_specific_variant` and add "do not publish; local use only" to
  the supporting resources line. Keep it out of `page-release`.
- **For faster cycles:** you can tell the assistant to switch Round 2+ to
  `/evaluate-cv-quick` once the structural issues are resolved and only
  fine-tuning remains. Ask for a final deep-dive before declaring ready.
- **For multiple applications in parallel:** run this prompt once per
  target role and keep each variant file + report set named after the
  company/role so iteration history stays clean.
- **Stop condition tuning:** if you want a hard cap, add a line to INPUTS
  such as `Max rounds: 3` and the assistant will stop even if the CV is not
  yet ideal, returning its best version plus outstanding gaps.
- **Repo-independent use:** this prompt assumes the `.agent/` workflow files
  exist. If you reuse it in a repo that lacks them, either copy those files
  over first or paste their key sections inline in the prompt.
