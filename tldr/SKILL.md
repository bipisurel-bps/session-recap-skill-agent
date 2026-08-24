---
name: tldr
description: "Generate a quick recap of the current session. Two parts: completed work and current task status. Trigger: user says 'tldr', 'recap', 'summarize', 'where are we', 'udah ngapain aja', or /tldr."
version: 1.1.0
metadata:
  hermes:
    tags: [session, recap, tldr, summary, productivity]
---

# TLDR — Session Recap

Generate a quick session recap. Format is concise, scannable, and straight to the point.

## When to Load

Load this skill when the user says:
- "tldr", "recap", "summarize"
- "where are we", "udah ngapain aja"
- "ringkasan", "resume", "status dong"
- `/tldr`

**Hermes:** Load via `skill_view(name='tldr')` — Hermes does not auto-trigger skills by keyword. The agent must explicitly load the skill when it recognizes the user's intent.

**OpenClaw:** If your OpenClaw instance supports keyword-based skill activation, configure the trigger words above. Otherwise, load manually.

## Tone

- **Casual but informative** — not an essay, not a robot
- **Short, straight to the point** — people read recaps to get up to speed fast, not to read a novel
- **Mix of English and local language** — match the conversation style, not formal
- Stay professional on technical details (file paths, commit hashes, commands — write them as-is)

## Output Format

```
## Done
- <action> <object>. <result>.
- ...

## Current task
- Task: <name>
- Status: <complete | in progress | blocked>
- Last step: <what was done last>
- Next step: <what comes next>
- Blocker: <why blocked> (only if status is blocked)
```

## Rules

### Done (completed work)
- **Max 5 bullets.** One line per bullet.
- **Max 15 words per bullet.**
- **Group related items** — don't make one bullet per commit. If 3 commits fix the same thing, combine into one bullet.
- **Write the result, not the process.** "Fixed prayer times bug" not "I checked the log, found an error on line 42, then I changed..."
- **Include references** when available:
  - Commit: [`0d43184`](https://github.com/org/repo/commit/0d43184)
  - PR: [#446](https://github.com/org/repo/pull/446)
  - File: [`services/prayerTimes.ts`](services/prayerTimes.ts)
  - File + line: [`services/prayerTimes.ts:42`](services/prayerTimes.ts#L42)
  - If you don't know the URL, write the plain identifier. Don't fabricate URLs.

### Current task (latest task)
- **Max 5 fields** (Task, Status, Last step, Next step, Blocker)
- One line per field
- Blocker only written if status is `blocked`

### Hard limits
- **Total max 20 lines** (Done + Current task combined)
- If too many, keep the **highest impact** items only
- No tables, sub-bullets, or code blocks in the recap
- Strip tool call narration — only write the outcome

### Edge cases
- If no work completed: write **"Nothing completed yet."** — don't make things up
- If session just started: write **"New session, no work done yet."**
- Recap is for the **current session only**, not a combination of all sessions

## Example

```
## Done
- Fixed duplicate adzan notifications. Debounce guard 4 seconds. [`a30199c`](https://github.com/bipisurel-bps/Jadda-android-apps/commit/a30199c)
- Updated doa100.json translations. 100 doas, 0 duplicates.
- Built APK v1.3.0-vc15. [Download](https://gofile.io/d/CEvFA4)

## Current task
- Task: Deploy to Play Store
- Status: blocked
- Last step: APK built, uploaded to gofile
- Next step: Upload AAB to Google Play Console
- Blocker: EAS free tier limit exhausted, waiting for reset
```
