# claude-spec-driven

A single Claude Code slash command that bootstraps spec-driven development for any project. It creates governing spec documents and generates project-specific workflow commands customized to your tech stack.

## How it works

Run `/sdd-bootstrap` in any project. It will:

1. **Detect brownfield vs greenfield** — analyzes existing code, configs, and docs to pre-fill what it can
2. **Create governing specs** — walks you through defining:
   - `specs/mission.md` — goals, non-goals, design principles, development practices
   - `specs/tech-stack.md` — language, dependencies, project structure, build commands
   - `specs/roadmap.md` — phased implementation plan with deliverables
   - `specs/conventions.md` — commit message format, PR templates, branch naming
3. **Create CLAUDE.md** — project context for Claude Code
4. **Generate project-specific workflow commands** — four slash commands customized to your tech stack:

| Generated command | What it does |
|-------------------|-------------|
| `/sdd-plan-next-phase` | Find next roadmap phase, create branch + spec, auto-review |
| `/sdd-implement` | Build from phase spec, verify validation criteria |
| `/sdd-review` | Review branch changes for consistency and correctness |
| `/sdd-ship` | Validate, commit, push, create PR (asks before committing) |

The generated commands use your project's actual build/test/format commands (e.g., `make check` vs `cargo test`) and follow your commit/PR conventions. They live in your project and can evolve with it.

## Installation

Install `sdd-bootstrap` as a user-level command so it's available in any project:

```bash
# Clone the repo
git clone https://github.com/perdasilva/claude-spec-driven.git ~/.claude-spec-driven

# Symlink the bootstrap command
mkdir -p ~/.claude/commands
ln -s ~/.claude-spec-driven/commands/sdd-bootstrap.md ~/.claude/commands/sdd-bootstrap.md
```

Then in any project, run `/sdd-bootstrap` to set everything up.

To update:

```bash
cd ~/.claude-spec-driven && git pull
```

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
