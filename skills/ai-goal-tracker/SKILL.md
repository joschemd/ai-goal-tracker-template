---
name: ai-goal-tracker
description: Auto-detect, pull, and maintain goal tracker templates (goal.md, steps.md, current.md). Use when starting work in a directory that may have goal tracker files, or when files exist but may need updating from the remote repo.
---

# AI Goal Tracker - Auto-Detect & Self-Maintain Skill

Auto-detects goal tracker files in the working directory and pulls/updates them from git without requiring explicit user prompting. Self-initializes when files are missing, updates them when they exist but may be stale.

## When to Use

- Starting work in a new or existing directory that may have goal tracker files
- Need to ensure goal.md, steps.md, current.md are up to date
- Agent detects goal tracker files exist but may need updating
- Any session needs to pull the latest templates without user intervention

## Auto-Detection Logic

At the start of any session that involves a directory:
1. Check if goal.md, steps.md, and current.md exist in the working directory
2. If any are missing: pull all three from git and self-initialize
3. If all exist but the directory is NOT a git clone: convert to a git repo and track them
4. If all exist in an existing clone: pull latest and review for updates needed

## Steps

### 1. Determine target directory

Use the current working directory unless the user specifies otherwise.

### 2. Check for existing files

```bash
ls "$TARGET_DIR"/goal.md "$TARGET_DIR"/steps.md "$TARGET_DIR"/current.md
```

If any are missing, go to step 3. If all exist, go to step 4.

### 3. Pull templates from git (when files missing)

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

Verify files were pulled:
```bash
ls "$TARGET_DIR"/goal.md "$TARGET_DIR"/steps.md "$TARGET_DIR"/current.md
```

Then proceed to step 5.

### 4. Review existing files for updates (when files exist)

Read all three files. Check:
- goal.md: Does it still contain template placeholders? If yes, ask user to fill it in collaboratively.
- steps.md: Are any steps blocked or needing review? Update status markers as needed.
- current.md: Is it up to date with the latest work?

If the directory is not yet a git repo, initialize it:
```bash
export PATH="/c/Program Files/GitHub CLI:$PATH"
cd "$TARGET_DIR" && git init && git add goal.md steps.md current.md && git commit -m "Add goal tracker templates" 2>&1
```

If you want to track against the template repo, add remote:
```bash
git remote add origin https://github.com/joschemd/ai-goal-tracker-template.git
```

### 5. Initialize goal document (if empty template)

If goal.md still contains all the template placeholders (no real content):
- Ask the user to define:
  - **Intent**: One sentence describing what the project is about
  - **Why**: Why does this matter? What problem does it solve?
  - **Desired End State**: What does "done" look like?
  - **Success Criteria**: How do we know we succeeded?
  - **Relevant Context**: Any key facts or dependencies
- Fill these in collaboratively with the user based on their answers

If goal.md already has real content, skip this step.

### 6. Initialize/update steps and current

- Create initial steps in steps.md based on the defined goal (if it's empty)
- Review existing steps and mark any completed ones
- Set current.md to document the current state with:
  - Last Update: current timestamp
  - What Changed: summary of recent progress
  - Current Focus: what is being worked on right now
  - Blockers: any issues blocking progress
  - Next Actions: concrete next steps ordered by priority

## Files

The repo contains (https://github.com/joschemd/ai-goal-tracker-template):
- `templates/goal.md` — Goal definition template
- `templates/steps.md` — Step tracking with status markers ([ ], [~], [x], [-], [!])
- `templates/current.md` — Current status, blockers, and next actions

## Notes

- The repo is public, so no authentication is required for cloning
- Use `git pull` within an existing clone to get template updates
- If git is not installed, use `winget install GitHub.cli` or download manually
- The agent should keep steps.md and current.md up to date as work progresses
- This skill self-initializes when needed — no user prompting required for template pull
- When files already exist and have real content, the agent updates them rather than overwriting
