# AGENTS.md

This file provides guidance to AI coding agents (Claude Code and others) when working
with code in this repository. Claude Code loads it through `CLAUDE.md`, which only
imports it (`@AGENTS.md`) — edit instructions here, not in `CLAUDE.md`.

## Auto-start

**At the start of every new conversation in this repository, immediately invoke the
`psychologist` skill** — read all three `memory/` files and open with a check-in,
without waiting for the user to ask. This is the sole purpose of this repo.

The skill is defined in `.claude/skills/psychologist/SKILL.md`; its memory files live
next to it in `.claude/skills/psychologist/memory/`. An agent without Claude Code
skills should read `SKILL.md` and follow it directly.

## What this repository is

This is the **base skill repository** for the CBT psychologist skill. It contains
only the shared configuration — skill definition, Claude Code settings, AGENTS.md
(and the CLAUDE.md that imports it), and the `sync-upstream` workflow. It has no
user memory files.

User repositories fork/derive from this repo and add their own `memory/` files.
When the skill definition or settings change here, user repos pull the update via
the `sync-upstream` GitHub Action (creates a PR automatically).

## Layout

```
.claude/settings.json                           # model, effort, thinking settings
.claude/skills/psychologist/
  SKILL.md                                      # skill definition + operating instructions
  memory/                                       # user repos only: profile, log, techniques, sessions/
.github/workflows/sync-upstream.yml             # daily sync of shared files into user repos
AGENTS.md                                       # this file — instructions for coding agents
CLAUDE.md                                       # imports AGENTS.md for Claude Code
```

Memory files (`user-profile.md`, `session-log.md`, `techniques-knowledge-base.md`)
and session debug logs (`memory/sessions/*.json`) live only in individual user
repositories — they are not part of this base.

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

Changes to `AGENTS.md`, `CLAUDE.md`, or anything in `.claude/` in this repo
propagate to user repos via the `sync-upstream` GitHub Action
(`.github/workflows/sync-upstream.yml`). It runs daily in each user repo (or on
demand: Actions → sync-upstream → Run workflow) and opens or updates a PR from the
`sync-upstream` branch — merge it to apply the update. In this base repo the job is
skipped.

The sync never touches memory files (`.claude/skills/psychologist/memory/`) or
`.claude/settings.local.json`. It adds and updates files from this repo but does not
delete files that exist only in a user repo (such as other skills) — except inside
`.claude/skills/psychologist/`, which mirrors this repo apart from `memory/`.

To set up a user repo: copy `.github/workflows/sync-upstream.yml` there and enable
Settings → Actions → General → "Allow GitHub Actions to create and approve pull
requests". The workflow file itself is not synced (the Actions token cannot modify
workflows) — when it changes here, copy it into each user repo by hand.
