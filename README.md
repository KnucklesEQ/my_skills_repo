# My Skills Repo

A repository for the prompts, rules, skills, and supporting documents I use in my AI-powered projects.

I use [OpenCode](https://github.com/anomalyco/opencode) as the main tool for working with these assets.

## OpenCode File Locations

### Project-level files

- `AGENTS.md`: project-specific rules and instructions.
- `opencode.json`: project config for models, permissions, instructions, commands, agents, plugins, MCP servers, and more.
- `tui.json`: optional TUI-only settings such as theme and keybindings.

### `.opencode/` directory

- `.opencode/skills/<name>/SKILL.md`: reusable skills loaded on demand.
- `.opencode/commands/<name>.md`: custom slash commands.
- `.opencode/agents/<name>.md`: custom agents.
- `.opencode/tools/<name>.ts`: custom tools callable by the agent.
- `.opencode/plugins/<name>.ts`: plugins and hooks that extend OpenCode.
- `.opencode/package.json`: optional dependencies for local tools and plugins.

## Supporting Project Documentation

These are not special OpenCode files, but they are very useful in OpenCode-driven workflows.

- `docs/`: stable project documentation such as architecture notes, conventions, ADRs, onboarding, and user-facing docs.
- `specs/`: change-focused specifications for spec-driven development.

Recommended convention for specs:

- `specs/<nnn-slug>/spec.md`: required spec file.
- `specs/<nnn-slug>/plan.md`: optional implementation plan.
- `specs/<nnn-slug>/tasks.md`: optional task breakdown or checklist.

Example:

```text
specs/
  001-readme-structure/
    spec.md
  002-new-skill-template/
    spec.md
    plan.md
    tasks.md
```

## Recommended Repository Layout

```text
.
├── AGENTS.md
├── README.md
├── opencode.json
├── tui.json
├── docs/
│   ├── architecture.md
│   ├── conventions.md
│   └── user-guide.md
├── specs/
│   └── 001-readme-structure/
│       ├── spec.md
│       ├── plan.md
│       └── tasks.md
└── .opencode/
    ├── package.json
    ├── skills/
    │   └── behavior-driven-java-tests/
    │       └── SKILL.md
    ├── commands/
    ├── agents/
    ├── tools/
    └── plugins/
```

## Notes

- Use `docs/` for stable, general project documentation.
- Use `specs/` for feature or change-specific work.
- Prefer one folder per skill, and keep the folder name aligned with the `name` field inside `SKILL.md`.
- Prefer one folder per spec using `specs/<nnn-slug>/`.
- Keep `spec.md` mandatory; add `plan.md` and `tasks.md` only when they add value.
- OpenCode also supports compatibility paths such as `.claude/` and `.agents/`, but this repository uses the native `.opencode/` layout.

## Global OpenCode Files

These live outside the repository and are useful for personal defaults:

- `~/.config/opencode/AGENTS.md`: global user rules.
- `~/.config/opencode/opencode.json`: global config.
- `~/.config/opencode/tui.json`: global TUI (Terminal User Interface) settings.
- `~/.config/opencode/skills/`: global reusable skills.
- `~/.config/opencode/commands/`: global commands.
- `~/.config/opencode/agents/`: global agents.
- `~/.config/opencode/tools/`: global tools.
- `~/.config/opencode/plugins/`: global plugins.
