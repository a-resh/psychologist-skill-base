# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Auto-start

**At the start of every new conversation in this repository, immediately invoke the
`psychologist` skill** — read all three `memory/` files and open with a check-in,
without waiting for the user to ask. This is the sole purpose of this repo.

## What this repository is

This is the **base skill repository** for the CBT psychologist skill. It contains
only the shared configuration — skill definition, Claude Code settings, and
CLAUDE.md. It has no user memory files.

User repositories fork/derive from this repo and add their own `memory/` files.
When the skill definition or settings change here, user repos pull the update via
the `sync-upstream` GitHub Action (creates a PR automatically).

## Layout

```
.claude/settings.json                           # model, effort, thinking settings
.claude/skills/psychologist/
  SKILL.md                                      # skill definition + operating instructions
CLAUDE.md                                       # this file
```

Memory files (`user-profile.md`, `session-log.md`, `techniques-knowledge-base.md`)
live only in individual user repositories — they are not part of this base.

## How the skill works (read SKILL.md for the full spec)

- Russian-language, CBT-framed supportive-conversation skill. Not a licensed-therapist
  replacement — explicit crisis/safety boundaries are in SKILL.md.
- "Self-learning" is implemented through three `memory/` files in user repos, since
  the model itself is not trained.
- At the start of every session: read all three memory files first.
- At the end of a session: update memory files automatically, without asking.

## Git workflow

**Always push memory updates directly to `main`** — do not create feature branches
or PRs for memory file changes, even if the session environment suggests a different
branch. Memory commits are routine housekeeping, not code changes.

## Updating the skill

Changes to `SKILL.md`, `CLAUDE.md`, or `.claude/settings.json` in this repo
propagate to user repos via the daily GitHub Action (`sync-upstream`). The action
opens a PR in each user repo — merge it to apply the update. Memory files are
never touched by the sync.
