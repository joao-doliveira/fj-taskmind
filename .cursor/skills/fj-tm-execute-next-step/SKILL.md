---
name: fj-tm-execute-next-step
description: >-
  Runs the next incomplete step from the active TaskMind phase plan in
  .cursor/plans/phase-N.md. Invoke with @fj-tm-execute-next-step. One step
  per invocation — commit before running again.
---

# FJ-TM execute next step

Implement the next incomplete step from the active phase plan. One invocation = one step sized for a single commit.

## Paths

| What | Path (from fj-taskmind root) |
|------|-------------------------------|
| Active plan | `.cursor/plans/phase-N.md` |
| Journey phase file | `../fullstack-journey/projects/taskmind/phase-N.md` |

## Procedure

1. Read `.cursor/plans/phase-N.md`. If missing, tell user to invoke `@fj-plan-on-project` in Journey with the phase file attached.
2. Find the **current step** from **Task completion tracking** (`current` row, or first `todo` after last `done`).
3. Read that step's full section (goal, acceptance, alternatives).
4. Implement **only that step** in `fj-taskmind`. Apply `.cursor/rules/*`.
   - Scope the work to a **reasonable commit chunk** — incremental, structured progress, not a multi-step dump.
5. Run acceptance checks listed for the step.
6. Update the plan:
   - Mark completed step `done` in tracking table.
   - Set next step to `current` (or mark phase `review` if all done).
   - Fill **Last session** with date and brief summary.
7. List Journey checklist items to tick (do not tick unless user asks).
8. **Stop after one step.** Do not start the next step in this invocation.

## Commit boundary

Do **not** commit unless the user asks. After finishing the step, remind the user to review, refine if needed, and **commit** before invoking `@fj-tm-execute-next-step` again.

Typical loop: `@fj-tm-execute-next-step` → refine → commit → `@fj-tm-execute-next-step` → …

## Output

Summarize: what was done, suggested commit message, what to tick in Journey, suggested deep-dives, whether the phase is ready for `@fj-tm-review`.
