---
name: my-skill
description: Manages the skills repository lifecycle: git pull latest changes, create new skills in the current working directory, auto-commit, install via symlinks into Claude Code and Cline skills directories, and git push. Use this skill whenever the user wants to create a new skill, sync skills, install a skill, or push skill changes to the remote repository.
disable-model-invocation: true
argument-hint: [skill-name]
---

# My Skill — Skills Repository Manager

## Overview

This skill manages the full lifecycle of skills in the local repository:
1. Pull latest from remote
2. Create new skills in the current working directory
3. Auto-commit changes
4. Install via symlinks into Claude Code and Cline
5. Push to remote

## Workflow

### Step 1: Pull latest skills from remote

```bash
git -C /home/galbot/Asset_to_Simulation_Sufficient/skills pull origin main
```

If there are conflicts, surface them to the user before proceeding.

### Step 2: Create a new skill in the current working directory

Run the initializer from the skill-creator scripts, targeting the current working directory:

```bash
python3 ~/.claude/skills/skill-creator/scripts/init_skill.py <skill-name> --path /home/galbot/Asset_to_Simulation_Sufficient/skills
```

Then edit the generated `SKILL.md` to fill in the actual content based on what the user described.

The skill is created at:
```
/home/galbot/Asset_to_Simulation_Sufficient/skills/<skill-name>/
```

### Step 3: Auto-commit

Stage and commit the new or modified skill:

```bash
git -C /home/galbot/Asset_to_Simulation_Sufficient/skills add <skill-name>/
git -C /home/galbot/Asset_to_Simulation_Sufficient/skills commit -m "feat(skills): add <skill-name> skill"
```

### Step 4: Install via symlinks

Install the skill into both Claude Code and Cline by creating symlinks:

**Claude Code:**
```bash
ln -sf /home/galbot/Asset_to_Simulation_Sufficient/skills/<skill-name> ~/.claude/skills/<skill-name>
```

**Cline** (check for Cline skills directory first):
```bash
# Detect Cline skills path — common locations:
# ~/.cline/skills/
# ~/.config/cline/skills/
# ~/Library/Application Support/Cline/skills/  (macOS)
CLINE_SKILLS=$(ls -d ~/.cline/skills ~/.config/cline/skills 2>/dev/null | head -1)
if [ -n "$CLINE_SKILLS" ]; then
  ln -sf /home/galbot/Asset_to_Simulation_Sufficient/skills/<skill-name> "$CLINE_SKILLS/<skill-name>"
fi
```

If the Cline skills directory doesn't exist, inform the user and skip that step.

### Step 5: Git push

```bash
git -C /home/galbot/Asset_to_Simulation_Sufficient/skills push origin main
```

## Notes

- Always pull before creating a new skill to avoid conflicts.
- Symlinks point to the source repo, so edits in either location are reflected immediately.
- If the user only wants a subset of these steps (e.g., just install, or just push), execute only the relevant steps.
- The skills repo remote is: `ssh://git@git.galbot.com:6043/jiangjiaxi/skills.git`
