## 1. Brainstorming Skill Modification

- [x] 1.1 Add OpenSpec prompt flow after design completion
- [x] 1.2 Implement OpenSpec change structure generation (proposal.md, design.md, tasks.md, plan/, specs/)
- [x] 1.3 Extract proposal.md content from design discussion (Why, What Changes, Impact)
- [x] 1.4 Generate tasks.md high-level task checklist from design
- [x] 1.5 Remove docs/plans/ output path

## 2. Writing-plans Skill Modification

- [x] 2.1 Add OpenSpec change directory context reading (proposal.md, tasks.md, design.md)
- [x] 2.2 Change output path to openspec/changes/<feature>/plan/YYYY-MM-DD-v<N>.md
- [x] 2.3 Implement automatic version number detection (v1, v2...)
- [x] 2.4 Update plan template with OpenSpec Change reference
- [x] 2.5 Remove mandatory commit steps from task template

## 3. Executing-plans Skill Modification

- [x] 3.1 Add OpenSpec change directory location logic
- [x] 3.2 Implement full context reading (proposal.md, tasks.md, design.md)
- [x] 3.3 Implement latest plan version detection from plan/ directory
- [x] 3.4 Implement tasks.md status sync (update [x] after batch completion)
- [x] 3.5 Add archive prompt after completion

## 4. Documentation and Testing

- [x] 4.1 Update design documentation
- [x] 4.2 Verify integrated workflow across three skills
