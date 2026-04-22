# claude-sdd

A Claude Code plugin that bootstraps spec-driven development for any project. It creates governing spec documents and generates project-specific workflow commands customized to your tech stack.

## What is Spec-Driven Development?

Spec-Driven Development (SDD) is a workflow where you define **what** you're building before you write code, then use those specs to drive every subsequent step — planning, implementation, review, and shipping.

The core idea: a set of governing specs (mission, tech stack, roadmap, conventions) acts as a single source of truth for your project. Claude Code reads these specs at every stage to make decisions that stay consistent with your goals, stack, and conventions — without you having to repeat yourself.

The workflow follows a repeating cycle:

```
   ┌─────────────────────────────────────────────┐
   │              /sdd:bootstrap                  │
   │  Define mission, tech stack, roadmap,        │
   │  conventions → generate workflow commands    │
   └──────────────────┬──────────────────────────┘
                      │
                      ▼
   ┌──────────────────────────────────────────────┐
   │           /sdd-plan-next-phase               │
   │  Pick next roadmap phase, create branch,     │◄───┐
   │  write phase spec (plan + requirements +     │    │
   │  validation criteria)                        │    │
   └──────────────────┬──────────────────────────┘    │
                      │                                │
                      ▼                                │
   ┌──────────────────────────────────────────────┐    │
   │             /sdd-implement                   │    │
   │  Build from the phase spec, follow task      │    │
   │  groups in order, run checks, verify         │    │
   │  validation criteria                         │    │
   └──────────────────┬──────────────────────────┘    │
                      │                                │
                      ▼                                │
   ┌──────────────────────────────────────────────┐    │
   │              /sdd-review                     │    │
   │  Review changes against specs, check         │    │
   │  consistency, fix issues                     │    │
   └──────────────────┬──────────────────────────┘    │
                      │                                │
                      ▼                                │
   ┌──────────────────────────────────────────────┐    │
   │               /sdd-ship                      │    │
   │  Run checks, verify validation, commit,      │────┘
   │  push, create PR — then back to next phase   │
   └──────────────────────────────────────────────┘
```

Each command reads your governing specs so it uses the right build commands, follows your commit conventions, and stays aligned with your project goals.

## Installation

```bash
claude plugin marketplace add perdasilva/claude-sdd
claude plugin install sdd@claude-sdd
```

## Usage example

### 1. Bootstrap your project

Open Claude Code in your project directory and run:

```
/sdd:bootstrap
```

Claude will walk you through an interactive session:

- **Brownfield detection** — if your project already has code, it analyzes your repo to infer the tech stack, then asks you to confirm or correct
- **Mission** — define what the project does, its goals, non-goals, and design principles
- **Tech stack** — language, dependencies, build/test/format commands, project structure
- **Roadmap** — break the work into numbered phases with deliverables
- **Conventions** — commit message format, PR templates, branch naming
- **Command generation** — creates four workflow commands customized to your stack
- **CLAUDE.md** — generates project context so Claude Code understands your project

After bootstrapping, you'll have a `specs/` directory with your governing specs and four slash commands under `.claude/commands/`. Re-running `/sdd:bootstrap` on an already-bootstrapped project is safe — it will ask whether to update, overwrite, or abort.

> **Note:** After bootstrapping, restart Claude Code so it picks up the newly generated slash commands. The `/sdd-plan-next-phase`, `/sdd-implement`, `/sdd-review`, and `/sdd-ship` commands won't be available until the session is restarted.

### 2. Plan a phase

```
/sdd-plan-next-phase
```

This finds the next unstarted phase in your roadmap, creates a feature branch, and writes a phase spec:
- `plan.md` — numbered task groups to implement
- `requirements.md` — scope, decisions, and context
- `validation.md` — acceptance criteria for the phase

It auto-reviews the spec for consistency before finishing.

### 3. Implement

```
/sdd-implement
```

Claude reads the phase spec and builds the feature, following the task groups in order. It runs your project's check command after implementation and verifies the validation criteria from the spec. If it hits an uncovered decision, it asks you.

### 4. Review

```
/sdd-review
```

Reviews all changes on the branch against the governing specs and the phase spec. Checks for internal consistency, spec compliance, and whether `CLAUDE.md` needs updating. Applies straightforward fixes directly and asks about anything ambiguous.

### 5. Ship

```
/sdd-ship
```

Three phases:
1. **Verify** — runs checks, validates acceptance criteria, verifies formatting
2. **Commit** — reads your conventions, drafts the commit, and asks you to confirm before committing
3. **Publish** — asks before pushing, then creates (or updates) a PR following your conventions

Then go back to step 2 for the next phase.

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
