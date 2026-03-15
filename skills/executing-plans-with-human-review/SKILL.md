---
name: executing-plans-with-human-review
description: Use when executing implementation plans where each task requires explicit human approval before it's marked as completed, and plan changes from review must be reconciled with the design document
---

# Executing Plans with Human Review

## Overview

Same as executing-plans, but with a mandatory human review gate before marking any task complete. If the reviewer requests changes, assess plan impact and update the plan document before implementing.

**Announce at start:** "I'm using the executing-plans-with-human-review skill to implement this plan."

**Note:** Subagents significantly improve execution quality. Use superpowers:subagent-driven-development when available.

## The Process

### Step 1: Load and Review Plan

1. Read plan file
2. Review critically — identify questions or concerns
3. If concerns: Raise with human partner before starting
4. If no concerns: Create TodoWrite and proceed

### Step 2: Execute Tasks

For each task:

1. Mark as `in_progress`
2. Follow each step exactly (plan has bite-sized steps)
3. Run verifications as specified
4. Request human review: present what was done and ask: "Please review this task. Approve to continue, or describe any changes needed."
5. If changes requested:
   a. Assess whether the requested change impacts the design or implementation plan
   b. If yes: Update the plan document to reflect the new direction before implementing
   c. Implement the requested changes
   d. Re-run verifications
   e. Return to sub-step 4 (request review again)
6. If approved: Mark as `completed` and proceed to the next task

> **CRITICAL:** Never mark a task as `completed` before receiving explicit human approval.

### Step 3: Complete Development

After all tasks complete and approved:

- Announce: "I'm using the finishing-a-development-branch skill to complete this work."
- **REQUIRED SUB-SKILL:** Use superpowers:finishing-a-development-branch
- Follow that skill to verify tests, present options, execute choice

## Plan Impact Assessment (Step 2.5b)

When a reviewer requests changes, ask:
- Does this change a public interface, data model, or architectural decision stated in the plan?
- Does it affect tasks not yet executed?
- Does it invalidate assumptions the plan relies on?

If **yes to any**: Update the plan file first, then implement. Note the change inline with a comment explaining why.

If **no to all**: Implement directly without touching the plan.

## When to Stop and Ask for Help

**STOP immediately when:**
- Blocker: missing dependency, failing test, unclear instruction
- Plan has critical gaps preventing progress
- A change request contradicts a core plan assumption (surface the conflict before proceeding)
- Verification fails repeatedly

**Ask for clarification rather than guessing.**

## Red Flags — STOP Before Marking Complete

| Thought | Reality |
|---|---|
| "Tests pass, so the task is done" | Tests passing ≠ human approval. Wait for review. |
| "The user asked me to be autonomous" | Autonomy means executing the plan, not skipping review gates. |
| "Review would interrupt the flow" | That interruption is the point of this skill. |
| "The change is small, no need to update the plan" | Small changes drift plan from reality. Assess first. |
| "I'll reconcile the plan at the end" | Plan divergence compounds. Update on each change. |
| "I already checked this thoroughly" | Your review ≠ human approval. Still wait. |

## When to Revisit Earlier Steps

**Return to Step 1 (Review)** when:
- A change request fundamentally alters the overall approach
- Multiple related upcoming tasks need replanning

**Don't force through blockers** — stop and ask.

## Remember

- Never mark `completed` without explicit human approval
- Assess plan impact before implementing any reviewer-requested change
- Update the plan document when changes affect design or future tasks
- Never start implementation on main/master without explicit user consent
- Reference skills when the plan says to

## Integration

**Required workflow skills:**
- **superpowers:using-git-worktrees** — REQUIRED: Set up isolated workspace before starting
- **superpowers:writing-plans** — Creates the plan this skill executes
- **superpowers:finishing-a-development-branch** — Complete development after all tasks
