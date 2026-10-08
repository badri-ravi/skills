# Install, update, and remove skills

Use the [Skills CLI](https://github.com/vercel-labs/skills) with Node.js/npm installed. These examples install `easyui` for all your projects.

## Install

Run the command for your tool:

| Tool | Command |
| --- | --- |
| Claude Code | `npx skills add badri-ravi/skills --skill easyui -a claude-code -g` |
| Codex | `npx skills add badri-ravi/skills --skill easyui -a codex -g` |
| Cursor | `npx skills add badri-ravi/skills --skill easyui -a cursor -g` |
| OpenCode | `npx skills add badri-ravi/skills --skill easyui -a opencode -g` |

For a project-only installation, run the command from your project's root and omit `-g`. Replace `easyui` with another skill name as the collection grows. Start a new agent session after installation.

## Update

Update the installed skill across tools:

```sh
npx skills update easyui -g
```

For project-installed skills, run from the project root and replace `-g` with `-p`.

## Remove

Remove the skill from one tool, using Claude Code as an example:

```sh
npx skills remove easyui -a claude-code -g
```

Replace `claude-code` with `codex`, `cursor`, or `opencode`. For a project-only installation, run from the project root and omit `-g`.

For Claude web/Desktop, upload a ZIP of the individual skill folder through [Customize > Skills](https://support.claude.com/en/articles/12512180-use-skills-in-claude). To update, replace the uploaded skill with a fresh ZIP; to remove, delete it there.
