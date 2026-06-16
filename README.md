# setup-memory — Claude Code Skill

A skill that gives Claude **persistent memory across sessions**. Once installed, Claude automatically loads project context at the start of every session and saves it at the end — without you having to ask.

---

## The Problem It Solves

Every time you start a new Claude Code session, Claude has no idea what you were working on. You have to re-explain the project, re-explain decisions, and re-explain context. It wastes time and breaks your flow.

**setup-memory fixes this.** It creates a `Memory/` folder in your project that Claude reads at the start of every session and updates at the end.

---

## How It Works

- **Session start:** A hook runs automatically and injects the contents of `Memory/current-state.md` directly into Claude's context — no file read required, guaranteed to load.
- **Session end:** When you say "done", Claude runs the end-session skill → session-handoff → memory-keeper rewrites `current-state.md` with a fresh snapshot.
- **If you close without saying done:** A `SessionEnd` hook writes a marker file. Next session, Claude is warned that the previous session wasn't saved.

One file, always fresh, never grows.

---

## What Gets Created (One-Time Setup)

| Item | What it does |
|------|--------------|
| `Memory/README.md` | Explains the memory system |
| `Memory/current-state.md` | Single snapshot — status, decisions, preferences, next step. Overwritten each session. |
| `.claude/agents/memory-keeper.md` | Sub-agent that rewrites current-state.md at session end |
| `.claude/settings.json` hooks | SessionStart: injects memory into context. SessionEnd: writes crash-guard marker. |

---

## How to Set It Up

### Step 1 — Copy the skill into your Claude Code skills folder

```
~/.claude/skills/setup-memory/
  SKILL.md
  templates/
    current-state.md
    memory-keeper-agent.md
    memory-readme.md
```

### Step 2 — Open Claude Code in your project and say:

- `set up memory`
- `add memory to this project`
- `I want Claude to remember things across sessions`

Claude asks two questions: what the project is, and what you want to build first. Then sets everything up.

### Step 3 — Start a new session

From that point on, memory is active automatically.

---

## How Memory Gets Saved

Say any of these when you're done:
- `done for today`
- `see you tomorrow`
- `update memory`
- `wrap up`

Claude saves the session automatically. If you close without saying done, the next session will warn you that memory from the previous session may be missing.

---

## What current-state.md Looks Like

```
# Current State

**Project:** My Forex trading journal SaaS
**Status:** Login page built, now building the dashboard
**Last session:** June 15, 2026

**What we did:**
- Built login page with Supabase auth
- Connected frontend to Supabase
- Fixed redirect bug after login

**Key decisions:**
- Using Supabase for auth (not Firebase)
- Next.js for the frontend

**About me:**
- Explain things in plain language, no jargon
- Always tell me what to do next

**Next step:** Build the dashboard main view
```

---

## Requirements

- [Claude Code](https://claude.ai/code)
- A project folder
- The skill installed in `~/.claude/skills/setup-memory/`
- Also install the `session-handoff` and `end-session` skills — the save chain depends on them

---

## Created by

Jorge — built with Claude Code.
