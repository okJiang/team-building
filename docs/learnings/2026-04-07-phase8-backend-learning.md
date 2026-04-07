# Phase 8 Backend Learning: Ask Backend and Observability

## Summary

- Phase 8 showed that AI feature delivery and observability should be designed together, not as separate follow-up steps.
- The two strongest decisions were to make Ask boundaries explicit in the API contract and to make observability privacy-safe by default.
- The durable lesson is that a backend feature becomes easier to ship, debug, and explain when retrieval rules, response states, and logs all follow the same product boundary.

## What Happened

- Task `#52` added structured observability for the existing callable functions, including Search, Settings, and transcription-related flows.
- Task `#58` added `askQuestion` for Ask V1 with retrieval over the user's own entries, grounded answer generation, evidence output, and three result states: `answered`, `insufficient_evidence`, and `out_of_scope`.
- The Ask work reused Search-side retrieval logic and extended the observability helper with Ask-safe input summaries.
- Both tasks changed shared backend surfaces such as callable registration, logging shape, and frontend-visible response behavior.

## Why It Happened

- Ask V1 needed grounded retrieval, but the product also needed a clear line between supported questions and unsupported ones.
- Observability had to be added because backend behavior was expanding across multiple callables and debugging could no longer rely on ad hoc logs.
- Privacy risk increased as more user-generated content flowed through Search, Settings, Ask, and transcription paths.
- Search and Ask touched similar entry-selection logic, so duplication would have created drift and harder debugging.

## Impact

- Ask became safer to ship because unsupported questions and weak-evidence cases now have explicit backend states instead of vague model wording.
- Frontend integration became simpler because iOS can map `answered`, `insufficient_evidence`, and `out_of_scope` to deterministic UI states.
- Debugging improved because function start, finish, failure, duration, and safe parameter summaries now follow one shape across callables.
- Privacy risk was reduced because logs capture lengths and safe flags rather than raw user content.
- Future maintenance cost was reduced because Search and Ask now share retrieval-related behavior instead of maintaining parallel logic.

## Problems and Solutions

### Problem 1: Search and Ask could have drifted apart

- Risk: Search and Ask might have looked at different entry fields or built snippets differently, creating inconsistent product behavior.
- Solution: reuse the shared searchable-entry resolver so both paths use the same entry-text selection rules.

### Problem 2: The model could answer outside Ask V1 boundaries

- Risk: Ask might respond to external-knowledge, diagnosis, advice, or ghostwriting-style questions even though the product should refuse them.
- Solution: encode the boundary in both the system prompt and the API contract, then add focused tests for `out_of_scope`.

### Problem 3: “Not enough evidence” and “should not answer” could be mixed together

- Risk: a single fallback state would make frontend behavior confusing and would blur product policy with missing user data.
- Solution: separate `insufficient_evidence` from `out_of_scope` and make both first-class response states.

### Problem 4: Observability could leak private content

- Risk: raw search queries, Ask questions, deletion phrases, or transcript-like content could end up in logs.
- Solution: add dedicated summary helpers that log only safe values such as `questionLength`, `queryLength`, and confirmation length.

### Problem 5: Observability across functions could become inconsistent

- Risk: each callable could emit different log fields, making cross-function debugging slower and metrics less trustworthy.
- Solution: use one shared observability helper with a standard start / finish / fail pattern and narrow per-function summaries.

### Problem 6: Ask and transcription did not share the same rollout readiness

- Risk: one LLM-backed feature could become blocked by another feature's provider or configuration state.
- Solution: isolate Ask configuration from transcription configuration so each capability can move at its own release pace.

## Prevention

- Decide feature boundaries and response states before writing the model prompt.
- Reuse existing retrieval or data-selection helpers when a new feature is adjacent to an older one.
- Treat every logging change as both a debugging change and a privacy change.
- Add focused tests for policy states such as `out_of_scope`, not only happy-path answers.
- Isolate provider configuration per backend capability when readiness can differ.
- Prefer shared instrumentation helpers over copy-pasted logs across callables.

## Rule or Workflow Changes

- Add a backend checklist item: decide whether a new AI feature should return `out_of_scope`, `insufficient_evidence`, or both before implementation begins.
- Add a privacy checklist item: no new backend log should include raw user content unless there is explicit approval and a strong reason.
- Add a reuse checklist item: when a new capability depends on existing retrieval behavior, explicitly choose reuse or separation before coding.
- Add a monitoring checklist item: if a feature adds new user-visible states, observability should record enough structured metadata to debug those states later.

## Related Tasks

- InnerTrace task `#52` — Backend observability (`okJiang/InnerTrace#39`)
- InnerTrace task `#58` — Ask backend (`okJiang/InnerTrace#47`)
- InnerTrace task `#43` — Search backend (`okJiang/InnerTrace#28`)
- Agent task `#64` — Phase 8 backend learning record
