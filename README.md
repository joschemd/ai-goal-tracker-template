# AI Goal Tracker Template

A lightweight, session-persistent project tracking system with three documents designed for AI-assisted workflows.

## Files

- **templates/** — The three markdown templates:
  - `goal.md` — Define the project goal, intent, scope, and success criteria
  - `steps.md` — Step-by-step task list with status markers
  - `current.md` — Current status, blockers, and next actions

## How It Works

1. **Goal** describes the end state. Any session reads this first to understand the target.
2. **Steps** breaks work into numbered, actionable items with status tracking (`[ ] pending`, `[~] in progress`, `[x] done`, `[-] blocked`, `[!] needs review`).
3. **Current** captures the latest status so any session can pick up exactly where the last one left off.

The agent (AI assistant) keeps steps.md and current.md up to date as work progresses. If the goal isn't defined yet, the agent works with the user to define it first.

## Setup

### Quick Start

Clone the templates into your project directory:

```bash
mkdir -p templates && cd templates && git clone https://github.com/joschemd/ai-goal-tracker-template.git __tmp__ && cp __tmp__/templates/*.md . && rm -rf __tmp__
```

### As a Skill

If using Hermes Agent, load the `ai-goal-tracker` skill to auto-pull templates on demand.

## Why These Three Files?

- **Goal** — Self-contained enough that any future session knows what "done" looks like
- **Steps** — Incremental progress tracking, always up to date by the agent
- **Current** — Lightweight status snapshot; no need to re-read the full conversation history

## License

MIT
