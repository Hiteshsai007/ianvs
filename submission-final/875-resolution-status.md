# Discussion #875 — resolved / unresolved status table

This table is intended to make the current state explicit without overstating any unexecuted benchmark or hardware result.

| Area | Status | Evidence | Boundary / remaining limitation |
|---|---|---|---|
| Core constructor ordering on `main` vs #767 | **Executed and reverified** | Real `ParadigmBase` / `SingleTaskLearning` control-flow probe for omitted, `true`, and `false` cases on `main@37a9c60` and #767 at `00ddb06` | Establishes Ianvs-side environment-write ordering; does not prove real CUDA runtime enumeration on hardware |
| PR #524 copied-helper suite | **Executed** | `41 passed in 1.23s` at #524 head `2d834342` | Tests copied helpers, not production `BaseModel` integration |
| PR #524 production `BaseModel` constructor/load path | **Bounded CPU smoke test executed** | Actual production `BaseModel` loaded from #524; CPU path completed through `production_model.to(cpu)`; baseline failed at production `.cuda()` | Import-boundary stubs and fake checkpoint/model were used; no real checkpoint, inference, CUDA/MPS, or full MOT17 run |
| PR #802 reviewer-impact chain | **Executed and reverified** | Findings, author response, follow-up hardening, and final verification at `89c2a01` | Full federated benchmark was not run |
| #767 + #536 + #524 merge topology | **Executed** | Sequential merge from base was clean at the pinned heads | Git-level result only; no merged-tree full-suite claim |
| #536 + #764 reconciliation | **Conflict reproduced** | Conflict at `examples/government_rag/singletask_learning_bench/testalgorithms/gov_rag.py` | The province-scoped filter must be preserved during manual resolution |
| Government RAG production path | **Not independently executed** | #536 diff and source/config inspection | Requires a bounded production smoke test or remains static evidence; no full pipeline claim |
| Shared device resolver | **Proposed, not implemented** | Task 3 architecture and semantics table | Requires migration design, production tests, and staged adoption |
| Core-to-Example device contract | **Partially established / not complete** | #767 corrects Core ordering; #536/#524 still use independent Example-local resolvers | No uniform enforcement or shared API exists yet |
| CI guard against new unconditional CUDA usage | **Proposed, not executed** | Task 4 G7 design | Must be tested with unsafe and guarded fixtures |
| Real CUDA/MPS behavior | **Not executed** | Explicitly outside current environment | Must not be described as passed |
| Full Government RAG/MOT17 benchmarks | **Not executed** | External models, datasets, and runtime dependencies unavailable/out of scope | Must remain outside the completed verification boundary |

## Copy-ready status paragraph

> **Current resolution status.** The Core constructor-ordering behavior was executed on `main@37a9c60` and PR #767 at `00ddb06`; PR #524's 41 copied-helper tests passed on CPU; and a new bounded smoke test imported the actual PR #524 production `BaseModel` and reached `production_model.to(cpu)`, while the baseline production path failed at `.cuda()`. The exact pinned #767/#536/#524 heads merge cleanly in sequence, while #536/#764 reproduces one `gov_rag.py` conflict whose resolution must preserve province filtering. The shared device API, production Example integration, CI guard, real CUDA/MPS behavior, and full Government RAG/MOT17 benchmarks remain unexecuted or proposed and are not claimed as complete.
