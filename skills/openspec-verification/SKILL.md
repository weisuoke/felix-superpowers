---
name: openspec-verification
description: Use after verification-before-completion passes, before finishing-a-development-branch - verifies PRD completeness, implementation consistency, and OpenSpec document alignment
---

# OpenSpec Verification

## Overview

Verify that implementation matches PRD requirements and OpenSpec documents are internally consistent.

**Core principle:** No completion without PRD alignment verification.

**Announce at start:** "Using openspec-verification to check PRD compliance."

## When to Use

**Mandatory:**
- After verification-before-completion passes
- Before finishing-a-development-branch

**Optional:**
- Anytime you want to check current implementation against PRD
- After major implementation milestones

## The Process

### Step 1: Locate OpenSpec Change

```bash
# Find active change directory
ls openspec/changes/
```

If multiple active changes exist, ask user which one to verify.

If no OpenSpec change exists:
```
No OpenSpec change found. Skipping PRD verification.
Proceeding to finishing-a-development-branch.
```

### Step 2: Collect Context

Gather these files (if they exist):
- `openspec/changes/<name>/sources.md`
- `openspec/changes/<name>/proposal.md`
- `openspec/changes/<name>/tasks.md`
- `openspec/changes/<name>/design.md`

At minimum, proposal.md must exist. If not found:
```
OpenSpec change <name> has no proposal.md. Cannot verify PRD compliance.
```

### Step 3: Get Git Diff

```bash
BASE_SHA=$(git merge-base HEAD main 2>/dev/null || git merge-base HEAD master)
HEAD_SHA=$(git rev-parse HEAD)
git diff --stat $BASE_SHA..$HEAD_SHA
```

### Step 4: Dispatch Verifier Subagent

Use Task tool with `superpowers:openspec-verifier` type:

```
Task tool:
  subagent_type: "superpowers:openspec-verifier"
  description: "Verify PRD compliance for <change-name>"
  prompt: |
    Verify implementation against OpenSpec change: <change-name>

    Git range: <BASE_SHA>..<HEAD_SHA>

    OpenSpec files to review:
    - openspec/changes/<name>/sources.md
    - openspec/changes/<name>/proposal.md
    - openspec/changes/<name>/tasks.md
    - openspec/changes/<name>/design.md
```

See `verifier-prompt.md` for full subagent instructions.

### Step 5: Report Results

**If PASS:**
```
OpenSpec verification passed.

[Include subagent report summary]

Proceeding to finishing-a-development-branch.
```

**If FAIL:**
```
OpenSpec verification failed.

[Include full subagent report]

Blocking issues must be resolved before proceeding.
```

Do NOT proceed to finishing-a-development-branch until all blocking issues are resolved.

## Integration

**Called by:**
- Workflow: After verification-before-completion passes
- Manual: `/verify-openspec` command

**Calls:**
- `superpowers:openspec-verifier` subagent

**Followed by:**
- `finishing-a-development-branch` (if pass)
- Back to implementation (if fail)

## Red Flags

**Never:**
- Skip verification because "it's a small change"
- Proceed with FAIL status
- Trust implementation without reading code

**Always:**
- Run verification before completing work
- Address all blocking issues
- Re-run verification after fixes

## Quick Reference

| Check | What | Pass Criteria |
|-------|------|---------------|
| Document Consistency | OpenSpec docs align | No contradictions |
| PRD Completeness | All requirements implemented | 100% coverage |
| PRD Consistency | Implementation matches description | Behavior aligns |
