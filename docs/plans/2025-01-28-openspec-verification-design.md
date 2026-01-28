# OpenSpec Verification Design

## Overview

Add a PRD consistency verification step between `verification-before-completion` and `finishing-a-development-branch` in the development workflow.

## Problem

Currently, the workflow verifies that tests pass but does not verify that:
1. All PRD requirements are implemented
2. Implementation matches PRD descriptions
3. OpenSpec documents are internally consistent

## Solution

Create a new skill `openspec-verification` that dispatches a subagent to perform three checks:

1. **Document Consistency** - Compare OpenSpec documents for conflicts
2. **PRD Completeness** - Verify all requirements are implemented
3. **PRD Consistency** - Verify implementation matches PRD descriptions

## Deliverables

| File | Purpose |
|------|---------|
| `skills/openspec-verification/SKILL.md` | Main skill file |
| `skills/openspec-verification/verifier-prompt.md` | Subagent prompt template |
| `commands/verify-openspec.md` | Slash command entry |
| `diagram/v3.md` | Updated workflow diagram |

## Workflow Position

```
verification-before-completion (pass)
        ↓
openspec-verification  ← NEW
        ↓
    Pass? ──No──→ Fix issues → Back to implementation
        ↓ Yes
finishing-a-development-branch
```

## Trigger Methods

1. **Automatic:** After `verification-before-completion` passes in workflow
2. **Manual:** `/verify-openspec` slash command

## Subagent Checks

### 1. Document Consistency Check

Compare document pairs:
- sources.md ↔ proposal.md
- proposal.md ↔ tasks.md
- tasks.md ↔ design.md (if exists)

Look for contradictions, scope mismatches, missing items.

### 2. PRD Completeness Check

Extract requirements from sources.md/proposal.md.
Verify each requirement has corresponding implementation in code.

### 3. PRD Consistency Check

For implemented features, verify behavior matches PRD description.
Read actual code, don't trust assumptions.

## Output Format

```markdown
### Document Consistency
| Document Pair | Status | Issues |
|---------------|--------|--------|
| sources.md ↔ proposal.md | ✅/❌ | ... |

### PRD Completeness
| Requirement | Status | Code Location |
|-------------|--------|---------------|
| Feature A | ✅ Implemented | src/a.ts:10-25 |

### PRD Consistency
| Requirement | PRD Description | Actual Implementation | Status |
|-------------|-----------------|----------------------|--------|
| Feature A | "User can..." | "Code does..." | ✅/⚠️/❌ |

### Verdict
**Status:** PASS / FAIL
**Summary:** [1-2 sentences]
**Blocking Issues:** (if FAIL)
```

## Architecture

```
User/Workflow triggers
       ↓
┌─────────────────────────────────┐
│  openspec-verification skill    │
│  1. Locate OpenSpec change dir  │
│  2. Collect files to review     │
│  3. Get git diff (BASE..HEAD)   │
│  4. Dispatch verifier subagent  │
└─────────────────────────────────┘
       ↓
┌─────────────────────────────────┐
│  verifier subagent              │
│  - Read OpenSpec documents      │
│  - Read code implementation     │
│  - Execute three checks         │
│  - Output verification report   │
└─────────────────────────────────┘
       ↓
   Return Pass/Fail + Report
```

## V3 Workflow Changes

| Step | Change |
|------|--------|
| openspec-verification | NEW: PRD consistency verification step |
| Check content | Document consistency, completeness, implementation consistency |
| Trigger | Auto in workflow OR `/verify-openspec` manual |
