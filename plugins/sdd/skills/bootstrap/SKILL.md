---
name: bootstrap
description: Bootstrap spec-driven development — create governing specs and generate project-specific workflow commands
allowed-tools: [Read, Glob, Grep, Bash, Write, Edit, AskUserQuestion]
---

Help me define the foundation for this project. We will create governing spec documents under a specs/ directory and project-specific workflow commands under .claude/commands/.

Important: Use the AskUserQuestion tool at every step to gather my requirements before writing anything. Do not assume — ask.

## Step 0: Process initial context

The user may have provided an initial description of their project as input to this command: $ARGUMENTS

If input was provided, use it as context throughout the remaining steps — pre-fill answers where the input is clear enough, and focus your AskUserQuestion prompts on gaps, ambiguities, or details the input didn't cover. Always confirm your interpretation before writing specs.

If no input was provided, proceed fully interactively as described below.

## Step 1: Analyze existing project (brownfield detection)

Before asking any questions, check whether this project has already been bootstrapped and whether it is greenfield or brownfield:

1. Check for existing specs/ directory or .claude/commands/sdd-*.md files. If found, this project was previously bootstrapped — use AskUserQuestion to ask the user whether to: (a) update existing specs and commands in place, filling in gaps but preserving customizations, (b) overwrite everything and start fresh, or (c) abort. Proceed according to their choice.
2. Look for existing code, configuration, and documentation in the repository (e.g., package.json, Cargo.toml, go.mod, pyproject.toml, Makefile, Dockerfile, README.md, .github/, src/, etc.).
3. If the project already has code or configuration:
   - Analyze the existing files to infer: language/runtime, dependencies, project structure, build system, test framework, linting/formatting tools, CI/CD, and containerization.
   - Summarize what you found and present it to the user via AskUserQuestion, asking them to confirm or correct your analysis before proceeding.
   - Use the confirmed analysis as the starting point for each step below — pre-fill what you can derive from the codebase and only ask about what's missing or ambiguous.
4. If the project is empty (greenfield), proceed directly to Step 2.

## Step 2: Mission (specs/mission.md)

For brownfield projects, propose a mission based on existing README, code structure, and comments. Ask the user to confirm or refine.

For greenfield projects, ask me about:
- What this project does and who it's for
- Key goals (3-5 bullet points)
- What's explicitly NOT in scope (non-goals)
- Core design principles (how we make decisions)
- Development practices (quality gates, workflow conventions)

Write specs/mission.md with sections: Goals, Non-Goals, Design Principles, Development Practices.

## Step 3: Tech Stack (specs/tech-stack.md)

For brownfield projects, present the inferred tech stack from Step 1 and ask the user to confirm, correct, or add missing details.

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

## Step 4: Roadmap

Use AskUserQuestion to ask the user which roadmap mode to use:

**(a) File-based (specs/roadmap.md)** — phases are defined in a local markdown file. Good for projects that want everything version-controlled alongside the code.

**(b) GitHub-issue-driven** — phases are tracked as GitHub issues with a configurable label (default: `sdd-phase`). Good for brownfield projects or teams that already use GitHub for planning, since it gives you assignment, status tracking, and PR linking for free.

### If file-based:

For brownfield projects, propose phases based on what exists vs what's missing or needs improvement. Ask the user to confirm priorities.

For greenfield projects, ask me about:
- How work should be phased (vertical slices vs horizontal layers vs core-then-extend)
- What the first few phases should cover
- How granular each phase should be

Write specs/roadmap.md with phases using this format — the generated commands depend on this structure:

```markdown
## Phase 1: <short-name>

- Deliverable one
- Deliverable two
- ...
```

Each phase gets an `## Phase N: <name>` heading followed by 3-6 bullet points describing deliverables. Phase numbers must be sequential.

### If GitHub-issue-driven:

For brownfield projects, check existing issues, labels, templates, and milestones to infer the project's current workflow. Present your findings to the user via AskUserQuestion and ask them to confirm or correct.

For greenfield projects or projects without a defined issue workflow, suggest sensible defaults (noted below) and let the user accept or customize.

Use AskUserQuestion to gather the following configuration:

- **Phase label** — which label identifies phase issues (default: `sdd-phase`). Whether to create it now via `gh label create`
- **Additional required labels** — any other labels that should be applied to phase issues (e.g., `epic`, `feature`, a team label). May be none
- **Issue template** — whether to use an existing issue template (check `.github/ISSUE_TEMPLATE/` for available templates). If none exist, whether to create one. May be skipped
- **Issue title format** — how phase issue titles should be formatted (default: `Phase N: <name>`). Some projects may prefer a different pattern (e.g., `[SDD] Phase N: <name>`, or just a free-form title with the phase label doing the identification)
- **How to find the next phase** — how `/sdd-plan-next-phase` should pick the next issue to work on. Options include:
  - By lowest issue number with the phase label that is open and unassigned (default)
  - By milestone
  - By project board column
  - By a priority label (e.g., `priority/high`, `priority/medium`)
  - Some other project-specific mechanism
- **How to assign** — whether to assign the issue to the current user when starting work (default: yes, via `gh issue edit`)
- **How to close** — which keyword to use in PR descriptions to auto-close the issue on merge (default: `Closes #N`). Some projects use `Fixes #N`, `Resolves #N`, or don't auto-close at all
- **Whether to create initial phase issues now** — or let the user create them later

Do NOT create specs/roadmap.md — the configuration will be baked directly into the generated commands in Step 6. Remember all of the user's choices for use when generating commands.

