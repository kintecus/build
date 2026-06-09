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

## Transcript detection

Detect call transcripts via two signals:

1. **Path**: folder name contains `call-transcripts` OR filename contains `recording`, `transcript`, `sync`, `meeting`, `call`
2. **Content**: 3+ unique speaker labels in first 50 lines, in either format:
   - `**Speaker Name**` (newer .md transcripts)
   - `Speaker Name  0:00` (older .txt with timestamps)

Either signal is sufficient. Both together = high confidence.

## Workflow

1. Read the source document
2. **If transcript detected**: run the transcript cleaning sub-step (see below), then use the cleaned file as input for step 3
3. Generate a structured markdown summary (see format below)
4. Save to `projects/{project}/docs/` with a matching name (e.g., `raw/spec.pdf` becomes `docs/spec.md`)
5. Update `projects/{project}/docs/INDEX.md` with a one-line description
6. **If transcript was cleaned**: include both raw and cleaned source paths in the summary header

## Transcript cleaning

When a transcript is detected, before summarizing:

1. Read [transcript-cleaning.md](references/transcript-cleaning.md) (relative to this skill) for the full cleaning instructions
2. Use the `Agent` tool to spawn a cleaning sub-agent:
   - `model: "sonnet"`
   - `subagent_type: "general-purpose"`
   - Prompt must include:
     - The cleaning instructions from the reference file
     - The full raw transcript content
     - Paths to project context files for the sub-agent to read: `projects/{project}/docs/transcript-context.md`, `projects/{project}/docs/glossary.md`, `projects/{project}/BRIEF.md`
     - The original filename (for the `source` frontmatter field)
   - The sub-agent reads context files, applies ASR corrections, and returns the cleaned transcript
   - For non-English transcripts, the sub-agent also translates to English (per the cleaning instructions)
3. Save the sub-agent's output as `{original_path}/{original_name}_cleaned.md` (same folder as the raw file)
4. Proceed with the cleaned transcript as input to the summary step

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

For cleaned transcripts, the header becomes:

```markdown
> Source: raw/{original-filename}
> Cleaned: raw/{cleaned-filename}
> Processed: {date}
```

## Guidelines

- Keep summaries under 500 lines. If the source is huge, split into multiple docs by topic.
- Preserve important specifics: numbers, dates, names, technical terms, API endpoints.
- Flag contradictions or ambiguities - don't silently resolve them.
- Put a TL;DR at the top - this is what gets read most often.
- Use the same terminology as the source document for searchability.
- If the PDF is image-based or poorly structured, note any extraction issues.
