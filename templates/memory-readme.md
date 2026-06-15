# Memory

This folder is the brain of this project. It keeps track of everything important across sessions so Claude never starts from scratch.

**You own this folder.** It's written in plain language — open any file anytime.

## What's inside

| File | What it holds |
|------|---------------|
| `current-state.md` | A single snapshot of where the project stands right now — overwritten each session, never grows |
| `about-me.md` | Your preferences, habits, and patterns Claude notices |
| `project-decisions.md` | Every design and build decision that gets locked in |

## How it works

At the start of each session, Claude reads `current-state.md` — one small file, always up to date.
At the end of each session, Claude overwrites it with a fresh snapshot of what happened and what's next.

You never have to ask. It just happens.
