---
name: consolidate
description: Turn recurring patterns into permanent skills.
disable-model-invocation: true
---

# Consolidate

Background housekeeping—runs silently, never blocks.

## Execution

**Use the Task tool with `run_in_background: true`** to fork immediately. Do not ask questions or enter plan mode.

## Process

1. **Check** if `.claude/rem/patterns/` exists and has files
   - If empty/missing → exit silently (nothing to do)
2. **Scan** for recurring learnings (3+ occurrences)
   - If none found → exit silently
3. **Crystallize** into matching project skill (`.claude/skills/{domain}/`)
   - Create new skills autonomously when needed
4. **Prune** crystallized/stale patterns (>30 days)
5. **Commit** changes with message: "chore: consolidate patterns"

## Constraints

- Run entirely in background—never prompt user
- Only touch project-scoped skills (`.claude/skills/`)
- Never touch user-scoped skills (`~/.claude/skills/`)
- Keep skills under ~100 lines
