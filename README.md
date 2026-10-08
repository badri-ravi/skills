# Skills

A growing collection of portable agent skills for use across coding harnesses.

## Available skills

| Skill | Purpose | Document |
| --- | --- | --- |
| `easyui` | Discover, integrate, and adapt [EasyUI](https://www.easyui.site/) animated React components | [SKILL.md](skills/easyui/SKILL.md) |

Each skill uses the [Agent Skills format](https://agentskills.io/specification): YAML metadata followed by Markdown instructions. The EasyUI skill contains no harness-specific tool names, executable hooks, or plugin dependencies.

## Repository structure

```text
skills/
├── README.md
├── LICENSE
└── skills/
    └── easyui/
        └── SKILL.md
```

The outer `skills` directory is the repository; the inner `skills` directory contains individual skills. Add future skills as siblings of `easyui`.

## Add another skill

Create `skills/<skill-name>/SKILL.md` with a lowercase, hyphenated folder name matching its YAML `name`. Include a `description` that explains the capability and when to use it. Write the instructions below the YAML frontmatter. Add `references/`, `scripts/`, or `assets/` inside that skill only when they support its workflow, and use relative links.

Add the new skill to the table above. Keep instructions portable; document any harness-specific requirements rather than silently assuming a tool or integration exists.

## Install in a coding harness

See the **[installation guide](INSTALL.md)** for step-by-step instructions, Windows and macOS/Linux commands, project-only installs, updates, and troubleshooting. It covers Codex, Claude Code, GitHub Copilot, Cursor, and Claude web/Desktop.

Copy `skills/easyui` into a supported skill location. Keep the folder named `easyui`. The same skill document works in each location below; future skills follow the same pattern.

| Harness | Project location | Personal location | Official documentation |
| --- | --- | --- | --- |
| Codex | `.agents/skills/easyui/SKILL.md` | `~/.agents/skills/easyui/SKILL.md` | [Codex skills](https://learn.chatgpt.com/docs/build-skills) |
| Claude Code | `.claude/skills/easyui/SKILL.md` | `~/.claude/skills/easyui/SKILL.md` | [Claude Code skills](https://code.claude.com/docs/en/skills) |
| GitHub Copilot | `.github/skills/easyui/SKILL.md` or `.agents/skills/easyui/SKILL.md` | `~/.copilot/skills/easyui/SKILL.md` or `~/.agents/skills/easyui/SKILL.md` | [Copilot agent skills](https://docs.github.com/en/copilot/concepts/agents/about-agent-skills) |
| Cursor | `.cursor/skills/easyui/SKILL.md` or `.agents/skills/easyui/SKILL.md` | `~/.cursor/skills/easyui/SKILL.md` or `~/.agents/skills/easyui/SKILL.md` | [Cursor skills](https://cursor.com/docs/skills) |
| Claude web/Desktop | Upload a ZIP of the individual skill folder | Enabled through your Claude account | [Claude skill uploads](https://support.claude.com/en/articles/12512180-use-skills-in-claude) |

`~` means your home directory. Locations and invocation features can vary by harness version; the linked documentation describes current support. Installing the same skill in multiple locations within one harness may create duplicates. Choose one location for that harness.

To maintain one source, clone this repository and copy or, where supported, symlink its `skills/easyui` folder into the chosen skill location. Pull updates to the clone, then refresh copied installations. Codex, Copilot, and Cursor can share a supported `.agents/skills` installation. Claude Code uses its own documented location.

For a harness without native skill discovery, provide `skills/easyui/SKILL.md` as context and ask it to follow the document. Automatic selection and slash-command support depend on the harness, not this file.

## Use

Example requests:

- "Use the easyui skill to add an animated login form to this React app."
- "Use the easyui skill to integrate a data table with filtering and pagination."
- "Use the easyui skill to add a notification panel that supports keyboard access and reduced motion."

In Codex, you can mention `$easyui`. In Claude Code, you can invoke `/easyui`. Other harnesses may select the skill from its description or offer their own invocation UI.

The agent needs access to the target project and editing tools. Access to the official website or repository enables current documentation and implementation checks. Without network access, the skill directs the agent to use inspected local or user-provided source and disclose that freshness was not verified.

## Sources and scope

Created from the documentation routes and component model described in [EasyUI's llms.txt](https://www.easyui.site/llms.txt), with [the Button page](https://www.easyui.site/components/button) checked as a representative integration page. Some linked guide pages were inaccessible during preparation, so no unverified motion constants, dependency versions, or installation commands are embedded.

The skill fetches relevant documentation when needed rather than freezing the component catalog. It instructs the agent to distinguish UI demos from real application services and to validate accessibility and motion behavior in the host project.

Documentation locations were checked on 2026-10-08. Format validation does not constitute execution testing in every harness or a real React integration test.

## License

The original skill instructions and repository documentation are licensed under [MIT](LICENSE). EasyUI is a separate project; its source and notices remain governed by its [upstream license](https://github.com/Surajmaurya1/easyui/blob/main/LICENSE). This repository does not bundle EasyUI component source and is not an official EasyUI distribution.
