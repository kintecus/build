# Obsidian integration

If you maintain an Obsidian vault, you can wire it into this workspace so your agent can read your notes, navigate the graph, and update your daily note without leaving the CLI.

This works with any agent that can run shell commands - it's a CLI wrapper, not a tool integration.

This is optional. The workspace works fine without Obsidian.

## The integration, in one sentence

You point Obsidian at the `build/` folder as a vault, keep Obsidian running, and install the `obsidian-cli` wrapper. The agent uses the CLI to read/write notes directly; you use Obsidian for visual navigation, graph views, and plugins.

## Setup

1. Install Obsidian: <https://obsidian.md>
2. Open the `build/` directory as an Obsidian vault.
3. Install `obsidian-cli`: <https://github.com/Yakitrak/obsidian-cli> (v1.12+).
4. Verify with `obsidian-cli recents` while Obsidian is running.

That's it. The agent can now use the CLI.

## obsidian-cli conventions

- `file=` resolves by note name (like wikilinks); `path=` is exact (folder/note.md).
- Most commands default to the active file when file/path is omitted.
- Quote values with spaces: `name="My Note"`.
- Use `\n` for newline, `\t` for tab in content values.
- To target a different vault: `obsidian-cli <cmd> vault="Other Vault"`.

### Read and browse

```bash
obsidian-cli read path="<vault-relative-path>"
obsidian-cli read file="<note name>"
obsidian-cli files folder="<path>" ext=md
obsidian-cli folders folder="<path>"
obsidian-cli recents
obsidian-cli random:read folder="<path>"
```

### Search

```bash
obsidian-cli search query="<term>" path="<folder>" format=json limit=<n>
obsidian-cli search:context query="<term>" path="<folder>"
```

### Links and graph

```bash
obsidian-cli links path="<note>"             # outgoing
obsidian-cli backlinks path="<note>"         # incoming
obsidian-cli orphans                         # no incoming links
obsidian-cli deadends                        # no outgoing links
obsidian-cli unresolved                      # broken wikilinks
```

### Daily note

```bash
obsidian-cli daily:read
obsidian-cli daily:append content="<text>"
obsidian-cli daily:prepend content="<text>"
obsidian-cli tasks daily todo
```

### Write operations

```bash
obsidian-cli create name="<name>" content="<text>"
obsidian-cli append path="<note>" content="<text>"
obsidian-cli property:set name="<key>" value="<val>" path="<note>"
```

For the full command surface, see `obsidian-cli --help` or the project README.

## Obsidian Bases view

Obsidian's Bases plugin lets you define tabular views over your vault. This workspace ships `OBSIDIAN.base` at the workspace root - a starter config that shows "all active project STATUS files" and "recently edited markdown outside `raw/`". Drop it in and tune to taste.

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
- `obsidian-vault` - navigate and search the vault, read notes, follow links.
- `clippings-digest` - summarize unreviewed web clippings into the daily note.

These aren't bundled in this repo because they depend on personal vault structure. The patterns are simple enough to replicate:

- A skill that runs `obsidian-cli daily:read` on invocation and surfaces today's note.
- A skill that takes a search term, runs `obsidian-cli search`, and formats the results.

Use the skill definitions in your own `~/.claude/skills/` folder. See `docs/skills-catalog.md` for a discussion of which patterns to bundle vs. reference.

## Privacy

If your vault contains personal or sensitive material, remember: everything in the vault is readable by the agent when the CLI is available. Treat the vault as you would any folder you've given an agent file access to. For highly sensitive notes, consider a separate vault that's not pointed at this workspace.
