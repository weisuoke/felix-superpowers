## Why

The current superpowers skills (brainstorming, writing-plans, executing-plans) operate independently and store plans in `docs/plans/`. This lacks integration with structured specification workflows and makes it difficult to track the lifecycle of features from ideation to implementation.

OpenSpec provides a spec-driven development workflow that aligns humans and AI on what to build before code is written. Integrating superpowers skills with OpenSpec creates a unified workflow where designs, plans, and implementations are tracked together.

## What Changes

- **brainstorming skill**: After design completion, ask whether to create OpenSpec change documents. If yes, generate complete change structure (proposal.md, design.md, tasks.md, plan/, specs/).

- **writing-plans skill**:
  - Read context from OpenSpec change directory (proposal.md, tasks.md, design.md)
  - Output plans to `openspec/changes/<feature>/plan/YYYY-MM-DD-v<N>.md`
  - Remove mandatory commit steps from task template
  - Remove `docs/plans/` as output path

- **executing-plans skill**:
  - Read full OpenSpec change context before execution
  - Execute latest plan version from plan/ directory
  - Sync task completion status to tasks.md
  - Prompt for `openspec archive` after completion

- **New directory structure**: Add `plan/` folder to OpenSpec change structure for detailed implementation plans

## Impact

- Affected skills:
  - `skills/brainstorming/SKILL.md`
  - `skills/writing-plans/SKILL.md`
  - `skills/executing-plans/SKILL.md`

- New conventions:
  - Plans stored in `openspec/changes/<feature>/plan/YYYY-MM-DD-v<N>.md`
  - `tasks.md` serves as high-level overview
  - `plan/*.md` contains detailed implementation steps
  - Multiple plan versions supported for iterative changes
