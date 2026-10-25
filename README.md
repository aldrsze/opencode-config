# OpenCode Config

My personal OpenCode setup: global rules, CLI config, and skills.

| Path | What it is |
| --- | --- |
| `AGENTS.md` | Global rules and style |
| `opencode.jsonc` | Main config (agent, skills path) |
| `cli.json` | TUI config (theme, tabs, permissions) |
| `skills/` | Each folder is one skill with a `SKILL.md` |

## Install

Linux/macOS:

```sh
git clone https://github.com/aldrsze/opencode-config ~/Projects/opencode-config
```

Windows:

```sh
git clone https://github.com/aldrsze/opencode-config %USERPROFILE%\Projects\opencode-config
```

Point `skills` at the clone in `~/.config/opencode/opencode.jsonc`:

```jsonc
{
  "skills": ["~/Projects/opencode-config/skills"]
}
```

Optional: symlink the files instead of copying, so edits stay in git.

```sh
cd ~/.config/opencode
ln -sf ~/Projects/opencode-config/AGENTS.md AGENTS.md
ln -sf ~/Projects/opencode-config/opencode.jsonc opencode.jsonc
ln -sf ~/Projects/opencode-config/cli.json cli.json
```

`service.json` is machine-local and secret. Never commit it.

## Skills

| Skill | What it does |
| --- | --- |
| `grill-idea` | Brainstorm a project idea, one question at a time |
| `grill-prd` | Grill a PRD for gaps before coding |
| `potato` | Create/update projects in `potato-projects` |
| `guru` | Read-only codebase tutor: explains code, cites `file:line` |
| `simplify` | Rewrite a file to beginner-level code per `AGENTS.md` |
| `starter` | Step-by-step guided project building |
