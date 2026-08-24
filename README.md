# Session Recap Skill

A skill that generates quick, scannable recaps of the current session. Two parts: what's done and what's next.

## What It Does

When triggered, the agent produces a structured recap:

```
## Done
- Fixed duplicate adzan notifications. Debounce guard 4 seconds. [`a30199c`](...)
- Updated doa100.json translations. 100 doas, 0 duplicates.

## Current task
- Task: Deploy to Play Store
- Status: blocked
- Last step: APK built, uploaded to gofile
- Next step: Upload AAB to Google Play Console
- Blocker: EAS free tier limit exhausted
```

**Constraints:** Max 20 lines, 5 bullets for Done (15 words each), 5 fields for Current task. No tables, sub-bullets, or code blocks. Outcome only, no process narration.

## Trigger Words

The skill activates when the user says any of:

| Trigger | Language |
|---------|----------|
| `tldr`, `recap`, `summarize` | English |
| `where are we` | English |
| `udah ngapain aja` | Indonesian |
| `ringkasan`, `resume`, `status dong` | Indonesian |
| `/tldr` | Slash command |

---

## Installation

### Hermes Agent

Hermes uses `~/.hermes/skills/` with category subdirectories.

```bash
# Option 1: Copy directly
cp -r tldr/ ~/.hermes/skills/productivity/tldr/

# Option 2: Clone and copy
git clone https://github.com/bipisurel-bps/session-recap-skill-agent.git /tmp/session-recap-skill-agent
mkdir -p ~/.hermes/skills/productivity/
cp -r /tmp/session-recap-skill-agent/tldr/ ~/.hermes/skills/productivity/tldr/
```

**How Hermes loads it:**

Hermes does **not** auto-trigger skills by keyword. The agent loads skills explicitly when it recognizes relevance. To use:

1. Say "tldr" or "recap" in a session.
2. The agent should call `skill_view(name='tldr')` to load the skill instructions.
3. The agent follows the skill's format to generate the recap.

To make the agent always recognize "tldr" as a trigger, add to your `.hermes.md` or `SOUL.md`:

```markdown
When the user says "tldr", "recap", "summarize", or "where are we", load the `tldr` skill and generate a session recap.
```

### OpenClaw

OpenClaw uses `~/.openclaw/workspace/skills/` or `~/.agents/skills/`.

```bash
# Copy the skill folder
cp -r tldr/ ~/.openclaw/workspace/skills/tldr/

# Or via agents directory
cp -r tldr/ ~/.agents/skills/tldr/
```

**How OpenClaw loads it:**

If your OpenClaw instance supports keyword-based skill activation, configure the trigger words listed above in your OpenClaw config. Otherwise, the agent will load the skill when it recognizes the user's intent.

### Other Agents

Any agent that reads `SKILL.md` files with YAML frontmatter can use this skill. Copy the `tldr/` folder to your agent's skill directory.

---

## File Structure

```
session-recap-skill-agent/
├── README.md           ← This file
└── tldr/
    └── SKILL.md        ← The skill definition
```

## Customization

Edit `tldr/SKILL.md` to adjust:

- **Tone** — change the "Tone" section to match your preferred style
- **Limits** — adjust max lines, bullets, words per bullet
- **Format** — modify the output template
- **Triggers** — add/remove trigger words in the "When to Load" section
- **Language** — the default is English with room for local language mixing

## License

Free to use and modify.
