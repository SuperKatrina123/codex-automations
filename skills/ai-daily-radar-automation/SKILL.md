---
name: ai-daily-radar-automation
description: Use when the user asks to install, create, set up, or update an AI Daily Radar Codex automation/workflow from the codex-automations repo. Handles preflight checks for an information-fetching skill, defaulting to aihot / AI HOT, Gmail delivery, recipient and label placeholders, weekday scheduling, and Codex automation creation.
---

# AI Daily Radar Automation

This skill installs or updates a Codex automation that turns recent AI information sources into an actionable daily radar email.

## Core Rules

- Do not bind the automation to a local project unless the user explicitly asks.
- Do not hard-code private email addresses into repository files or reusable prompts.
- Default source skill is `aihot` / AI HOT, but the user may replace it with another information-fetching skill.
- If no suitable source skill is available, stop and tell the user what to install or replace. Do not generate reports from model memory.
- Gmail delivery requires a connected Gmail connector/app. It does not require a Chrome extension.
- Daily report archiving can push the final Markdown report to `SuperKatrina123/codex-automations` under `examples/` when Git credentials are available.

## Installation Workflow

1. Read [references/prompt.md](references/prompt.md).
2. Confirm source configuration:
   - Default `SOURCE_SKILL`: `aihot`
   - Default `SOURCE_NAME`: `AI HOT`
   - If the user wants another source, replace both values and follow that skill's instructions.
3. Preflight source skill:
   - If `SOURCE_SKILL` is available, continue.
   - If missing, ask the user to install/enable it or choose another information-fetching skill.
4. Preflight Gmail:
   - If Gmail connector tools are available, confirm the connected profile when useful.
   - If Gmail is unavailable, tell the user to connect Gmail before enabling email delivery.
5. Collect delivery variables if not already provided:
   - `REPORT_RECIPIENT_EMAIL`
   - `GMAIL_LABEL_NAME` (default: `AI Daily Radar`)
6. Confirm archive variables:
   - `GITHUB_ARCHIVE_REPO`: default `https://github.com/SuperKatrina123/codex-automations.git`
   - `GITHUB_ARCHIVE_BRANCH`: default `main`
   - `GITHUB_ARCHIVE_PATH`: default `examples/ai-daily-radar-YYYY-MM-DD.md`
   - If Git push is unavailable, the automation should still send Gmail and report the archive failure.
7. Create or update the Codex automation:
   - Name: `AI Daily Radar`
   - Kind: `heartbeat`
   - Destination: current thread
   - Schedule: weekdays at 10:30 in `Asia/Shanghai`, unless the user requests another schedule.
   - Prompt: the template from `references/prompt.md`, with placeholders filled in.
8. Tell the user what was installed and what still needs manual connection, if anything.

## Default Schedule

```text
DTSTART;TZID=Asia/Shanghai:20260513T103000
RRULE:FREQ=WEEKLY;BYDAY=MO,TU,WE,TH,FR
```

## Placeholder Policy

Keep placeholders in reusable repo files:

- `SOURCE_SKILL`
- `SOURCE_NAME`
- `REPORT_RECIPIENT_EMAIL`
- `GMAIL_LABEL_NAME`
- `GITHUB_ARCHIVE_REPO`
- `GITHUB_ARCHIVE_BRANCH`
- `GITHUB_ARCHIVE_PATH`

Only fill them inside the actual Codex automation prompt after the user provides or confirms the values.
