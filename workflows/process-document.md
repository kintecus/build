# Workflow: Process Raw Document

## Usage

```
# Tell CC: "Process the document projects/{project}/raw/{filename} into docs/"
```

## What this workflow does

1. Read the raw document (PDF, text, export)
2. Generate a structured markdown summary with:
   - TL;DR (2-3 sentences)
   - Key points organized by topic
   - Action items or requirements (if any)
   - Open questions or ambiguities found in the doc
3. Save to docs/ with matching name (e.g., raw/spec.pdf → docs/spec.md)
4. Update docs/INDEX.md with one-line description

## Notes

- Preserve important specifics (numbers, dates, names, technical details)
- Flag anything that seems contradictory or unclear in the source
- Keep summaries under 500 lines. If the source is huge, split into multiple docs by topic.
