---
name: gpt-balance-orchestrator
description: Match models and reasoning effort to non-trivial code changes, with TDD, selective delegation, and independent review. Do not use for read-only work or trivial isolated edits.
---

# GPT Balance Orchestrator

Improve token efficiency by matching model capability and reasoning effort to each task's requirements and quality bar, avoiding unnecessary use of stronger models.

Use this only for non-trivial code-changing features, refactors, or test additions. A feature unit is one cohesive, testable behavior—not a file or method. Do not force this process on read-only work, documentation-only work, trivial isolated edits, or a user-requested different process.

## Execution and delegation

The main session owns scope, ownership, sequencing, rerouting, and risk acceptance. Delegate execution only when parallelism or context separation provides a concrete benefit; otherwise perform the continuous workflow directly. Independent review is required in either case.

- Divide work into the smallest cohesive units. Parallelize only units with no overlapping writable paths or behavioral contracts; otherwise sequence them.
- Give each child a bounded brief with its goal, acceptance criteria, owned and adjacent/forbidden paths, dependencies, validation command, route, and completion-report channel. Use child agents rather than user-owned tasks unless the user explicitly requests a separate task.
- Agents may resolve concrete issues directly with peer owners within assigned scopes. Escalate unresolved blockers and proposed scope, ownership, or shared-contract changes to the parent, who decides those changes before affected work proceeds.
- The parent must observe and evaluate every child report before completion. Use automatic final delivery by default; recover only after a missed expected report, blocker, upstream need, or high-risk signal. Keep implementation and tests together when they need the same behavioral context.
- A child report must state the outcome, changed paths, checks run, and risks or blockers. The main session integrates those decisions; bounded review fixes return to the current owner, while broader or unresolved work is rerouted.

## Build, review, and close

For each meaningful testable behavior, the owner runs the smallest Red → Green → Refactor loop. If Red is not meaningful, explain why and run the closest executable check.

Obtain one independent review per cohesive feature or integration outcome, covering acceptance criteria, regressions, edges, and test quality. Security, authorization, payments, data deletion, migrations, and deployments require a Final reviewer. The owner runs focused checks plus the smallest relevant integration, lint, type, or build gate. Stop when required checks pass; broaden or repeat them only after a new change, failure, or unresolved concern.

## Routing

Use only `gpt-6-luna`, `gpt-6.1-terra`, `gpt-6.1-sol`, or `gpt-6-astra` for new sessions and child agents. Choose model and reasoning effort by task fit and required quality, considering complexity, uncertainty, and risk, subject to the Astra exception below. If no eligible model is available, report a blocker.

| Decision | Route |
| --- | --- |
| First-pass triage or mechanical edits | `gpt-6-luna` at Low |
| Prescribed implementation or test updates with clear acceptance criteria | `gpt-6-luna` at Medium |
| Scoped code or test diagnosis, or ambiguous visual UI diagnosis | `gpt-6.1-terra` at High |
| Bounded browser verification or visual UI checks with clear criteria | `gpt-6.1-terra` at Medium |
| Routine feature-documentation rewriting from supplied evidence | `gpt-6.1-terra` at Low |
| Ordinary feature work, tests, planning, or routine coordination | `gpt-6.1-sol` at Medium |
| Architecture, difficult diagnosis, or complex cross-system work | `gpt-6.1-sol` at High |
| Independent code or test review, including high-risk final review | `gpt-6.1-sol` at High |
| Exceptional unresolved reasoning blocker | `gpt-6-astra` at High |

Use these as task-specific defaults; adjust when the task's demands or results justify it. XHigh requires an explicit user request or demonstrated quality gains on comparable tasks.

Unless explicitly requested by the user, Astra is reserved for a specific, consequential reasoning blocker that remains after focused Sol High diagnosis and independent Sol review. Complexity or risk alone does not qualify; missing information or tools does not justify escalation. Give Astra only the relevant evidence and the unresolved question, then return execution to Sol. Use Astra XHigh only when explicitly requested by the user.
