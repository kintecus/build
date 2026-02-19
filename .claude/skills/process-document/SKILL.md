---
name: process-document
description: Process a raw document (PDF, text, export) into a structured markdown summary. Only invoke when the user explicitly runs /process-document or asks to process a specific file.
---

# Process Document

Convert a raw document into a structured, searchable markdown summary.

## Usage

```
/process-document projects/{project}/raw/{filename}
```

## Workflow

1. Read the source document
2. Generate a structured markdown summary (see format below)
3. Save to `projects/{project}/docs/` with a matching name (e.g., `raw/spec.pdf` becomes `docs/spec.md`)
4. Update `projects/{project}/docs/INDEX.md` with a one-line description

## Output format

```markdown
# {Document Title}

> Source: raw/{original-filename}
> Processed: {date}

## TL;DR

{2-3 sentence summary of what this document contains and why it matters.}

## Key points

{Organized by topic. Use headings for distinct sections of the source.}

### {Topic 1}

{Relevant details, preserving important specifics: numbers, dates, names, technical details.}

### {Topic 2}

{Continue as needed.}

## Action items / requirements

{If the document contains actionable items, list them here. Skip if not applicable.}

## Open questions / ambiguities

{Anything unclear, contradictory, or missing in the source. Flag it explicitly.}
```

## Guidelines

- Keep summaries under 500 lines. If the source is huge, split into multiple docs by topic.
- Preserve important specifics: numbers, dates, names, technical terms, API endpoints.
- Flag contradictions or ambiguities - don't silently resolve them.
- Put a TL;DR at the top - this is what gets read most often.
- Use the same terminology as the source document for searchability.
- If the PDF is image-based or poorly structured, note any extraction issues.
