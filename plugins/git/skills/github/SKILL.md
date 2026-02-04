---
name: github
description: Use when interacting with GitHub (issues, PRs, projects, repo exploration)
compatibility: Requires gh CLI
---

# GitHub Skill

Use `gh` CLI to interact with GitHub repositories.

## Key Patterns

- `gh api repos/owner/repo/contents/path` — read files from any repo
- `gh api repos/owner/repo/issues/N/comments` — read issue discussions
- `gh repo view owner/repo` — README and metadata
- Add `--repo owner/repo` to any command for third-party repos
- When creating gists with markdown, use `.md` extension (e.g., `gh gist create README.md`) for proper rendering
