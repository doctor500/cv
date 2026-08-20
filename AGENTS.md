# AGENTS.md

> **Single source of truth for this project.** Every agent platform (Hermes, OpenCode,
> OpenAgents, Claude Code, Cursor, GitHub Copilot, Windsurf, Gemini CLI, Aider, Codex)
> reads this file. Per-platform shims (CLAUDE.md, .cursorrules, GEMINI.md, .windsurfrules,
> .aider.conf.yml) all point here. **Do not duplicate project facts in this file — update
> `.agents/` instead.**

## Golden Rules

1. **Read before acting:** `.agents/README.md` first — it routes you to the right file for the task.
2. **One fact = one home.** Reference via pointer; never restate facts here or in shims.
3. **Write after changing state:** update the relevant `.agents/` file (DECISIONS.md, memory/, docs/).
4. **Keep this file lean.** Detail lives in `.agents/`.

## Where Things Live

| File | When to load |
|------|--------------|
| `.agents/README.md` | Always — the router table |
| `.agents/PROJECT.md` | Project overview, ownership, scope |
| `.agents/ARCHITECTURE.md` | System design, data flow, key paths |
| `.agents/CONVENTIONS.md` | Coding standards, style, actionable rules |
| `.agents/DECISIONS.md` | Decision log (append-only) |
| `.agents/memory/` | Agent session memory (read/update) |
| `.agents/docs/` | Deep reference, runbooks |
| `.agents/skills/` | Platform-neutral agent skills (SKILL.md) |

## Commands

<!-- Replace with real commands: install / dev / build / test / lint -->

## Repository Structure

<!-- One block: top-level layout + where the core logic lives -->

## Agent Protocol

- **Plan before action:** read context, state your plan, get approval for destructive changes.
- **Verify after change:** run tests/lint before finishing; report what changed.
- **Sync:** after finishing, run `agents-sync check` and resolve warnings.
