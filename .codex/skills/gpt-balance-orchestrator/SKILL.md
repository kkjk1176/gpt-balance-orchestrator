---
name: gpt-balance-orchestrator
description: Orchestrate non-trivial software implementation, feature changes, refactors, and test additions as cohesive feature units using TDD, isolated child-agent work, report-driven completion handling with exception tracking across delegation hierarchies, independent reviews, and targeted improvement loops. Use when a request benefits from planning and executing one or more code-changing functional units; apply selectively rather than for read-only questions or trivially safe edits.
---

# GPT Balance Orchestrator

Apply this workflow only when it improves confidence or throughput. Do not force it onto read-only work, a clearly trivial isolated edit, or a user request that explicitly asks for a different process. Treat a feature unit as one cohesive behavior with a testable acceptance criterion; do not split merely by file, class, or individual method.

## 1. Shape the work

1. Read applicable repository instructions and inspect the relevant code and tests.
2. State the acceptance criteria, affected boundaries, test command, and risk factors.
3. Divide the change into the smallest **cohesive feature units** that can be independently implemented and reviewed. Keep a unit together when its parts require the same behavioral context.
4. Build a dependency and path-ownership map. Run units in parallel only when their writable paths and behavioral contracts do not overlap. Otherwise run them in dependency order.
5. Assign a route from [Routing](#routing) to every unit and to each meaningful delegated subtask. Choose the higher-risk route when purposes overlap.

Use child-agent sessions for feature units whenever the environment exposes child-agent delegation. Give each session a bounded brief containing the goal, acceptance tests, owned paths, forbidden/adjacent paths, dependencies, validation command, assigned route, and named completion-report channel. Select the channel before delegation: automatic final-result delivery, an explicit parent-message/report operation, or a parent-side wait/status/read operation. A request to “report back” without a named channel is not a completion contract. In shared-workspace environments, never delegate overlapping writes concurrently. If separate user-owned tasks are available only on explicit user request, use child agents rather than creating user-owned tasks without that request.

## Delegation lifecycle

Treat delegation as a parent-owned lifecycle, not a fire-and-forget handoff. Use child-to-parent final reports as the default completion mechanism; use active status tracking only as an exception. Before delegation, select and record the completion-report channel and its recovery action. The agent that creates a child remains responsible for evaluating the child's final result and must not treat the work as complete until the parent has observed the report through the named channel or received an explained blocker.

- **Main session:** Own the overall plan and every delegated workstream. Maintain lightweight state for each workstream: owner, expected result, dependency or risk level, named completion-report channel, recovery action, and the condition that makes a delayed report actionable. Continue integration work while awaiting reports; do not continuously poll healthy children. A final report counts only after the main session observes automatic delivery or retrieves it through the named channel. Do not finalize the user request or report completion while a required final report is absent. Consolidate reported results, resolve dependencies and conflicts, and communicate the integrated outcome.
- **Child session:** Complete its bounded assignment and may delegate non-overlapping subtasks. Require a final report from each delegated child before treating the corresponding subtask as complete, but avoid continuous polling when it is healthy. Incorporate those reports, run assigned validation, and before ending submit a concise final report through the named completion-report channel. Include the outcome, changed paths, checks run, unresolved risks, and any blocked work. If the channel fails or is unavailable, use the nearest available parent-report channel to report that blocker instead of silently ending.
- **Child agent:** Complete its assigned scope, run required validation where possible, and before ending submit its final outcome through the named completion-report channel. Do not silently stop after partial progress; explicitly report blockers, failed checks, remaining work, or a report-channel failure through the nearest available parent-report channel.

Actively check a child's status only when its expected reporting point has passed, it emits a failure or blocker signal, an upstream dependency needs an answer, or the work is high risk. Use bounded status waits rather than unbounded blocking waits. If the expected report is absent, run the recorded recovery action to retrieve the final result or confirm the report-channel failure. If a child is blocked, fails, or returns an incomplete result, keep ownership at the parent: clarify, retry, re-route, or escalate before closing the parent assignment. Report upward only after the delegated subtree has final reports observed by the parent with evaluated results, or has a clearly explained, user-visible blocker that prevents further progress.

## 2. Implement each feature unit

For every unit, use this loop in order:

1. **Red:** add or update the smallest meaningful automated test that expresses the acceptance criterion, then run it and confirm the intended failure. When a red test cannot be produced (for example, an external-only change), explain why and use the closest executable check.
2. **Green:** implement only enough production code to satisfy the test; run the focused test.
3. **Refactor:** improve the design only while the relevant tests remain green; rerun the focused test.
4. **Review:** have an independently routed Critic or Test review examine the diff, acceptance criteria, regressions, edge cases, and test quality. Do not treat the implementer's self-check as the required review.
5. **Improve:** route review fixes as Executor work when bounded and clear; otherwise use the escalated route. Rerun the focused tests and repeat review if the fixes materially change behavior.

Delegate a subtask only when it has a crisp output and owned paths, such as test design, a bounded implementation, failure triage, or focused review. Keep cross-unit coordination, merge decisions, and risk acceptance with the parent agent. Record the review outcome and any intentionally deferred risk before moving on.

## 3. Integrate and close

1. Integrate completed units in dependency order and resolve interface mismatches with the owner of the affected unit.
2. Run all focused checks plus the smallest relevant integration, lint, type, or build gate. Add or strengthen regression coverage when a defect or review finding revealed a gap.
3. Route security, authorization, payment, data deletion/migration, or deployment changes through Final reviewer before finalizing.
4. Report completed units, tests run and results, review findings/fixes, and remaining risks or checks that could not run.

## Routing

Use the following default model and reasoning level. Set an explicit model override only when the delegation API supports it. When the delegation API exposes Luna as selectable for a Luna route, use Luna. If the delegation API does not expose Luna as selectable, use Terra at the stated reasoning level and disclose the substitution in the handoff. For any other requested model that the delegation API does not expose, choose the lowest available model capable of achieving the route's intent and purpose, and say so in the handoff.

| Purpose | Default model | Reasoning | Use for |
| --- | --- | --- | --- |
| Default | GPT-5.6 Terra | High | General development, analysis, documentation |
| Architect | GPT-5.6 Sol | High | System design, technology choices, complex structure |
| Executor | GPT-5.6 Luna | Medium | Small, clear implementation or repeat edits |
| Planner | GPT-5.6 Terra | Medium | Unit breakdown, scope, execution order |
| Critic | GPT-5.6 Terra | XHigh | Requirement, logic, and test-perspective review |
| Deep problem-solver | GPT-5.6 Sol | XHigh | Unknown-cause bugs, hard refactors, algorithms |
| Research / synthesis | GPT-5.6 Terra | High | Comparing evidence and deriving a conclusion |
| Fast triage | GPT-5.6 Luna | Low | First-pass logs, classification, simple drafts |
| Final reviewer | GPT-5.6 Sol | XHigh | Security, access, payment, data loss, release review |
| Test implementation | GPT-5.6 Terra | High | Unit/integration/regression tests |
| Test triage | GPT-5.6 Luna | Medium | Failure reproduction and simple fixes |
| Test diagnosis | GPT-5.6 Sol | High | Flaky, integration, or E2E failure diagnosis |
| Test review | GPT-5.6 Terra | XHigh | Coverage gaps, edge cases, missing validation |

## Escalation rules

- Promote Executor work to Terra High or stronger when it spans multiple files/services or its cause is unclear.
- Use Final reviewer for security, authorization, payment, data deletion/migration, and deployment changes.
- Promote to Deep problem-solver after two failed approaches or when the cause is still not narrowed.
- Do not start simple work at XHigh. Start lower and promote only with evidence of uncertainty or quality risk.
- Before final changes, run the necessary tests and validation. Explicitly name residual risks and unverified items.
