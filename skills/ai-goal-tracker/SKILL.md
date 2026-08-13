---
name: ai-goal-tracker
description: Pull goal tracker templates (goal.md, steps.md, current.md) from the remote repo and place them in a working directory. Use when starting a new project that needs the goal-tracking template set.
---

# AI Goal Tracker - Template Pull Skill

Use this skill when starting a new project and need the goal-tracking template files (goal.md, steps.md, current.md) in a working directory.

## When to Use

- Starting a new project or task
- A fresh session needs the goal-tracking files
- User asks for "goal tracker templates" or similar

## Steps

### 1. Clone the latest templates

Run in the target directory (or parent directory if creating a new project structure):

```bash
export PATH="/c/Program Files/GitHub CLI:$PATH"
mkdir -p . && git clone https://github.com/joschemd/ai-goal-tracker-template.git __goal_tracker_tmp__ && cp __goal_tracker_tmp__/templates/*.md . && rm -rf __goal_tracker_tmp__
```

Or to pull into a subdirectory:

```bash
export PATH="/c/Program Files/GitHub CLI:$PATH"
mkdir -p templates && cd templates && git clone https://github.com/joschemd/ai-goal-tracker-template.git __goal_tracker_tmp__ && cp __goal_tracker_tmp__/templates/*.md . && rm -rf __goal_tracker_tmp__
```

### 2. Verify files were pulled

Check that all three files are present:

```bash
ls goal.md steps.md current.md
```

### 3. Initialize the goal (if not already done)

If goal.md is still the template, work with the user to fill in:
- Intent
- Desired End State
- Success Criteria
- Relevant Context

### 4. Initialize steps and current

- Set initial steps in steps.md based on the goal
- Set current.md to document the starting state

## Files

The repo contains:
- `templates/goal.md` — Goal definition template
- `templates/steps.md` — Step tracking with status markers
- `templates/current.md` — Current status and blockers

## Notes

- The repo is public, so no authentication is required for cloning
- Use `git pull` within an existing clone to get updates to the templates
- If git is not installed on the system, use the `winget`/`choco` installer or download manually from https://github.com/joschemd/ai-goal-tracker-template