If the user chose to create initial phase issues now, gather the phases (same questions as the file-based flow), then create them via `gh issue create` using the user's configured labels, template, and title format.

## Step 5: Commit & PR conventions (specs/conventions.md)

For brownfield projects, check existing git history for commit message patterns and any PR templates (.github/PULL_REQUEST_TEMPLATE.md). Propose conventions based on what's already in use.

For greenfield projects, ask me about:
- Commit message format (e.g., conventional commits, free-form with subject+body, single-line, etc.)
- Whether commits should include a co-author line or other trailer
- PR title format (e.g., match commit subject, include phase/ticket reference, etc.)
- PR description template (e.g., Summary + Test Plan sections, checklist, link to spec, etc.)
- Any other conventions for branch naming, commit scope, or PR labels

Write specs/conventions.md with sections: Commit Messages, Pull Requests, Branch Naming. Include concrete examples for each format so the conventions are unambiguous.

## Step 6: Generate project-specific workflow commands

Create five slash commands under .claude/commands/, customized to this project's tech stack and conventions. Each generated command should be a concise markdown file containing imperative instructions for Claude Code — similar in style to this bootstrap command.

Generate commands only for the roadmap mode the user selected. Each command should contain concrete instructions for that mode (e.g., `gh issue list` commands with the exact label, or `specs/roadmap.md` parsing), not conditional logic for both modes.

### .claude/commands/sdd-plan-next-phase.md

Generate a command that:
- Checks for uncommitted changes; if any, uses AskUserQuestion to ask whether to stash or abort
- Checks out main and pulls latest
- Finds the next phase. For file-based roadmaps: parse `## Phase N: <name>` headings from specs/roadmap.md, list existing specs/*-phase-N-*/ directories, and pick the first phase whose number doesn't have a matching spec directory yet. For GitHub-issue-driven roadmaps: embed the user's configuration (phase label, additional labels, next-phase selection method, auto-assign setting) directly into the command, and use `gh issue list` to find the next phase issue accordingly. If auto-assign is enabled, assign the issue to the current user via `gh issue edit`
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
- Checks internal consistency and consistency with governing specs (for file-based roadmaps, includes specs/roadmap.md; for GitHub-issue-driven roadmaps, reads the phase issue body for context)
- Checks whether CLAUDE.md or governing specs need updating
- Uses AskUserQuestion for multi-option issues
- Applies straightforward fixes directly

### .claude/commands/sdd-ship.md

Generate a command organized into three phases:

**Phase 1 — Verify:**
- Run the project's actual check command
- Run the project's build/container verification if applicable (e.g., `make docker-build` — only if Docker was set up in tech stack)
- Verify validation criteria from the phase spec (if one exists)
- Check CLAUDE.md freshness
- Run the project's format command

**Phase 2 — Commit:**
- Read specs/conventions.md for commit and PR format
- Use AskUserQuestion to confirm before committing
- Handle squashing post-review fixes into a single commit

**Phase 3 — Publish:**
- Use AskUserQuestion to confirm before pushing
- Push the branch and create or update the PR following the project's conventions
- For GitHub-issue-driven roadmaps: embed the user's configured close keyword directly into the command and add it to the PR body (e.g., "Closes #N", "Fixes #N", or "Resolves #N") so the issue closes automatically when the PR merges. If auto-close is disabled, skip this

### .claude/commands/sdd-ideate.md

Generate a command that:
- Accepts an optional starting idea or theme via $ARGUMENTS
- Reads specs/mission.md and specs/tech-stack.md to understand the project context
- For file-based roadmaps: also reads specs/roadmap.md to see existing phases
- For GitHub-issue-driven roadmaps: uses `gh issue list` with the embedded phase label to see existing phases
- Scans existing phase spec directories under specs/ to understand what's been planned and done
- Summarizes the current state briefly: what exists, where the roadmap currently ends
- Uses AskUserQuestion to brainstorm new phases with the user — exploring problems, gaps, user requests, technical debt, and unaddressed mission goals
- Proposes candidate phases, each with a short name, one-line description, 3-6 deliverable bullets, and why it matters
- Uses AskUserQuestion to iterate: split large phases, merge small ones, reorder by priority, check for dependencies, and verify none conflict with non-goals
- Once finalized, adds phases to the roadmap:
  - **File-based:** appends to specs/roadmap.md using `## Phase N: <name>` headings with deliverable bullets, continuing from the last existing phase number
  - **GitHub-issue-driven:** creates each phase as a GitHub issue via `gh issue create` using the embedded configuration (phase label, additional labels, issue template, title format), with deliverable bullets in the body
- Summarizes what was added and suggests running `/sdd-plan-next-phase` to start the next one

## Step 7: CLAUDE.md

Create a CLAUDE.md at the project root with:
- Project overview (one paragraph)
- Architecture summary (if applicable, from tech stack)
- Key design principles (from mission)
- How to build, test, and run (from tech stack — list the actual commands)
- Phase-based workflow description and slash command reference (from the commands generated in Step 6). For file-based roadmaps, reference specs/roadmap.md. For GitHub-issue-driven roadmaps, document the phase label and how phases are tracked as issues
- Conventions summary (from conventions)

## Step 8: Review

After writing all files, run a review pass: check for internal consistency across all documents and generated commands, gaps, contradictions, and feasibility issues. For brownfield projects, also verify that the specs accurately reflect the existing codebase. Verify the generated commands reference the correct project-specific commands. Use the AskUserQuestion tool for issues with multiple valid options. Apply straightforward fixes directly.
