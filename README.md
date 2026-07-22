# GPT Balance Orchestrator

[한국어로 읽기](README.ko.md)

`GPT Balance Orchestrator` is a Codex skill for turning a meaningful software change into a well-managed feature workflow. It helps an agent plan, implement, review, and verify a feature as one coherent piece of work—without treating every tiny edit as a full-scale project.

## What it is for

Use this skill when a request involves a non-trivial feature, refactor, or test addition and you want dependable progress rather than a pile of disconnected edits. It is particularly useful when work needs clear acceptance criteria, focused tests, careful delegation, or an independent review before it is considered done.

For a small documentation tweak or an obviously safe one-line change, the skill intentionally stays lightweight. Good engineering judgment includes knowing when *not* to add process.

## How it works

The workflow keeps ownership and verification clear from start to finish:

1. **Shape the feature.** The agent identifies the acceptance criteria, affected boundaries, test command, risks, and the smallest cohesive units of work. If work is delegated, each agent gets an explicit scope and the parent remains responsible for the final result.
2. **Build with a feedback loop.** Each unit follows a Red → Green → Refactor cycle: first express the intended behavior in a focused test, then implement the minimum change, then improve the design while the test stays green. For work that cannot be tested this way, the closest practical executable check is used instead.
3. **Review independently.** A separate reviewer checks the diff for requirement gaps, regressions, edge cases, and test quality. Clear, bounded findings are fixed and rechecked; uncertain or higher-risk issues are escalated rather than guessed at.
4. **Integrate and close.** Completed units are combined in dependency order, relevant focused and integration checks run, and any remaining risk or unverified item is reported plainly.

This makes the process visible: someone is always accountable for coordination, implementation, review, and validation.

## Built-in routing

The skill matches work to the right level of effort. It uses lighter execution for small, clear changes and stronger planning, critique, diagnosis, or final review for work with broader impact. Security, authorization, payments, data deletion or migration, and deployment changes receive a final high-scrutiny review.

It also avoids the common delegation trap: child agents report their result—including checks run, changed paths, and blockers—back to the parent. The parent does not mark the feature complete until those results have been evaluated.

## How to use it

Describe the outcome you want in plain language. For example:

```text
Add CSV export for the billing report. Include permission checks, regression tests,
and a review of the error states.
```

Or ask explicitly for the workflow when the change is substantial:

```text
Use GPT Balance Orchestrator to refactor the notification pipeline safely.
```

The skill will help turn that request into cohesive feature units, select an appropriate implementation and review route, and report the checks and any remaining risks when it finishes.

## What you can expect

- A clear description of what “done” means before implementation begins.
- Tests or the closest relevant executable validation for each meaningful change.
- Independent review instead of relying only on the implementer’s self-check.
- A concise completion report covering changes, verification, findings, and residual risks.

The goal is not ceremony. It is to make non-trivial changes easier to trust, easier to review, and easier to improve.

## License

This project is available under the [MIT License](LICENSE).
