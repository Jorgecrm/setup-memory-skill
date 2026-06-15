---
name: setup-memory
description: Use this skill whenever the user wants to add a memory system to a project, says "set up memory", "initialize memory", "add memory to this project", "I want Claude to remember things across sessions", "set up the memory folder", "deploy the memory system", or is starting a new project and wants persistent memory. Also trigger proactively when a user mentions being frustrated that Claude forgets things between sessions. This skill creates everything needed in one shot — the Memory/ folder, the memory-keeper agent, and the SessionStart hook — so Claude can remember what was built and decided across every future session in this project.
---

# Setup Memory

Your job is to deploy a complete memory system into the current project. Once set up, Claude will read what happened in past sessions at the start of every new session, and save what happens at the end — automatically.

## Step 1 — Ask one question first

Before creating anything, ask:

> "What is this project about?"

Wait for the answer. You'll use it to seed the README so the memory files have useful context from day one.

## Step 2 — Safety check

Look for these in the project root before writing anything:
- `Memory/` folder
- `.claude/agents/memory-keeper.md`
- A `SessionStart` hook inside `.claude/settings.json`

If any already exist, tell the user what you found and ask if they want to continue. Never overwrite without permission — the existing files might have real session history in them.

## Step 3 — Create the files

Read each template from this skill's `templates/` folder and use it exactly. Do not write the content from scratch.

### Memory/ folder

Create these 4 files from their templates:
- `Memory/README.md` ← `templates/memory-readme.md` — after copying, add one line under the title: `**Project:** [user's answer from Step 1]`
- `Memory/current-state.md` ← `templates/current-state.md` — after copying, fill in the `**Project:**` line with the user's answer from Step 1
- `Memory/project-decisions.md` ← `templates/project-decisions.md`
- `Memory/about-me.md` ← `templates/about-me.md`

### Memory-keeper agent

Create `.claude/agents/` if it doesn't exist, then create `.claude/agents/memory-keeper.md` from `templates/memory-keeper-agent.md` exactly as written.

### SessionStart hook

Add this hook to `.claude/settings.json`. If the file already exists, merge carefully — preserve every existing setting and just add the hook:

```json
{
  "hooks": {
    "SessionStart": [
      {
        "hooks": [
          {
            "type": "command",
            "command": "printf '%s' '{\"continue\":true,\"suppressOutput\":true,\"hookSpecificOutput\":{\"hookEventName\":\"SessionStart\",\"additionalContext\":\"MEMORY RULE — mandatory this session: Read Memory/current-state.md at the start of this session to get up to speed on where the project stands. That single file is all you need — it is always up to date. When the user signals they are wrapping up (says done, see you tomorrow, going to clear, update memory, wrap up, or similar), run the end-session skill to save the session. Do not wait to be asked.\"}}'" ,
            "statusMessage": "Loading memory rules..."
          }
        ]
      }
    ]
  }
}
```

## Step 4 — Confirm

Tell the user what was set up in plain language — one sentence per item. Keep it short. Then tell them: "Start a new session in this project and the memory system will be active."
