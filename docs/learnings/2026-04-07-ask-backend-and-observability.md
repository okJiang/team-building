# Lessons from Ask Backend and Observability in Phase 8

## Summary

- Task `#58` (Ask backend) and task `#52` (observability) exposed the same backend truth: product boundaries, privacy rules, and debug signals need to be designed together rather than added one by one.
- The most durable decisions were to reuse shared retrieval rules, make `out_of_scope` an explicit API state, isolate Ask configuration from transcription configuration, and treat logs as a privacy-sensitive product surface.
- The main lesson is that a user-facing AI feature becomes easier to ship and easier to debug when retrieval, status semantics, and observability all share the same contract.

## What Happened

- Task `#52` added structured observability across the existing callable functions, including Search, Settings, and transcription-related flows.
- Task `#58` added `askQuestion` for Ask V1 with candidate retrieval over the user's own entries, answer generation, evidence output, and three explicit result states: `answered`, `insufficient_evidence`, and `out_of_scope`.
- Both tasks touched shared backend surfaces such as callable registration, input summaries, and API-level product behavior.

## Key Lessons from Ask Backend

### 1. Reuse retrieval rules instead of recreating them

- Ask V1 needed the same understanding of searchable entry content that Search already had.
- Reusing the shared searchable-entry resolver avoided a silent drift where Search and Ask could have looked at different fields for the same entry.
- This reduced maintenance cost and made Ask evidence behavior easier to explain to iOS.

### 2. Product boundaries should be first-class API states

- `insufficient_evidence` and `out_of_scope` look similar from far away, but they mean very different things for product behavior.
- `insufficient_evidence` means the question is valid for Ask, but the user's entries do not support a grounded answer yet.
- `out_of_scope` means Ask should not answer that question at all, such as external knowledge, diagnosis, advice or decision-making, and ghostwriting-style requests.
- Making both states explicit in the contract kept iOS behavior simple and prevented the model from turning product policy into ambiguous prose.

### 3. Evidence is not optional in a grounding-first feature

- Ask V1 is useful only if the answer stays tied to the user's own entries.
- Returning an evidence list with `entryId` and snippet gave the frontend a concrete explanation surface and reduced the risk of “trust me” AI output.
- This also made testing easier because grounded answers had observable support rather than only free-form text.

### 4. Keep capability-specific model configuration isolated

- Ask and transcription both use LLM infrastructure, but they do not have the same rollout timing or provider constraints.
- Isolating Ask model and client configuration avoided accidental coupling with the still-blocked transcription line.
- The lesson is to separate configuration per backend capability when the release readiness of those capabilities can diverge.

## Key Lessons from Observability

### 1. One helper is better than many ad hoc logs

- A shared observability helper created a consistent start / finish / fail shape across functions.
- That consistency made it much easier to compare call volume, latency, and failure patterns across Search, Settings, Ask, and transcription-related flows.
- It also reduced the chance that one function would quietly skip useful fields such as duration or status.

### 2. Parameter summaries are safer than raw inputs

- The safest log is often not “more detail,” but “the smallest useful detail.”
- Logging `questionLength`, `queryLength`, confirmation-phrase length, or presence flags preserved debugging value without copying user content into logs.
- This turned privacy from an afterthought into a repeatable implementation pattern.

### 3. Error shape matters as much as success shape

- Standardized error serialization made backend failures easier to classify and faster to debug.
- Capturing structured error metadata together with `functionName`, `executionKind`, and duration made it possible to reason about failure rate instead of only reading raw stack traces.
- The practical lesson is that observability should be designed around the questions the team will need to answer later, not around what is easiest to print today.

## Problems We Hit and How We Solved Them

### Problem: Search and Ask could have drifted apart

- Risk: Ask might score or display entry content differently from Search, which would create hard-to-explain product inconsistencies.
- Solution: reuse the shared searchable-entry resolver so both paths share the same entry-text selection rules.

### Problem: the model could answer outside Ask V1 boundaries

- Risk: the system could return confident but policy-breaking answers for external knowledge or advice-like questions.
- Solution: encode the boundary in both the system prompt and the API response contract, then add focused tests for `out_of_scope`.

### Problem: observability could leak private user content

- Risk: raw search queries, Ask questions, deletion phrases, or transcripts could end up in logs.
- Solution: add dedicated input-summary helpers and test them, so only lengths and safe flags are recorded.

### Problem: logs across functions could become inconsistent over time

- Risk: each callable might emit slightly different metadata, making cross-function debugging slower and noisier.
- Solution: move the common logging pattern into a shared helper and keep per-function customization limited to safe summaries and a small amount of contextual metadata.

### Problem: rollout readiness differed across backend capabilities

- Risk: Ask and transcription could accidentally share the same configuration assumptions even though transcription remained owner-blocked.
- Solution: keep Ask configuration isolated so one capability can ship without being constrained by another capability's provider status.

## What We Would Repeat Next Time

- Decide feature boundaries and response states before writing the model prompt.
- Reuse existing retrieval or data-selection helpers when adding adjacent AI features.
- Treat logging changes as product and privacy changes, not as “just backend plumbing.”
- Add focused tests for policy states such as `out_of_scope`, not only happy-path answers.
- Isolate provider configuration per capability when rollout timing may differ.

## Workflow Improvements to Keep

- Add privacy review as a required checkpoint for any new logging or monitoring change.
- When a new feature depends on an older backend surface, explicitly decide whether it should reuse or replace that surface before implementation starts.
- Keep response states product-meaningful so iOS can map them to simple, deterministic UI behavior.
- Prefer shared instrumentation helpers over copy-pasted logs across callables.

## Related Tasks

- InnerTrace task `#52` — Backend observability (`okJiang/InnerTrace#39`)
- InnerTrace task `#58` — Ask backend (`okJiang/InnerTrace#47`)
- InnerTrace task `#43` — Search backend (`okJiang/InnerTrace#28`)
- Agent task `#64` — Phase 8 backend learning record
