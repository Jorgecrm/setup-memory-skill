# setup-memory — Claude Code Skill

A skill that gives Claude **persistent memory across sessions**. Once installed, Claude automatically reads what happened in past sessions when you start a new one, and saves what happened when you're done — without you having to ask.

---

## The Problem It Solves

Every time you start a new Claude Code session, Claude has no idea what you were working on. You have to re-explain the project, re-explain decisions, and re-explain context. It wastes time and breaks your flow.

**setup-memory fixes this.** It creates a `Memory/` folder in your project that Claude reads at the start of every session and updates at the end.

---

## How It Stays Token-Efficient

At the start of each session, Claude reads **one small file** — `Memory/current-state.md`. It's always up to date and never grows. At the end of the session, the memory-keeper agent **overwrites** it with a fresh snapshot. No diary, no accumulation, no token bloat.

---

## What Gets Created (One-Time Setup)

When you run the skill, it creates:

| Item | What it does |
|------|--------------|
| `Memory/README.md` | Explains the memory system in plain language |
| `Memory/current-state.md` | Single snapshot of where the project stands — overwritten each session |
| `Memory/project-decisions.md` | Every locked design/build decision so you never relitigate them |
| `Memory/about-me.md` | Your preferences and habits Claude notices over time |
| `.claude/agents/memory-keeper.md` | A fast sub-agent that writes to the Memory files |
| `.claude/settings.json` hook | Tells Claude to read current-state.md at session start, save at session end |

---

## How to Set It Up

### Step 1 — Copy the skill into your Claude Code skills folder

Your skills folder is at:
```
~/.claude/skills/
```

Create a new folder inside it called `setup-memory` and copy these files in:
```
~/.claude/skills/setup-memory/
  SKILL.md
  templates/
    about-me.md
    current-state.md
    memory-keeper-agent.md
    memory-readme.md
    project-decisions.md
```

### Step 2 — Open a Claude Code session in your project

Open Claude Code inside the project you want to add memory to.

### Step 3 — Trigger the skill

Just say one of these things:

- `set up memory`
- `add memory to this project`
- `I want Claude to remember things across sessions`
- `initialize memory`

Claude will ask you one question: **"What is this project about?"** — answer it in plain language. Then it sets everything up automatically.

### Step 4 — Start a new session

Close the current session and open a new one. From that point on, memory is active. Claude will read `Memory/current-state.md` at the start of every session.

---

## How Memory Gets Saved

When you're done working, say any of these:
- `done for today`
- `see you tomorrow`
- `update memory`
- `wrap up`

Claude will save the session automatically. You don't have to do anything else.

---

## What the Memory/ Folder Looks Like

```
Memory/
  README.md              ← explains the folder
  current-state.md       ← always up to date, never grows
  project-decisions.md   ← locked decisions
  about-me.md            ← your preferences
```

All files are plain text — you can read and edit them anytime.

---

## Requirements

- [Claude Code](https://claude.ai/code) (the CLI or desktop app)
- A project folder (any project — SaaS, website, trading bot, anything)
- The skill installed in `~/.claude/skills/setup-memory/`

---

## File Structure of This Skill

```
setup-memory/
  SKILL.md                          ← skill definition (Claude reads this)
  templates/
    current-state.md                ← template for Memory/current-state.md
    memory-readme.md                ← template for Memory/README.md
    project-decisions.md            ← template for Memory/project-decisions.md
    about-me.md                     ← template for Memory/about-me.md
    memory-keeper-agent.md          ← template for the memory-keeper sub-agent
```

---

## Created by

Jorge — built with Claude Code.
