Prepare this branch for merge. Follow these steps in order:

1. Run the project's full check command (e.g., `make check` or equivalent from specs/tech-stack.md) to verify all quality gates pass.
2. If a phase spec directory exists on this branch (specs/YYYY-MM-DD-phase-\*/), read its validation.md and verify all acceptance criteria are met. If no phase spec exists, skip this step.
3. Check if CLAUDE.md needs updating based on changes in this branch (new commands, architecture changes, new conventions). If so, update it.
4. Run the project's format command to ensure all files are formatted.

Report the results of steps 1-4. Then use the AskUserQuestion tool to confirm before proceeding with:

5. Check git status for uncommitted changes and the commit log for existing commits on this branch (vs main).
   - If there are no existing commits: stage all relevant files and create a new commit with a descriptive message.
   - If there are existing commits AND uncommitted changes (e.g. from review fixes): squash everything into a single commit by resetting to main, staging all changes, and creating one clean commit. Update the commit message to reflect the full scope of changes.
   - If there are existing commits and NO uncommitted changes: proceed with the existing commits as-is.
6. Push the branch (force-with-lease if the commit was squashed) and either create a new PR or update the existing PR's title and description to reflect the full scope of changes.

Return the PR URL when done.
