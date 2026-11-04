# Agent instructions

This repository uses a **vendor-neutral** agent setup. Do not rely on Cursor-only paths.

## Where things live

| Path | Purpose |
|------|---------|
| `.agents/skills/` | Shared OpenSpec skills (source of truth for agent workflows) |
| `openspec/` | Specs, changes, and project OpenSpec config |
| `AGENTS.md` | This file — cross-agent orientation |

## Do not use `.cursor`

This project **does not** use a `.cursor/` directory for skills, commands, rules, or MCP config.

- Prefer `.agents/skills/` over any `.cursor/skills` or `.cursor/commands` paths.
- Do not recreate `.cursor` for OpenSpec or project agent instructions.
- Tool-specific config (MCP, IDE settings) belongs in that tool’s own config, not in `.cursor`.

## OpenSpec

Workflow skills are under `.agents/skills/` (for example `openspec-propose`, `openspec-apply-change`, `openspec-explore`).

Typical entry points:

- Propose a change: use the `openspec-propose` skill (or `/openspec-propose "your idea"` when your agent supports that form).
- Apply / continue / archive: use the matching `openspec-*` skill in `.agents/skills/`.

Specs and change artifacts live in `openspec/`. Keep that directory as the shared source of truth across agents.

### Mandatory workflow (requirements & bug fixes)

All **feature requests** and **bug fixes** MUST go through the **full OpenSpec flow** (propose → artifacts → apply → verify/archive as appropriate). Do not jump straight to code edits for those requests.

**Exception:** If the user **explicitly** names an OpenSpec skill/command (e.g. `/openspec-explore`, `/openspec-apply-change`, `openspec-propose`), follow that skill as specified and do not substitute a different OpenSpec entry point.

### Risk & confirmation

When OpenSpec (or related skills) presents options:

| Risk | Agent behavior |
|------|----------------|
| **Low** | Choose the **recommended** option and proceed **without** asking for confirmation. State the choice briefly. |
| **High** | Stop and get **explicit human confirmation** before proceeding. |

Treat as **high risk** when the choice would materially affect: production data, auth/security, public API contracts, irreversible migrations, billing, or broad breaking changes. Everything else defaults to **low risk** (take the recommendation and continue).

## Updating agent tooling

To refresh shared skills without bringing back Cursor paths:

```bash
openspec init --tools agents --force
```

Do not pass `cursor` to `--tools` for this repo.
