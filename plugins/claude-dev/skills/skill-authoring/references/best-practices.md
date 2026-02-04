# Skill Authoring Best Practices

Source: https://platform.claude.com/docs/en/agents-and-tools/agent-skills/best-practices

## Core Principles

### Concise is Key

The context window is shared. Challenge each piece of information:
- "Does Claude need this explanation?"
- "Can I assume Claude knows this?"
- "Does this justify its token cost?"

### Degrees of Freedom

Match specificity to task fragility:

**High freedom** (instructions only):
- Multiple valid approaches
- Decisions depend on context

**Medium freedom** (templates/pseudocode):
- Preferred pattern exists
- Some variation acceptable

**Low freedom** (exact scripts):
- Operations are fragile
- Consistency critical
- Specific sequence required

Think: narrow bridge (low freedom) vs open field (high freedom).

## Naming Conventions

Prefer gerund form (verb + -ing):
- `processing-pdfs`
- `analyzing-spreadsheets`
- `managing-databases`

Acceptable alternatives:
- Noun phrases: `pdf-processing`
- Action-oriented: `process-pdfs`

Avoid:
- Vague: `helper`, `utils`, `tools`
- Generic: `documents`, `data`

## Writing Descriptions

**Always third person.** Description is injected into system prompt.

```yaml
# Good
description: Processes Excel files and generates reports

# Bad
description: I can help you process Excel files
description: You can use this to process Excel files
```

Include:
- What it does
- Specific triggers/contexts
- Key terms for discovery

## Progressive Disclosure Patterns

### Pattern 1: High-level guide with references

```markdown
# PDF Processing

## Quick start
[Basic example]

## Advanced features
- **Form filling**: See [FORMS.md](references/FORMS.md)
- **API reference**: See [REFERENCE.md](references/REFERENCE.md)
```

### Pattern 2: Domain-specific organization

```
skill/
├── SKILL.md
└── references/
    ├── finance.md
    ├── sales.md
    └── product.md
```

### Pattern 3: Conditional details

```markdown
## Basic usage
[Instructions]

**For tracked changes**: See [REDLINING.md](references/REDLINING.md)
```

## Workflows

Break complex operations into steps with checklists:

```markdown
## Processing Workflow

Track progress:
- [ ] Step 1: Analyze input
- [ ] Step 2: Create mapping
- [ ] Step 3: Validate
- [ ] Step 4: Execute
- [ ] Step 5: Verify

**Step 1: Analyze input**
...
```

## Feedback Loops

Common pattern: validate → fix → repeat

```markdown
1. Make changes
2. **Validate immediately**: run validator
3. If validation fails:
   - Review error
   - Fix issue
   - Validate again
4. Only proceed when validation passes
```

## Templates

Provide output templates when format matters:

```markdown
## Report Structure

Use this template:

# [Title]

## Executive Summary
[Overview]

## Key Findings
- Finding 1
- Finding 2

## Recommendations
1. Action item
2. Action item
```

## Anti-Patterns

### Avoid deeply nested references

```markdown
# Bad: Too deep
SKILL.md → advanced.md → details.md

# Good: One level
SKILL.md → advanced.md
SKILL.md → reference.md
SKILL.md → examples.md
```

### Avoid too many options

```markdown
# Bad
You can use pypdf, or pdfplumber, or PyMuPDF, or...

# Good
Use pdfplumber for text extraction.
For scanned PDFs requiring OCR, use pdf2image instead.
```

### Avoid time-sensitive info

```markdown
# Bad
If before August 2025, use old API...

# Good
## Current method
Use v2 API.

## Old patterns
<details>
<summary>Legacy v1 API (deprecated)</summary>
...
</details>
```

### Avoid Windows paths

Always use forward slashes: `references/guide.md`

## Scripts Best Practices

### Solve, don't punt

Handle errors explicitly instead of failing:

```python
# Good
try:
    with open(path) as f:
        return f.read()
except FileNotFoundError:
    print(f"Creating {path}")
    return ''

# Bad
return open(path).read()  # Just fails
```

### Document constants

```python
# Good
REQUEST_TIMEOUT = 30  # HTTP requests typically complete within 30s

# Bad
TIMEOUT = 47  # Why 47?
```

## Checklist

Before sharing a skill:

- [ ] Description is specific with "when to use"
- [ ] SKILL.md under 500 lines
- [ ] No time-sensitive information
- [ ] Consistent terminology
- [ ] File references one level deep
- [ ] Workflows have clear steps
- [ ] Tested with real scenarios
