# Obsidian integration

If you maintain an Obsidian vault, you can wire it into this workspace so your agent can read your notes, navigate the graph, and update your daily note without leaving the CLI.

This works with any agent that can run shell commands: it uses the official Obsidian CLI, not a tool-specific integration.

This is optional. The workspace works fine without Obsidian.

## The integration, in one sentence

You point Obsidian at the `build/` folder as a vault, keep Obsidian running with its command line interface enabled, and install the official Obsidian agent skills. The agent uses the CLI to read and write notes; you use Obsidian for visual navigation, graph views, and plugins.

## Setup

1. Install Obsidian (installer 1.12.7 or later): <https://obsidian.md>
2. Open the `build/` directory as an Obsidian vault.
3. Enable the CLI: **Settings → General → Command line interface**, then follow the prompt to register it. See <https://help.obsidian.md/cli>.
4. Verify with `obsidian help` while Obsidian is running.
5. Install the official skills, which teach the agent the CLI, Obsidian Markdown, Bases, and Canvas: <https://github.com/obsidianmd/obsidian-skills>. In Claude Code:

   ```
   /plugin install obsidian --marketplace obsidianmd/obsidian-skills
   ```

   Codex and Cursor installs are covered in that repo's README.

> Use the `obsidian` command that ships with the Obsidian installer. Third-party tools named `obsidian-cli` (on npm or GitHub) are unrelated and use a different command syntax.

## Conventions worth knowing

The official `obsidian-cli` skill carries the full command reference, so this doc doesn't repeat it. A few points matter for this workspace:

- **Vault targeting.** When the agent runs from inside `build/`, the CLI targets this vault automatically. If you have several vaults and the agent runs from elsewhere, pass `vault="<name>"`, which must be the first parameter: `obsidian vault="build" search query="STATUS"`.
- **Exit codes don't signal failure.** The CLI exits 0 even when a command fails, so read its output (`Error: ...`) before trusting a write.
- **Daily notes.** `obsidian daily:read`, `obsidian daily:append content="..."`, and `heading="<section>"` to insert under a specific heading.

## Obsidian Bases view

Obsidian Bases lets you define tabular views over your vault (the official `obsidian-bases` skill covers the syntax). This workspace ships `OBSIDIAN.base` at the workspace root - a starter config that shows "all active project STATUS files" and "recently edited markdown outside `raw/`". Drop it in and tune to taste.

Sample structure:

```yaml
views:
  - type: table
    name: Project status
    filters:
      and:
        - file.basename == "STATUS"
        - file.inFolder("projects")
    order:
      - file.folder
      - file.basename
      - file.mtime
    sort:
      - property: file.mtime
        direction: DESC
```

## Skill patterns (not bundled)

The author has skills that wrap common Obsidian workflows:

- `daily-note` - read/append to the Obsidian daily note.
- `clippings-digest` - summarize unreviewed web clippings into the daily note.

General vault navigation and search is covered by the official skills above, so there's no need for a custom wrapper.

These aren't bundled in this repo because they depend on personal vault structure. The patterns are simple enough to replicate:

- A skill that runs `obsidian daily:read` on invocation and surfaces today's note.
- A skill that logs a timestamped line with `obsidian daily:append`, then checks the output for `Appended to:`.

Use the skill definitions in your own `~/.claude/skills/` folder. See `docs/skills-catalog.md` for a discussion of which patterns to bundle vs. reference.

## Privacy

If your vault contains personal or sensitive material, remember: everything in the vault is readable by the agent when the CLI is available. Treat the vault as you would any folder you've given an agent file access to. For highly sensitive notes, consider a separate vault that's not pointed at this workspace.
