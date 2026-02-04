# Agent Skills Specification

Source: https://agentskills.io/specification

## Directory Structure

```
skill-name/
├── SKILL.md          # Required
├── scripts/          # Optional: executable code
├── references/       # Optional: documentation
└── assets/           # Optional: templates, resources
```

## Frontmatter Fields

### name (required)

- 1-64 characters
- Lowercase alphanumeric and hyphens only
- Cannot start/end with hyphen
- No consecutive hyphens
- Must match parent directory name

```yaml
# Valid
name: pdf-processing
name: data-analysis

# Invalid
name: PDF-Processing  # uppercase
name: -pdf            # starts with hyphen
name: pdf--proc       # consecutive hyphens
```

### description (required)

- 1-1024 characters
- Describe what it does AND when to use it
- Include keywords for discovery

```yaml
description: Extracts text and tables from PDF files, fills forms, merges documents. Use when working with PDF files or when the user mentions PDFs, forms, or document extraction.
```

### license (optional)

```yaml
license: Apache-2.0
license: Proprietary. LICENSE.txt has complete terms
```

### compatibility (optional)

- 1-500 characters
- Environment requirements only

```yaml
compatibility: Designed for Claude Code
compatibility: Requires git, docker, jq, and internet access
```

### metadata (optional)

Key-value pairs for additional properties.

```yaml
metadata:
  author: example-org
  version: "1.0"
```

### allowed-tools (optional, experimental)

Space-delimited list of pre-approved tools.

```yaml
allowed-tools: Bash(git:*) Bash(jq:*) Read
```

## Optional Directories

### scripts/

Executable code. Should be:
- Self-contained or document dependencies
- Include helpful error messages
- Handle edge cases

### references/

Additional documentation loaded on demand:
- REFERENCE.md - Technical reference
- Domain-specific files (finance.md, legal.md)

Keep files focused. Smaller = less context usage.

### assets/

Static resources:
- Templates
- Images/diagrams
- Data files, schemas

## Progressive Disclosure

| Level | Tokens | When Loaded |
|-------|--------|-------------|
| Metadata | ~100 | Startup (all skills) |
| SKILL.md body | <5000 | Skill activation |
| Reference files | As needed | When required |

## File References

Use relative paths from skill root:

```markdown
See [the reference](references/REFERENCE.md) for details.
Run: scripts/extract.py
```

Keep references one level deep. Avoid nested chains.

## Validation

```bash
skills-ref validate ./my-skill
```
