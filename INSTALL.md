# Install and use the skills

The repository is a collection. Install an individual folder such as `skills/easyui`, not the repository root. Keep the folder name aligned with the `name` in its `SKILL.md`. No GitHub sign-in is needed to download this public repository.

## Choose your harness

| Harness | Personal installation: all local projects | Project installation: one project | Use after installing |
| --- | --- | --- | --- |
| Codex | `~/.agents/skills/easyui/` | `<project>/.agents/skills/easyui/` | Mention `$easyui` |
| Claude Code | `~/.claude/skills/easyui/` | `<project>/.claude/skills/easyui/` | Invoke `/easyui` |
| GitHub Copilot | `~/.copilot/skills/easyui/` | `<project>/.github/skills/easyui/` | Ask Copilot to use the EasyUI skill |
| Cursor | `~/.cursor/skills/easyui/` | `<project>/.cursor/skills/easyui/` | Ask Agent to use the EasyUI skill |
| Claude web/Desktop | Upload the individual skill ZIP to your account | Use the account's enabled skill | Ask Claude to use the EasyUI skill |

`~` means your home directory. On Windows this is usually `C:\Users\your-name`. `<project>` means the target application's root, not this skills repository. Each local installation must end with `easyui/SKILL.md`.

Choose one location per harness to avoid duplicate discovery. Codex, Copilot, and Cursor also support `.agents/skills` locations, so they can share a suitable installation. Claude Code uses its documented `.claude/skills` location. Check the official documentation below for version-specific support.

## Codex: install through chat

In the target Codex instance, send:

```text
Use $skill-installer to install the EasyUI skill from
https://github.com/badri-ravi/skills/tree/main/skills/easyui
for my personal use.
```

Use this method when that instance offers the bundled skill installer. The installer chooses a location supported by its environment; the manual method below uses the currently documented `.agents/skills` location.

Then try:

```text
Use $easyui to add an animated login form to this React project.
```

