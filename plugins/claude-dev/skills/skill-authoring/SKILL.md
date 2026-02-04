---
name: skill-authoring
description: Creates and improves agent skills. Use when creating, updating, or reviewing skills, or when user asks about SKILL.md format, frontmatter fields, skill structure, or skill authoring best practices.
---

# Creating Agent Skills

## What Is a Skill

A folder with a `SKILL.md` file containing YAML frontmatter and markdown instructions.

```
skill-name/
├── SKILL.md           # Required
├── references/        # Optional: detailed docs
├── scripts/           # Optional: executable code
└── assets/            # Optional: templates, data
```

## SKILL.md Format

```yaml
---
name: skill-name
description: What this does and when to use it.
---

# Instructions

[Markdown body with instructions]
```

### Required Fields

| Field | Constraints |
|-------|-------------|
| `name` | 1-64 chars, lowercase, hyphens ok, must match folder name |
| `description` | 1-1024 chars, third person, include "when to use" |

### Optional Fields

- `license` - License name or reference
- `compatibility` - Environment requirements
- `metadata` - Key-value pairs (author, version)
- `allowed-tools` - Pre-approved tools (experimental)

## Core Principles

### Be Concise

Claude already knows most things. Only add what it doesn't have.

```markdown
# Good (~50 tokens)
## Extract PDF text
Use pdfplumber:
import pdfplumber
with pdfplumber.open("file.pdf") as pdf:
    text = pdf.pages[0].extract_text()

# Bad (~150 tokens)
## Extract PDF text
PDF files are a common format containing text...
To extract text, you'll need a library...
We recommend pdfplumber because...
```

### Set Degrees of Freedom

Match specificity to task fragility:

- **High freedom** (text instructions): Multiple valid approaches, context-dependent
- **Medium freedom** (templates/pseudocode): Preferred pattern exists, some variation ok
- **Low freedom** (exact scripts): Fragile operations, consistency critical

### Progressive Disclosure

1. **Metadata** (~100 tokens): Loaded at startup for all skills
2. **SKILL.md body** (<5000 tokens): Loaded when skill activates
3. **Reference files**: Loaded only when needed

Keep SKILL.md under 500 lines. Move details to `references/`.

### Naming

Lowercase, hyphens ok, 1-64 chars. Must match folder name.

Prefer gerund form: `processing-pdfs`, `analyzing-data`. Avoid vague names like `helper` or `utils`.

## Writing Descriptions

Third person. Specific. Include triggers.

```yaml
# Good
description: Extracts text from PDFs, fills forms, merges documents. Use when working with PDF files or document extraction.

# Bad
description: Helps with PDFs.
```

## Workflows

For multi-step tasks with validation points, provide checklists with feedback loops:

```markdown
## Form Processing Workflow

Copy and track progress:
- [ ] Step 1: Analyze form structure
- [ ] Step 2: Create field mapping
- [ ] Step 3: Validate mapping
- [ ] Step 4: Apply changes
- [ ] Step 5: Verify output
```

Include feedback loops: validate → fix → repeat.

## Anti-Patterns

- Deeply nested references (keep one level deep from SKILL.md)
- Too many options (provide defaults with escape hatches)
- Time-sensitive info (use "old patterns" sections instead)
- Windows paths (always use forward slashes)
- Verbose explanations of things Claude knows

## Reference

- [Specification](references/specification.md) - Complete format details
- [Best Practices](references/best-practices.md) - Comprehensive authoring guide
