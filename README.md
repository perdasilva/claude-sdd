# claude-spec-driven

A set of Claude Code slash commands for spec-driven development. Define your project's mission, tech stack, and roadmap as governing specs, then use phase-based commands to plan, implement, review, and ship work incrementally.

## How it works

1. **Bootstrap** — `/bootstrap-sdd` walks you through creating four governing spec documents:
   - `specs/mission.md` — goals, non-goals, design principles, development practices
   - `specs/tech-stack.md` — language, dependencies, project structure, build commands
   - `specs/roadmap.md` — phased implementation plan with deliverables
   - `specs/conventions.md` — commit message format, PR templates, branch naming

2. **Phase workflow** — work proceeds in small phases, each with its own spec:
   - `/sdd-plan-next-phase` — finds the next roadmap phase, creates a branch, asks questions, writes a spec directory (`plan.md`, `requirements.md`, `validation.md`), then auto-reviews
   - `/sdd-implement` — builds from the phase spec, follows task groups, verifies validation criteria
   - `/sdd-review` — checks all branch changes for consistency with governing specs
   - `/sdd-ship` — validates everything, asks before committing, creates a PR

## Installation

Copy the `commands/` directory into your project's `.claude/` folder:

```bash
cp -r /path/to/claude-spec-driven/commands/ your-project/.claude/commands/
```

The commands will be available as slash commands in Claude Code: `/sdd-bootstrap`, `/sdd-plan-next-phase`, `/sdd-implement`, `/sdd-review`, `/sdd-ship`.

## Spec structure

After bootstrapping, your project will have:

```
your-project/
├── specs/
│   ├── mission.md                          # Goals, principles, practices
│   ├── tech-stack.md                       # Dependencies, structure, commands
│   ├── roadmap.md                          # Phased implementation plan
│   ├── conventions.md                      # Commit, PR, and branch conventions
│   └── YYYY-MM-DD-phase-N-name/           # Per-phase specs (created by /next-phase)
│       ├── plan.md                         # Numbered task groups
│       ├── requirements.md                 # Scope, decisions, context
│       └── validation.md                   # Acceptance criteria
├── .claude/
│   └── commands/                           # Slash commands (from this repo)
└── CLAUDE.md                              # Project context for Claude Code
```

## Commands reference

| Command | What it does |
|---------|-------------|
| `/sdd-bootstrap` | Create mission.md, tech-stack.md, roadmap.md, and conventions.md interactively |
| `/sdd-plan-next-phase` | Find next roadmap phase, create branch + spec, auto-review |
| `/sdd-implement` | Build from phase spec, verify validation criteria |
| `/sdd-review` | Review branch changes for consistency and correctness |
| `/sdd-ship` | Validate, commit, push, create PR (asks before committing) |
