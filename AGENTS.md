## Agent skills

### Issue tracker

GitHub Issues via the `gh` CLI. See `docs/agents/issue-tracker.md`.

### Triage labels

Default vocabulary: needs-triage, needs-info, ready-for-agent, ready-for-human, wontfix. See `docs/agents/triage-labels.md`.

### Domain docs

Single-context: one CONTEXT.md + docs/adr/ at repo root. See `docs/agents/domain.md`.

### Language

Write all markdown you generate for this repo — specs, issues, task files, `CONTEXT.md`, ADRs, reports — in **English**. Code, commands, file paths, identifiers, and quoted error messages stay verbatim. This applies to new output only; don't translate existing docs.

Exception: spot records under `spots/` are written in **Chinese** — they document Chinese-language events and quote Chinese sources.
