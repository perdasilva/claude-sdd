# sdd — Spec-Driven Development

A Claude Code plugin that bootstraps spec-driven development for any project. It creates governing spec documents and generates project-specific workflow commands customized to your tech stack.

## Commands

### `/sdd:bootstrap`

Interactively set up spec-driven development in the current project. It detects whether the project is greenfield or brownfield, walks you through defining mission, tech stack, roadmap, and conventions as spec documents, then generates four project-specific workflow commands (`/sdd-plan-next-phase`, `/sdd-implement`, `/sdd-review`, `/sdd-ship`) customized to your build tools and PR conventions.

Accepts an optional initial description to front-load project context (e.g., `/sdd:bootstrap A REST API in Python using FastAPI...`). When provided, Claude pre-fills what it can and focuses questions on gaps.

Safe to re-run — it will ask whether to update, overwrite, or abort if specs already exist.

See [skills/bootstrap/SKILL.md](skills/bootstrap/SKILL.md) for full documentation.

## Installation

```bash
claude plugin marketplace add perdasilva/claude-sdd
claude plugin install sdd@claude-sdd
```
