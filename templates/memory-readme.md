# Memory

This folder is the brain of this project. It keeps Claude up to speed across sessions — automatically.

**You own this folder.** It's plain text — open it anytime.

## What's inside

| File | What it holds |
|------|---------------|
| `current-state.md` | Everything Claude needs to know: project status, what happened last session, locked decisions, your work preferences, and what's next. Overwritten each session — never grows. |

## How it works

At the start of each session, Claude reads `current-state.md` — one small file, always up to date.
At the end of each session, Claude overwrites it with a fresh snapshot.

You never have to ask. It just happens.
