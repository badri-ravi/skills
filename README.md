# EasyUI skill

A portable agent skill for discovering, integrating, and adapting [EasyUI](https://www.easyui.site/) animated React components.

The reusable artifact is [`easyui/SKILL.md`](easyui/SKILL.md). It uses the [Agent Skills format](https://agentskills.io/specification): YAML metadata followed by Markdown instructions. It contains no harness-specific tool names, executable hooks, or plugin dependencies.

## Install in a coding harness

Copy the `easyui` directory into a skill location supported by your harness. Keep the folder named `easyui` so it matches the skill's metadata. The exact same `SKILL.md` works in each location below.

| Harness | Project location | Personal location | Official documentation |
| --- | --- | --- | --- |
| Codex | `.agents/skills/easyui/SKILL.md` | `~/.agents/skills/easyui/SKILL.md` | [Codex skills](https://learn.chatgpt.com/docs/build-skills) |
| Claude Code | `.claude/skills/easyui/SKILL.md` | `~/.claude/skills/easyui/SKILL.md` | [Claude Code skills](https://code.claude.com/docs/en/skills) |
| GitHub Copilot | `.github/skills/easyui/SKILL.md` or `.agents/skills/easyui/SKILL.md` | `~/.copilot/skills/easyui/SKILL.md` or `~/.agents/skills/easyui/SKILL.md` | [Copilot agent skills](https://docs.github.com/en/copilot/concepts/agents/about-agent-skills) |

`~` means your home directory. Locations and invocation features can vary by harness version; the linked documentation describes current support. Installing the same skill in multiple locations within one harness may create duplicates. Choose one location for that harness.

To maintain one source, clone this repository and copy or, where supported, symlink its `easyui` folder into the chosen skill location. Pull updates to the clone, then refresh copied installations. Codex and Copilot can share a supported `.agents/skills` installation. Claude Code uses its own documented location.

For a harness without native skill discovery, provide `easyui/SKILL.md` as context and ask it to follow the document. Automatic selection and slash-command support depend on the harness, not this file.

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
