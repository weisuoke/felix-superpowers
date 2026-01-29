---
name: executing-plans
description: Use when you have a written implementation plan to execute in a separate session with review checkpoints
---

# Executing Plans

## Overview

Load plan, review critically, execute tasks in batches, report for review between batches.

**Core principle:** Batch execution with checkpoints for architect review.

**Announce at start:** "I'm using the executing-plans skill to implement this plan."

**OpenSpec Integration:**
- If plan is in `openspec/changes/<feature>/plan/`, read full context first:
  - `proposal.md` - Understand "why"
  - `tasks.md` - High-level task overview
  - `design.md` - Technical decisions (if exists)
- Select latest plan version from plan/ directory (by date and version number)
- Announce: "Reading OpenSpec change context from `openspec/changes/<name>/`..."

## The Process

### Step 1: Load and Review Plan

**If OpenSpec change exists:**
1. Read `openspec/changes/<feature>/proposal.md` - understand purpose
2. Read `openspec/changes/<feature>/tasks.md` - understand high-level tasks
3. Read `openspec/changes/<feature>/design.md` - understand technical decisions (if exists)
4. Read latest plan from `openspec/changes/<feature>/plan/` (sort by date + version)

**Then:**
1. Review plan critically - identify any questions or concerns
2. If concerns: Raise them with your human partner before starting
3. If no concerns: Create TodoWrite and proceed

### Step 2: Execute Batch
**Default: First 3 tasks**

For each task:
1. Mark as in_progress
2. Follow each step exactly (plan has bite-sized steps)
3. Run verifications as specified
4. Mark as completed

### Step 3: Report
When batch complete:
- **Update tasks.md:** Mark corresponding high-level tasks as complete `[x]`
  - Plan Task N corresponds to tasks.md section N.x
  - After completing Task N, mark all N.x tasks in tasks.md as `[x]`
- Show what was implemented
- Show verification output
- Say: "Ready for feedback."

### Step 4: Continue
Based on feedback:
- Apply changes if needed
- Execute next batch
- Repeat until complete

### Step 5: Complete Development

After all tasks complete and verified:
- **If OpenSpec change:** Ensure all tasks in tasks.md are marked `[x]`
- **If OpenSpec change:** Announce: "All tasks complete. Run `openspec archive <change-name>` to archive this change."
- **If OpenSpec change:** Use `superpowers:openspec-verification` to verify PRD compliance
  - Only proceed if verification passes
- Announce: "I'm using the finishing-a-development-branch skill to complete this work."
- **REQUIRED SUB-SKILL:** Use superpowers:finishing-a-development-branch
- Follow that skill to verify tests, present options, execute choice

## When to Stop and Ask for Help

**STOP executing immediately when:**
- Hit a blocker mid-batch (missing dependency, test fails, instruction unclear)
- Plan has critical gaps preventing starting
- You don't understand an instruction
- Verification fails repeatedly

**Ask for clarification rather than guessing.**

## When to Revisit Earlier Steps

**Return to Review (Step 1) when:**
- Partner updates the plan based on your feedback
- Fundamental approach needs rethinking

**Don't force through blockers** - stop and ask.

## Remember
- Review plan critically first
- Follow plan steps exactly
- Don't skip verifications
- Reference skills when plan says to
- Between batches: just report and wait
- Stop when blocked, don't guess
