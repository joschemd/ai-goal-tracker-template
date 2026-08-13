---
name: ai-goal-tracker
description: Pull goal tracker templates (goal.md, steps.md, current.md) from git and initialize project goal. Use when starting a project that needs structured goal tracking.
---

# AI Goal Tracker - Pull Templates Skill

Use this skill when starting a new project and need the goal-tracking template files (goal.md, steps.md, current.md).

## When to Use

- Starting a new project or task
- A fresh session needs the goal-tracking files
- User asks for "goal tracker templates" or similar
- Project needs structured goal, step, and status tracking

## Steps

### 1. Determine target directory

Ask the user where they want the templates, or default to the current working directory.

### 2. Pull templates from git

Run the following command to clone just the templates directory and copy them to the target:

```bash
export PATH="/c/Program Files/GitHub CLI:$PATH"
TMPDIR=$(mktemp -d)
cd "$TMPDIR" && git clone --depth 1 https://github.com/joschemd/ai-goal-tracker-template.git __tmp__ && \
  cp __tmp__/templates/*.md "$TARGET_DIR"/ && \
  rm -rf "$TMPDIR"
```

If git is not installed, download the zip:
```bash
curl -L https://github.com/joschemd/ai-goal-tracker-template/archive/refs/heads/main.zip -o /tmp/templates.zip
unzip -j /tmp/templates.zip "*/templates/*.md" -d "$TARGET_DIR"
rm /tmp/templates.zip
```

### 3. Verify files were pulled

```bash
ls "$TARGET_DIR"/goal.md "$TARGET_DIR"/steps.md "$TARGET_DIR"/current.md
```

### 4. Initialize the goal document (if not already filled in)

Read goal.md - if it still contains all the template placeholders, ask the user to define:
- **Intent**: One sentence describing what the project is about
- **Why**: Why does this matter? What problem does it solve?
- **Desired End State**: What does "done" look like?
- **Success Criteria**: How do we know we succeeded?
- **Relevant Context**: Any key facts or dependencies

Fill these in collaboratively with the user based on their answers.

### 5. Initialize steps and current

- Create initial steps in steps.md based on the defined goal
- Set current.md to document the starting state with:
  - Last Update: current timestamp
  - What Changed: initial setup
  - Current Focus: ready to begin first step
  - Blockers: none yet
  - Next Actions: the first step from steps.md

## Files

The repo contains (https://github.com/joschemd/ai-goal-tracker-template):
- `templates/goal.md` — Goal definition template
- `templates/steps.md` — Step tracking with status markers ([ ], [~], [x], [-], [!])
- `templates/current.md` — Current status, blockers, and next actions

## Notes

- The repo is public, so no authentication is required for cloning
- Use `git pull` within an existing clone to get template updates
- If git is not installed, use `winget install GitHub.cli` or download manually
- After this skill runs, the agent should keep steps.md and current.md up to date as work progresses
