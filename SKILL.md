---
name: setup-memory
description: Use this skill whenever the user wants to add a memory system to a project, says "set up memory", "initialize memory", "add memory to this project", "I want Claude to remember things across sessions", "set up the memory folder", "deploy the memory system", or is starting a new project and wants persistent memory. Also trigger proactively when a user mentions being frustrated that Claude forgets things between sessions. This skill creates everything needed in one shot — the Memory/ folder, the memory-keeper agent, and the SessionStart hook — so Claude can remember what was built and decided across every future session in this project.
---

# Setup Memory

Your job is to deploy a complete memory system into the current project. Once set up, Claude will automatically load project context at the start of every session and save it at the end.

## Step 1 — Ask one question first

Before creating anything, ask:

> "What is this project about? And what's the starting point — what do you want to build first?"

Wait for both answers. You'll use them to seed `current-state.md` with real content instead of placeholders.

## Step 2 — Safety check

Look for these in the project root before writing anything:
- `Memory/` folder
- `.claude/agents/memory-keeper.md`
- A `SessionStart` hook inside `.claude/settings.json`

If any already exist, tell the user what you found and ask if they want to continue. Never overwrite without permission — the existing files might have real session history in them.

## Step 3 — Create the files

Templates are at `~/.claude/skills/setup-memory/templates/`. Read each one and use it exactly. Do not write template content from scratch.

**Note:** The templates/ folder may contain extra files (about-me.md, project-decisions.md, session-log.md) left over from an older version. Ignore them — only use the three listed below.

### Memory/ folder

Create these 2 files:

**`Memory/README.md`** ← copy `templates/memory-readme.md` exactly, then add this line right after the title:
```
**Project:** [user's answer from Step 1]
```

**`Memory/current-state.md`** ← copy `templates/current-state.md`, then fill in ALL fields using the user's answers:
- `**Project:**` → user's project description
- `**Status:**` → "Project just initialized. No work done yet."
- `**Last session:**` → today's date
- `**What we did:**` → "(nothing yet — this is the first session)"
- `**Key decisions:**` → "(none yet)"
- `**About me:**` → "(Claude will update this as patterns emerge)"
- `**Next step:**` → user's stated starting point from Step 1

### Memory-keeper agent

Create `.claude/agents/` if it doesn't exist, then create `.claude/agents/memory-keeper.md` from `templates/memory-keeper-agent.md` exactly as written.

### SessionStart and SessionEnd hooks

Add both hooks to `.claude/settings.json`. If the file already exists, merge carefully — preserve every existing setting:

```json
{
  "hooks": {
    "SessionStart": [
      {
        "hooks": [
          {
            "type": "command",
            "command": "python3 -c \"import json,os; f='Memory/current-state.md'; ns=os.path.exists('Memory/.needs-save'); ns and os.remove('Memory/.needs-save'); c=open(f).read() if os.path.exists(f) else None; w='\\nNote: The previous session ended without saving — memory may be incomplete for that session.' if ns else ''; msg=('PROJECT MEMORY LOADED — read this carefully before responding:\\n\\n'+c+w+'\\n\\nWhen the user signals they are wrapping up (says done, see you tomorrow, going to clear, update memory, wrap up, or similar), run the end-session skill immediately. Do not wait to be asked.') if c else 'Memory/current-state.md not found. If this project should have memory, offer to run the setup-memory skill.'; print(json.dumps({'continue':True,'suppressOutput':True,'hookSpecificOutput':{'hookEventName':'SessionStart','additionalContext':msg}}))\""
          }
        ]
      }
    ],
    "SessionEnd": [
      {
        "hooks": [
          {
            "type": "command",
            "command": "python3 -c \"import os; os.makedirs('Memory', exist_ok=True); open('Memory/.needs-save', 'w').write('1')\" 2>/dev/null; true"
          }
        ]
      }
    ]
  }
}
```

## Step 4 — Confirm

Tell the user what was set up in plain language — one sentence per item. Keep it short. Then say: "Start a new session in this project and the memory system will be active."
