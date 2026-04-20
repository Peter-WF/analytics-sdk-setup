# Analytics SDK Setup

English README. For the Chinese version, see [`README.zh-CN.md`](./README.zh-CN.md).

A repo-aware agent skill for auditing, installing, comparing, aligning, and fixing TikTok Pixel + Meta Pixel integrations.

## Why this project

Analytics SDK work usually fails for boring reasons: snippets get pasted into the wrong place, the same platform gets initialized twice, TikTok and Meta drift apart, and privacy-sensitive behavior gets enabled without a clear policy.

This project packages that experience into a reusable skill for repo-aware coding agents. Instead of treating analytics as a copy-paste task, it treats it as an engineering and governance problem.

### What makes it different

- **Repo-aware, not snippet-first** — inspect the real codebase before editing.
- **Dual-platform by design** — reason about TikTok and Meta together, not as two unrelated patches.
- **Execution-mode driven** — force the agent to choose between direct implementation, planning, or questions first.
- **Privacy-conservative by default** — do not invent CSP, LDU, consent, or PII behavior.
- **Structured outputs** — support questions-first, plan-first, and direct-implementation deliverables.

## Install with `skills` CLI

The easiest way to try this skill is with the [`skills`](https://www.npmjs.com/package/skills) CLI.

### Quick install demo

Install this skill into a Claude Code project from a local path:

```bash
npx skills add ./analytics-sdk-setup -a claude-code
```

Install from a GitHub repository source:

```bash
npx skills add Peter-WF/analytics-sdk-setup -a claude-code
```

Verify what is installed:

```bash
npx skills list -a claude-code
```

### Project vs global install

For Claude Code, the `skills` CLI supports both project and global installation scopes.

| Scope | Flag | Claude Code path | Use case |
|---|---|---|---|
| Project | default | `.claude/skills/` | Share with the project and commit if needed |
| Global | `-g` | `~/.claude/skills/` | Reuse across projects |

Example global install:

```bash
npx skills add ./analytics-sdk-setup -g -a claude-code
```

### Other useful `skills` commands

```bash
# List installed skills
npx skills list

# Search for skills
npx skills find analytics

# Check for updates
npx skills check

# Update installed skills
npx skills update

# Remove a skill
npx skills remove analytics-sdk-setup -a claude-code
```

### Installation notes

- The CLI supports local paths, GitHub shorthand, full GitHub URLs, GitLab URLs, and generic git URLs.
- Interactive installs can use **Symlink** or **Copy** mode.
- **Symlink** is the recommended default because it keeps a canonical source of truth.
- Use `--copy` if symlinks are not supported in your environment.

## What this skill does

This skill is designed for coding agents that need to work on tracking code inside a real repository.

It helps agents:

- audit an existing TikTok / Meta setup
- compare the two platforms and identify drift
- fix duplicate initialization
- align event timing and event semantics
- review bootstrap ownership and source of truth
- respect consent, CSP, LDU, and Advanced Matching guardrails

## How it works

The skill forces a repo-aware workflow instead of allowing the agent to jump straight into code.

### Required first step

Before doing anything, the agent must:

1. classify the execution mode
2. output the chosen mode explicitly
3. justify the choice in 1–2 lines

### Execution modes

- **Mode A — Direct implementation**: use when the repo, scope, config source, and privacy constraints are all clear.
- **Mode B — Plan first**: use when the task is still architecture-heavy, broad, or governance-oriented.
- **Mode C — Questions first**: use when key blockers such as Pixel ID, consent behavior, route scope, or ownership are still unclear.

### Core operating model

The skill uses a dual-platform decision frame:

- per-platform correctness
- cross-platform consistency
- ownership and source of truth
- governance over local fixes

That means it does not only ask “Is TikTok working?” It also asks whether TikTok and Meta stay aligned, whether duplicate init exists, whether ownership is clear, and whether privacy-sensitive changes are actually allowed.

## Project structure

```text
analytics-sdk-setup/
├── README.md
├── README.zh-CN.md
├── SKILL.md
├── SKILL.zh-CN.md
├── agent-system-prompt.md
├── agent-system-prompt.zh-CN.md
└── references/
    ├── install-and-events.md
    ├── install-and-events.zh-CN.md
    ├── privacy-and-csp.md
    └── privacy-and-csp.zh-CN.md
```

### File roles

- `SKILL.md` — canonical skill spec
- `agent-system-prompt.md` — compact runtime summary for environments that want a single prompt block
- `references/install-and-events.md` — bootstrap placement, event mapping, payloads, and verification
- `references/privacy-and-csp.md` — consent, CSP, LDU, Advanced Matching / PII guardrails
- `*.zh-CN.md` — Chinese companion versions for review and team adoption

## References

Use the main README to understand the project, then go deeper with the docs below.

### Canonical spec

- [`SKILL.md`](./SKILL.md)
- [`SKILL.zh-CN.md`](./SKILL.zh-CN.md)

### Compact runtime prompt

- [`agent-system-prompt.md`](./agent-system-prompt.md)
- [`agent-system-prompt.zh-CN.md`](./agent-system-prompt.zh-CN.md)

### Install and event reference

- [`references/install-and-events.md`](./references/install-and-events.md)
- [`references/install-and-events.zh-CN.md`](./references/install-and-events.zh-CN.md)

### Privacy and CSP reference

- [`references/privacy-and-csp.md`](./references/privacy-and-csp.md)
- [`references/privacy-and-csp.zh-CN.md`](./references/privacy-and-csp.zh-CN.md)

## Example prompts

Use this skill for requests like:

- “Compare TikTok and Meta tracking in this repo and tell me where they drift.”
- “Fix duplicate Pixel initialization and keep TikTok and Meta aligned.”
- “Add Purchase tracking for TikTok and Meta using the same business moment and shared contract.”
- “Audit ownership, consent gating, and source of truth for TikTok and Meta events.”

You can also use it for narrower requests such as:

- fixing duplicate init in one app shell
- adding one concrete event at a known success point
- reviewing whether CSP changes are really required

## Safety and verification notes

This skill is intentionally conservative.

- It does **not** assume Pixel IDs, consent rules, CSP allowlists, LDU rules, or PII sources.
- It does **not** enable Advanced Matching / PII by default.
- It prefers reusing existing analytics and privacy abstractions in the repo.
- It treats privacy-sensitive behavior as a blocker when policy is unclear.

For installation verification, use the `skills` CLI itself:

```bash
npx skills list -a claude-code
```

For runtime verification, the most important checks are:

- bootstrap initializes once per platform
- page-view behavior is not duplicated
- target events fire once at the real business moment
- privacy gates behave as intended

## Related links

- [`skills` on npm](https://www.npmjs.com/package/skills)
- [Agent Skills specification](https://agentskills.io)
- [skills.sh](https://skills.sh)
- [Claude Code skills documentation](https://code.claude.com/docs/en/skills)
