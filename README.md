# claude-sdd

A Claude Code plugin that bootstraps spec-driven development for any project. It creates governing spec documents and generates project-specific workflow commands customized to your tech stack.

## Installation

```bash
claude plugin marketplace add perdasilva/claude-sdd
claude plugin install sdd@claude-sdd
```

## How it works

Run `/sdd:bootstrap` in any project. It will:

1. **Detect brownfield vs greenfield** — analyzes existing code, configs, and docs to pre-fill what it can
2. **Create governing specs** — walks you through defining mission, tech stack, roadmap, and conventions
3. **Generate project-specific workflow commands** — four slash commands customized to your tech stack
4. **Create CLAUDE.md** — project context for Claude Code, referencing the generated commands

Re-running `/sdd:bootstrap` on an already-bootstrapped project is safe — it will ask whether to update, overwrite, or abort.

The generated workflow commands:

| Generated command | What it does |
|-------------------|-------------|
| `/sdd-plan-next-phase` | Find next roadmap phase, create branch + spec, auto-review |
| `/sdd-implement` | Build from phase spec, verify validation criteria |
| `/sdd-review` | Review branch changes for consistency and correctness |
| `/sdd-ship` | Validate, commit, push, create PR (asks before committing) |

The generated commands use your project's actual build/test/format commands (e.g., `make check` vs `cargo test`) and follow your commit/PR conventions. They live in your project and can evolve with it.

## Project structure after bootstrapping

```
your-project/
├── specs/
│   ├── mission.md                          # Goals, principles, practices
│   ├── tech-stack.md                       # Dependencies, structure, commands
│   ├── roadmap.md                          # Phased implementation plan
│   ├── conventions.md                      # Commit, PR, and branch conventions
│   └── YYYY-MM-DD-phase-N-name/           # Per-phase specs (created by /sdd-plan-next-phase)
│       ├── plan.md                         # Numbered task groups
│       ├── requirements.md                 # Scope, decisions, context
│       └── validation.md                   # Acceptance criteria
├── .claude/
│   └── commands/                           # Generated workflow commands
│       ├── sdd-plan-next-phase.md
│       ├── sdd-implement.md
│       ├── sdd-review.md
│       └── sdd-ship.md
└── CLAUDE.md                              # Project context for Claude Code
```

## Plugin structure

```
claude-sdd/
├── .claude-plugin/
│   └── marketplace.json                   # Marketplace manifest
├── plugins/
│   └── sdd/
│       ├── .claude-plugin/
│       │   └── plugin.json                # Plugin metadata
│       ├── skills/
│       │   └── bootstrap/
│       │       └── SKILL.md               # /sdd:bootstrap command
│       └── README.md
└── README.md
```

## Updating

```bash
claude plugin update sdd@claude-sdd
```
