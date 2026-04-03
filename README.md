# Kindling

Open source skills for [Claude Code](https://docs.anthropic.com/en/docs/claude-code). Drop them in your skills directory and they just work.

Each skill is a markdown file that teaches Claude Code how to approach a specific type of task — no dependencies, no installs, no config.

## Skills

### Cascading Insights
Portfolio-level intelligence sweep across all your repos. Scans every repo, classifies status (Active/Warm/Stalled/Shipped/Dead), finds cross-project patterns, and recommends what to work on next.

### Drive Architect
Audit any drive's structure. Maps directories with sizes, classifies everything, diagnoses space waste and structural problems, proposes a clean layout, and generates an execution plan.

### YouTube Forge
Local-first devlog production pipeline. $0/video. Scout your content, capture screen recordings, transcribe with Whisper, scene map the edit, and ship consistent devlogs without paid tools.

## Installation

Copy any skill folder into your Claude Code skills directory:

```bash
# Individual skill
cp -r cascading-insights ~/.claude/skills/

# All skills
cp -r */ ~/.claude/skills/
```

Restart Claude Code. Skills auto-detect based on your prompts.

## Usage

Just ask naturally:

- "What's the state of all my projects?"
- "This drive is a mess, help me organize it"
- "I want to make a devlog"

Or invoke directly: `/cascading-insights`, `/drive-architect`, `/youtube-forge`

## Contributing

PRs welcome. Each skill lives in its own folder with a single `SKILL.md`. Keep skills focused, generic, and dependency-free.

## License

MIT
