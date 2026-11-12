# GPT Balance Orchestrator

[한국어로 읽기](README.ko.md)

`GPT Balance Orchestrator` is a Codex skill for non-trivial code-changing features, refactors, and test additions. It improves token efficiency by matching model capability and reasoning effort to each task's requirements and quality bar, avoiding unnecessary use of stronger models.

It delegates execution only when parallelism or context separation provides a concrete benefit; otherwise the main session performs the continuous workflow directly. Independent review is required in either case.

## What it preserves

- Parallel work never has overlapping writable paths or behavioral contracts.
- The parent observes and evaluates every child report before completion.
- Each meaningful behavior uses the smallest practical Red → Green → Refactor loop.
- Every feature or integration outcome gets independent review; security, authorization, payments, data deletion, migrations, and deployments also get a Final review.

Child reports state the outcome, changed paths, checks, and risks or blockers. The parent integrates those findings and reports the result, review findings or fixes, and residual risk.

## Routing

New sessions and child agents use only `gpt-6-luna`, `gpt-6.1-terra`, `gpt-6.1-sol`, or `gpt-6-astra`. Use Luna Low for first-pass triage or mechanical edits and Medium for prescribed implementation or test updates. Use GPT-6.1 Terra High for scoped code or test diagnosis and ambiguous visual UI diagnosis, Medium for bounded browser verification or visual UI checks with clear criteria, and Low for routine feature-documentation rewriting from supplied evidence. Use GPT-6.1 Sol Medium for ordinary feature work, tests, planning, or routine coordination, and High for architecture, difficult diagnosis, complex cross-system work, or independent review, including high-risk final review.

These are task-specific defaults; adjust when the task's demands or results justify it. XHigh requires an explicit user request or demonstrated quality gains on comparable tasks.

Unless explicitly requested by the user, Astra High is reserved for a specific, consequential reasoning blocker that remains after focused Sol High diagnosis and independent Sol review. Complexity, risk, or missing information or tools alone does not justify escalation. Limit Astra to the unresolved question and return execution to Sol. Astra XHigh requires an explicit user request. See the [full routing policy](.codex/skills/gpt-balance-orchestrator/SKILL.md#routing).

## How to use it

Describe the outcome you want, for example:

```text
Add CSV export for the billing report. Include permission checks, regression tests,
and a review of the error states.
```

Or explicitly request the workflow:

```text
Use GPT Balance Orchestrator to refactor the notification pipeline safely.
```

## License

This project is available under the [MIT License](LICENSE).
