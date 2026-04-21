First, checkout main and pull the latest changes to ensure you're branching from an up-to-date base.

Then find the next phase on specs/roadmap.md and create a new branch. Ask me about the feature spec before writing anything. Create:

    A new directory YYYY-MM-DD-<phase-number>-feature-name under specs for this feature work
    In there:
    plan.md as a series of numbered task groups.
    requirements.md for the scope, decisions, context
    validation.md for how to know the implementation succeeded and can be merged

Refer to specs/mission.md and specs/tech-stack.md for guidance.

Important: You must use your AskUserQuestion tool, grouped on these 3, before writing to disk.

After writing the spec files, automatically run a review pass: check all files created or modified in this branch for internal consistency, consistency with governing specs, gaps, contradictions, feasibility issues, and stale references. Use the AskUserQuestion tool for issues with multiple valid options. Apply straightforward fixes directly.
