---
name: browsing
description: Browser automation for any web task. Use when navigating websites, checking UI, or automating workflows.
---

# Browsing

**Always use a subagent** for browser tasks to keep main context clean. Browser state persists across subagent calls, so login/session carries over.

```
Task tool config:
  subagent_type: general-purpose
  model: haiku
```

Two MCP servers:
- **browsing** — headed, persistent (user watches browser, state persists)
- **browsing-headless** — headless, isolated (faster, clean state)

## Speed

- **Use `browser_fill_form`** for multiple fields instead of typing one at a time
- **Batch independent actions** in single tool calls
- **Never use `browser_wait_for` with time** unless explicitly needed (e.g., animations)

```
# Good: Fill form in one call
browser_fill_form with all fields

# Bad: Type each field separately
browser_type field1
browser_type field2
browser_type field3
```

## Screenshots vs Snapshots

| Use | When |
|-----|------|
| Screenshot | Checking visuals, verifying layout (saves tokens) |
| Snapshot | Need element refs to interact |

## Workflows

**Check UI rendering:** Navigate → screenshot → analyze

**Fill and submit form:** Snapshot (get refs) → `browser_fill_form` → click submit → screenshot (verify)

**Debug visual issues:** Full-page screenshot → element screenshot if needed → check console messages
