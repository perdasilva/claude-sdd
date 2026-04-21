Help me define the foundation for this project. We will create four governing spec documents under a specs/ directory.

Important: Use the AskUserQuestion tool at every step to gather my requirements before writing anything. Do not assume — ask.

## Step 1: Mission (specs/mission.md)

Ask me about:
- What this project does and who it's for
- Key goals (3-5 bullet points)
- What's explicitly NOT in scope (non-goals)
- Core design principles (how we make decisions)
- Development practices (quality gates, workflow conventions)

Write specs/mission.md with sections: Goals, Non-Goals, Design Principles, Development Practices.

## Step 2: Tech Stack (specs/tech-stack.md)

Ask me about:
- Language, runtime, and module system
- Core dependencies and their purpose
- Dev dependencies (testing, linting, formatting, building)
- Project structure (directory layout)
- Build and run commands (and whether to use a Makefile, npm scripts, or other task runner)
- Containerization (if applicable)
- CI/CD (if applicable)

Write specs/tech-stack.md with sections for each concern, including a project structure tree and a build commands table.

## Step 3: Roadmap (specs/roadmap.md)

Ask me about:
- How work should be phased (vertical slices vs horizontal layers vs core-then-extend)
- What the first few phases should cover
- How granular each phase should be

Write specs/roadmap.md with numbered phases, each containing 3-6 bullet points describing deliverables.

## Step 4: Commit & PR conventions (specs/conventions.md)

Ask me about:
- Commit message format (e.g., conventional commits, free-form with subject+body, single-line, etc.)
- Whether commits should include a co-author line or other trailer
- PR title format (e.g., match commit subject, include phase/ticket reference, etc.)
- PR description template (e.g., Summary + Test Plan sections, checklist, link to spec, etc.)
- Any other conventions for branch naming, commit scope, or PR labels

Write specs/conventions.md with sections: Commit Messages, Pull Requests, Branch Naming. Include concrete examples for each format so the conventions are unambiguous.

## Step 5: Review

After writing all four files, run a review pass: check for internal consistency across all documents, gaps, contradictions, and feasibility issues. Use the AskUserQuestion tool for issues with multiple valid options. Apply straightforward fixes directly.
