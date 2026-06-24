# fmartinezvidal-plugins

Claude Code plugin marketplace for productivity and academic workflows.

## Installation

Add the marketplace, then install a plugin:

```bash
/plugin marketplace add facundomartinezvidal/claude-plugins
/plugin install u-flow@fmartinezvidal-plugins
```

## Plugins

### u-flow — academic productivity for university students

Organize course material, research with your cátedra's sources, write reports and presentations against the real consigna, and generate study aids — all in Spanish, all grounded in your own course files.

| | |
|---|---|
| **Skills** | 8 — summarize, study-aids, diagrams, documents, presentations, research, init, organize |
| **Commands** | 8 — `/u-flow:init`, `/u-flow:organize`, `/u-flow:summarize`, `/u-flow:flashcards`, `/u-flow:diagram`, `/u-flow:report`, `/u-flow:presentation`, `/u-flow:research` |
| **Agents** | 3 — `reader` (isolated PDF reading), `reviewer` (verifies output against sources), `planner` (autonomous study plans) |

Core idea: **material-first**. Every skill reads your course files (apuntes, PDFs, consignas) before the web, and everything generated can be verified back against those sources by the reviewer agent.

→ Full documentation: [plugins/u-flow/README.md](plugins/u-flow/README.md)

## Repository Structure

```
claude-plugins/
├── .claude-plugin/
│   └── marketplace.json      ← marketplace manifest
└── plugins/
    └── u-flow/
        ├── .claude-plugin/
        │   └── plugin.json   ← plugin manifest
        ├── skills/           ← 8 skills (SKILL.md each)
        ├── commands/         ← 8 slash commands
        ├── agents/           ← 3 subagents
        └── README.md
```

## Contributing

Contributions and suggestions welcome — open an issue or PR on [GitHub](https://github.com/facundomartinezvidal/claude-plugins).

## License

MIT
