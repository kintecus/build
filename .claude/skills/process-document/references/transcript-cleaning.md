# Transcript cleaning - sub-agent instructions

## Purpose

Fix ASR (automatic speech recognition) errors in a call transcript. Do not summarize, restructure, or editorialize. Preserve the original speaker-turn structure exactly.

## Context files to read

Before cleaning, read these project context files (skip any that don't exist):

1. `projects/{project}/docs/transcript-context.md` - ASR corrections and participant lists
2. `projects/{project}/docs/glossary.md` - Domain terms and definitions
3. `projects/{project}/BRIEF.md` - Stakeholders section for people/company names

Use these files to inform corrections. The transcript-context.md file has the highest-priority correction patterns.

## What to fix (priority order)

1. **Proper names** - People, companies, products. Use the ASR misrecognition table from transcript-context.md as the primary reference.
2. **Technical terms** - Domain jargon garbled by ASR. Cross-reference with glossary.md.
3. **"Unknown Speaker" resolution** - Infer from conversation context, but only when unambiguous. Mark uncertain resolutions with `[?]` after the speaker name. **Cross-check**: when one speaker addresses another by name in the transcript, use that as ground truth for speaker identification - it overrides contextual inference.
4. **Garbled sentences** - Fix broken grammar where intended meaning is clear from context. Don't rephrase sentences that are already understandable.
5. **Format normalization** - Ensure consistent `**Speaker Name**` bold format for all speaker labels.

## Non-English transcripts

Some transcripts may be in a language other than English (e.g., a team that runs its internal syncs in a local language). When the transcript is not in English:

1. **Clean in the original language first** - apply all the same ASR correction rules (names, terms, speakers) but keep the transcript in its original language.
2. **Then translate to English** - produce a second pass that translates the cleaned transcript to English. Preserve speaker-turn structure exactly. Translate meaning, not word-for-word.
3. **Return only the English version** - the cleaned output file should be in English. This ensures all processed docs are consistently in English for searchability.

Add a note in the blockquote header: `> Cleaned and translated from {language}.`

If the transcript mixes languages (e.g., a local language with English technical terms), keep the technical terms in English as-is during translation.

## What NOT to fix

- Don't rephrase clear sentences
- Don't remove conversational tone (filler words, hesitations, informal language)
- Don't merge speaker turns
- Don't add content that wasn't in the original
- Don't change meaning - when in doubt, leave it as-is
- Don't remove or reorder any part of the transcript
- Don't invent context or fill in gaps - if something is unclear, leave it as-is or mark with `[unclear]`
- Don't drop content. Every speaker turn in the input must appear in the output. Preserving full context is more important than a clean read.

## Output format

Return the cleaned transcript with:

1. YAML frontmatter:

   ```yaml
   ---
   tags: [cleaned-transcript]
   source: {original_filename}
   ---
   ```

2. Blockquote header after frontmatter:

   ```
   > Cleaned version of {original title}. ASR errors corrected using project context. See source file for the raw version.
   ```

3. The cleaned transcript body - same structure as the input, with corrections applied

The output should be the complete cleaned transcript ready to save as a file. Do not include any commentary or explanation outside the transcript itself.
