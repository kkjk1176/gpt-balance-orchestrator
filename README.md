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

The preferred models are `gpt-6-luna`, `gpt-6.1-terra`, `gpt-6.2-sol`, and `gpt-6.2-astra`. GPT-6.2 Sol and Astra [launched on November 17, 2026](https://learn.chatgpt.com/docs/whats-new/gpt-62-astra-sol); routes were checked on November 18.

| Task | Default route |
| --- | --- |
| First-pass triage / mechanical edits | Luna Low |
| Prescribed implementation / test updates | Luna Medium |
| Visual UI diagnosis | GPT-6.1 Terra High |
| Bounded browser / visual checks | GPT-6.1 Terra Medium |
| Routine feature-documentation rewriting | GPT-6.1 Terra Low |
| Ordinary features / tests / planning / coordination | GPT-6.2 Sol Medium |
| Code diagnosis / architecture / cross-system work / independent or final review | GPT-6.2 Sol High |
| Consequential reasoning blocker after Sol diagnosis and independent review | GPT-6.2 Astra High |

These are task-specific defaults; adjust when the task's demands or results justify it. XHigh or Max requires an explicit user request or demonstrated quality gains on comparable tasks. Ultra requires an explicit user request, runtime support, and permission for automatic delegation. Astra above High always requires an explicit user request.

Unless explicitly requested by the user, Astra High remains reserved for a specific, consequential reasoning blocker after focused Sol High diagnosis and independent Sol review. Complexity, risk, or missing information or tools alone does not justify escalation. Limit Astra to the unresolved question and return execution to Sol.

Check the dispatch tool's actual model and effort options. If Luna is absent, use Terra for triage/mechanical edits or Sol for implementation/tests. If Terra is absent, use Sol with the required tools. Previous `gpt-6.1-sol` and `gpt-6-astra` are availability fallbacks for their GPT-6.2 counterparts. Report substitutions and retain the Astra gate. The skill guides routing; it does not switch the running session's model. See the [full routing policy](.codex/skills/gpt-balance-orchestrator/SKILL.md#routing) and [model evidence](.codex/skills/gpt-balance-orchestrator/references/models.md).

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
