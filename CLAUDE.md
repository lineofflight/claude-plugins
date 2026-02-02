# Claude Code Plugins

Plugin collection for extending Claude Code.

## Structure

- `plugins/` — Plugin packages
- `.claude/rem/patterns/` — Observed patterns inbox

## Memory System (rem plugin)

- **observe**: Records learnings to patterns inbox
- **consolidate**: Crystallizes patterns into skills or CLAUDE.md
- SessionStart hook loads observe instructions automatically

## Hooks

Hooks in `plugins/*/hooks/hooks.json` extend behavior:
- SessionStart: Load observe skill
- PostToolUse: Random consolidation trigger
