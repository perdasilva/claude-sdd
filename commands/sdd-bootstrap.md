Help me define the foundation for this project. We will create governing spec documents under a specs/ directory and project-specific workflow commands under .claude/commands/.

Important: Use the AskUserQuestion tool at every step to gather my requirements before writing anything. Do not assume — ask.

## Step 0: Analyze existing project (brownfield detection)

Before asking any questions, check whether this is a greenfield or brownfield project:

1. Look for existing code, configuration, and documentation in the repository (e.g., package.json, Cargo.toml, go.mod, pyproject.toml, Makefile, Dockerfile, README.md, .github/, src/, etc.).
2. If the project already has code or configuration:
   - Analyze the existing files to infer: language/runtime, dependencies, project structure, build system, test framework, linting/formatting tools, CI/CD, and containerization.
   - Summarize what you found and present it to the user via AskUserQuestion, asking them to confirm or correct your analysis before proceeding.
   - Use the confirmed analysis as the starting point for each step below — pre-fill what you can derive from the codebase and only ask about what's missing or ambiguous.
3. If the project is empty (greenfield), proceed directly to Step 1.

## Step 1: Mission (specs/mission.md)

For brownfield projects, propose a mission based on existing README, code structure, and comments. Ask the user to confirm or refine.

For greenfield projects, ask me about:
- What this project does and who it's for
- Key goals (3-5 bullet points)
- What's explicitly NOT in scope (non-goals)
- Core design principles (how we make decisions)
- Development practices (quality gates, workflow conventions)

Write specs/mission.md with sections: Goals, Non-Goals, Design Principles, Development Practices.

## Step 2: Tech Stack (specs/tech-stack.md)

For brownfield projects, present the inferred tech stack from Step 0 and ask the user to confirm, correct, or add missing details.

For greenfield projects, ask me about:
- Language, runtime, and module system
- Core dependencies and their purpose
- Dev dependencies (testing, linting, formatting, building)
- Project structure (directory layout)
- Build and run commands (and whether to use a Makefile, npm scripts, or other task runner)
- Containerization (if applicable)
- CI/CD (if applicable)

Write specs/tech-stack.md with sections for each concern, including a project structure tree and a build commands table.

Remember the project's check command (e.g., `make check`, `npm test`, `cargo test`), format command, and whether Docker is used — these will be needed for generating workflow commands in Step 6.

## Step 3: Roadmap (specs/roadmap.md)

For brownfield projects, propose phases based on what exists vs what's missing or needs improvement. Ask the user to confirm priorities.

For greenfield projects, ask me about:
- How work should be phased (vertical slices vs horizontal layers vs core-then-extend)
- What the first few phases should cover
- How granular each phase should be

Write specs/roadmap.md with numbered phases, each containing 3-6 bullet points describing deliverables.

## Step 4: Commit & PR conventions (specs/conventions.md)

For brownfield projects, check existing git history for commit message patterns and any PR templates (.github/PULL_REQUEST_TEMPLATE.md). Propose conventions based on what's already in use.

For greenfield projects, ask me about:
- Commit message format (e.g., conventional commits, free-form with subject+body, single-line, etc.)
- Whether commits should include a co-author line or other trailer
- PR title format (e.g., match commit subject, include phase/ticket reference, etc.)
- PR description template (e.g., Summary + Test Plan sections, checklist, link to spec, etc.)
- Any other conventions for branch naming, commit scope, or PR labels

Write specs/conventions.md with sections: Commit Messages, Pull Requests, Branch Naming. Include concrete examples for each format so the conventions are unambiguous.

## Step 5: CLAUDE.md

Create a CLAUDE.md at the project root with:
- Project overview (one paragraph)
- Architecture summary (if applicable, from tech stack)
- Key design principles (from mission)
- How to build, test, and run (from tech stack — list the actual commands)
- Phase-based workflow description and slash command reference (from the commands generated in Step 6)
- Conventions summary (from conventions)

## Step 6: Generate project-specific workflow commands

Create four slash commands under .claude/commands/, customized to this project's tech stack and conventions:

### .claude/commands/sdd-plan-next-phase.md

Generate a command that:
- Checks out main and pulls latest
- Finds the next phase in specs/roadmap.md
- Creates a new branch
- Uses AskUserQuestion to ask about the feature spec before writing
- Creates a spec directory (YYYY-MM-DD-phase-N-name/) with plan.md, requirements.md, validation.md
- References specs/mission.md and specs/tech-stack.md for guidance
- Auto-runs a review pass after writing

### .claude/commands/sdd-implement.md

Generate a command that:
- Reads the phase spec (plan.md, requirements.md, validation.md)
- Follows task groups in order
- Verifies validation criteria after implementation
- Runs the project's actual check command (e.g., `make check`, `npm test`, `cargo test` — use whatever was defined in specs/tech-stack.md)
- Uses AskUserQuestion for uncovered decisions

### .claude/commands/sdd-review.md

Generate a command that:
- Reviews all files changed on the branch
- Checks internal consistency and consistency with governing specs
- Checks whether CLAUDE.md or governing specs need updating
- Uses AskUserQuestion for multi-option issues
- Applies straightforward fixes directly

### .claude/commands/sdd-ship.md

Generate a command that:
- Runs the project's actual check command
- Runs the project's build/container verification if applicable (e.g., `make docker-build` — only if Docker was set up in tech stack)
- Verifies validation criteria from the phase spec (if one exists)
- Checks CLAUDE.md freshness
- Runs the project's format command
- Reads specs/conventions.md for commit and PR format
- Uses AskUserQuestion to confirm before committing
- Handles squashing post-review fixes into a single commit
- Creates or updates the PR following the project's conventions

## Step 7: Review

After writing all files, run a review pass: check for internal consistency across all documents and generated commands, gaps, contradictions, and feasibility issues. For brownfield projects, also verify that the specs accurately reflect the existing codebase. Verify the generated commands reference the correct project-specific commands. Use the AskUserQuestion tool for issues with multiple valid options. Apply straightforward fixes directly.