If the installed skill is not visible, start a new session or restart the instance. See [official Codex skill guidance](https://learn.chatgpt.com/docs/build-skills).

## Manual installation: local coding harnesses

These commands work for Codex, Claude Code, GitHub Copilot, and Cursor. They copy the complete skill folder, including any future supporting files.

First clone the collection into a directory of your choice and enter it:

```text
git clone https://github.com/badri-ravi/skills.git
cd skills
```

If you already have a clone, enter it and run `git pull --ff-only` instead. Keep this clone outside the target application's skill directory.

### Windows PowerShell

Choose exactly one destination. This example installs personally for Codex:

```powershell
$easyuiDestination = Join-Path $env:USERPROFILE '.agents/skills/easyui'
```

For another harness, replace the path with `.claude/skills/easyui`, `.copilot/skills/easyui`, or `.cursor/skills/easyui` from the table. For a project-only install, use an absolute project path, for example:

```powershell
$easyuiDestination = 'C:\Projects\my-app\.claude\skills\easyui'
```

From the collection's root, run:

```powershell
if (Test-Path -LiteralPath $easyuiDestination) {
    throw 'The skill already exists. Use the update instructions below.'
}
New-Item -ItemType Directory -Path (Split-Path -Parent $easyuiDestination) -Force | Out-Null
Copy-Item -LiteralPath './skills/easyui' -Destination $easyuiDestination -Recurse
Get-Item -LiteralPath (Join-Path $easyuiDestination 'SKILL.md')
```

### macOS/Linux: Bash or Zsh

Choose exactly one destination. This example installs personally for Claude Code:

```sh
easyui_destination="$HOME/.claude/skills/easyui"
```

For another harness, replace `.claude` with `.agents` for Codex, `.copilot` for Copilot, or `.cursor` for Cursor. For a project-only install, set an absolute path such as `/path/to/my-app/.agents/skills/easyui`.

From the collection's root, run:

```sh
if [ -e "$easyui_destination" ]; then
  printf '%s\n' 'The skill already exists. Use the update instructions below.'
else
  mkdir -p "$(dirname "$easyui_destination")" &&
    cp -R skills/easyui "$easyui_destination" &&
    ls "$easyui_destination/SKILL.md"
fi
```

Open the target application and a new session if it has not picked up the skill. In Claude Code, try `/easyui`; in Codex, try `$easyui`. In Copilot or Cursor Agent, ask it to use the EasyUI skill for a React task.

## Claude web/Desktop: upload a skill

Local filesystem installation is for Claude Code. Claude web/Desktop and Cowork use skills enabled for your Claude account; copying a folder on your computer alone does not install an account skill.

From the collection's root, create a ZIP containing only the `easyui` folder.

Windows PowerShell:

```powershell
Compress-Archive -LiteralPath './skills/easyui' -DestinationPath './easyui.zip'
```

macOS/Linux, if the `zip` utility is installed:

```sh
(cd skills && zip -r ../easyui.zip easyui)
```

The ZIP should contain `easyui/SKILL.md` at that path, not `skills/easyui/SKILL.md`. Do not upload GitHub's ZIP of the entire collection as a single skill.

In Claude, open **Customize > Skills**, choose **+ Create skill**, select **Upload a skill**, and upload `easyui.zip`. Enable the uploaded skill. Availability depends on your plan and organization settings. See [Claude's upload instructions](https://support.claude.com/en/articles/12512180-use-skills-in-claude).

The instruction format is portable; the available browsing, editing, and execution tools still determine what each environment can implement.

## Update an installed skill

GitHub updates do not automatically change copied installations. In your collection clone, run:

```text
git pull --ff-only
```

Review the changed skill before replacing the installed copy. Reuse the destination you chose above. These commands update files inside that existing skill only.

Windows PowerShell:

```powershell
if (-not (Test-Path -LiteralPath (Join-Path $easyuiDestination 'SKILL.md'))) {
    throw 'Set easyuiDestination to the existing installed skill directory.'
}
Copy-Item -Path './skills/easyui/*' -Destination $easyuiDestination -Recurse -Force
```

macOS/Linux:

```sh
if [ -f "$easyui_destination/SKILL.md" ]; then
  cp -R skills/easyui/. "$easyui_destination/"
else
  printf '%s\n' 'Set easyui_destination to the existing installed skill directory.'
fi
```

These copy commands do not remove obsolete supporting files. If a future update removes such files, review and remove those specific obsolete files from the installed skill as needed. For Claude web/Desktop, make a fresh individual-skill ZIP and replace the account's uploaded skill through its management UI.

Install separately on another computer or remote environment. Local personal skills are not automatically present in cloud workers; use a supported account-sync feature or commit a project installation when the harness requires it.

## Troubleshooting

- **Skill is missing:** verify the destination ends in `easyui/SKILL.md`, check your harness's supported directories, and start a new session.
- **Duplicate skills appear:** keep one installation per harness; compatible applications may discover each other's locations.
- **Cloud instance cannot find it:** local personal folders belong to that computer. Check the harness's cloud or account-sync instructions, or install in the project available to that instance.
- **The agent cannot install an EasyUI component:** the skill is guidance, not bundled component code. It still needs the target React project and suitable editing, documentation, and execution access.
- **Another skill:** replace `easyui` in the source and destination with the desired folder name. Recheck any environment requirements in that skill's document.

## Official references

Installation locations and UI instructions checked on 2026-10-08:

- [Agent Skills format](https://agentskills.io/specification)
- [Codex skills](https://learn.chatgpt.com/docs/build-skills)
- [Claude Code skills](https://code.claude.com/docs/en/skills)
- [GitHub Copilot agent skills](https://docs.github.com/en/copilot/concepts/agents/about-agent-skills)
- [Cursor skills](https://cursor.com/docs/skills)
- [Claude web/Desktop skill uploads](https://support.claude.com/en/articles/12512180-use-skills-in-claude)

Directory support and menus can change by version. The command examples describe installation; they are not a claim that the skill has been executed in every harness.
