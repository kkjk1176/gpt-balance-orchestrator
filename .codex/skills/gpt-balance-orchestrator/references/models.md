# Model evidence and availability

Checked on 2026-11-18. Recheck official model guidance and the current runtime's model/effort options when updating this skill.

## Official evidence

- [GPT-6.2 release, November 17, 2026](https://learn.chatgpt.com/docs/whats-new/gpt-62-astra-sol): GPT-6.2 Sol and Astra launched in Codex, ChatGPT Work, and the API. Sol is intended for everyday coding, writing, and multistep work, including PR review. Astra is intended for harder reasoning and cross-service diagnosis.
- [OpenAI model catalog](https://developers.openai.com/api/docs/models): GPT-6.1 Terra emphasizes visual reasoning, computer use, and writing; GPT-6 Luna emphasizes efficient, focused workloads. These remain useful specialist routes alongside GPT-6.2.
- [Codex and Work pricing](https://learn.chatgpt.com/docs/pricing): GPT-6.2 Sol retains GPT-6.1 Sol's token rates, while GPT-6.2 Astra has lower rates than GPT-6 Astra. Actual usage also depends on reasoning effort, speed, context, and plan. API rates do not establish subscription task cost.

The routing table is this skill's operating policy, inferred from these capabilities and its existing efficiency goals; it is not a measured benchmark of the repository's tasks. Retain the existing focused Sol diagnosis and independent-review gate before Astra unless the user explicitly requests Astra. A cheaper new Astra does not by itself justify broader escalation.

## Runtime verification

At the check date, this workspace's Codex model cache listed `gpt-6.2-sol`, `gpt-6.2-astra`, `gpt-6.1-terra`, and `gpt-6-luna`, as well as the previous Sol and Astra generations. The child-agent tool exposed the two GPT-6.2 models, Terra, and the previous Sol and Astra, but did not expose Luna. Thus a Luna task needs the fallback specified in the skill in this runtime.

Treat tool options as authoritative for dispatch. A local Codex model cache, when present, is useful supporting evidence, not a portable availability guarantee. Use the exact supported model IDs and effort values, respect the tool's model-override constraints, and do not edit a cache to make a model appear available. The documented fallback routes also apply when the preferred model is missing in another runtime.
