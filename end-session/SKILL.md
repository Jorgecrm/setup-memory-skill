---
name: end-session
description: Use this skill whenever the user signals they are wrapping up or ending their session — any phrasing like "I'm done", "done for today", "see you tomorrow", "going to clear", "update my memory", "wrap up", "heading out", "let's stop here", "save this session", "I gotta go", or similar. Also trigger if the user says /clear or mentions clearing context. This skill automates the full end-of-session memory workflow without the user having to manage it themselves. Always run this proactively the moment you detect a session-ending signal — do not wait for the user to ask twice or spell it out.
---

# End Session

Your job is to make sure nothing important gets lost when the user ends a session. Do two things in sequence, without stopping to ask for permission.

## Why this matters

When context is cleared, everything in this conversation disappears. Running this skill means the next session starts with accurate memory of what was built and decided, instead of starting from scratch or relying on the user to remember.

## Step 1 — Produce the session summary

Invoke the `session-handoff` skill. Let it run fully and produce its structured summary in the chat. That summary is the source of truth for what happened this session — don't abbreviate it or skip sections.

## Step 2 — Call the memory-keeper agent

Pass the complete session-handoff output to the `memory-keeper` subagent for this project. Include today's date so it can log the session correctly.

**If this project has no memory-keeper agent:** tell the user in one sentence, then show them the session-handoff summary so they can save it somewhere manually. Do not fail silently.

## Finish

Once the memory-keeper confirms it's done, tell the user in one plain sentence what was saved. Example: "Done — logged today's session and saved the layout decision to your memory files."

No extra commentary. Just confirm and stop.
