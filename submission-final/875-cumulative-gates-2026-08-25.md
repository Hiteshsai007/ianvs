# Discussion #875 — cumulative gate record

**Date:** 2026-08-25

The cumulative rule is: after each restoration stage, rerun that stage's gate and every earlier gate. A later passing gate cannot mask a regression in an earlier device-contract or production-integration gate.

| Gate | Current result | Evidence |
|---|---|---|
| G0 — red device-contract test | **PASS as a gate** | `main@37a9c60` is intentionally RED for the selected ordering/default defect; PR #767 at `00ddb06` is GREEN for omitted/true/false. Rerun after the new production probes: same result. |
| G1 — Core ordering probe | **Executed** | Exact production constructor/control-flow probe on `main` and #767; output preserved in the existing Task 1 evidence. |
| G2 — PR #524 copied-helper suite | **Previously executed** | Existing pinned result: `41 passed in 1.23s`. The current sandbox lacks the PR's Torch/Pytest environment, so this was not falsely represented as a fresh local rerun. |
| G3 — PR #524 production `BaseModel` smoke test | **Executed** | Actual production class: baseline fails at `.cuda()`; #524 reaches `.to(cpu)`. See `production-basemodel-smoke-test.md`. |
| G4 — PR merge matrix | **Executed** | #767→#536→#524 clean; #536→#764 conflicts only at `gov_rag.py`; see `merge-matrix-767-536-524-764.md`. |
| G5 — PR #536 production constructor smoke test | **Executed** | Actual production `GovernmentRAG.__init__`: baseline resolves CPU; #536 resolves MPS under controlled CUDA-unavailable/MPS-available conditions. See `production-govrag-smoke-test.md`. |
| G6 — production Example integration / full test matrix | **Proposed** | Requires production dependencies, datasets, and/or model fixtures; not claimed as passed. |
| G7 — CI guard | **Proposed** | Unsafe/guarded fixtures still need to be implemented and run. |

## Verification boundary

The new G3 and G5 results are controlled production-constructor/load probes with import-boundary stubs, fake model/checkpoint or persisted-store fixtures, and no hardware claim. The existing G2 result remains a prior pinned execution record, not a newly rerun result in this dependency-free sandbox. Real CUDA/MPS behavior and full benchmarks remain outside the completed boundary.

## Copy-ready Task 4 update

> **Cumulative gate status.** G0/G1 continue to distinguish the intentionally failing `main` contract from the passing PR #767 contract. After adding the bounded production probes, the actual PR #524 `BaseModel` path reaches `.to(cpu)` while baseline fails at `.cuda()`, and the actual PR #536 `GovernmentRAG` constructor selects the documented fallback under a controlled MPS-available environment. The exact pinned merge matrix is clean for #767→#536→#524 and reproduces the expected #536/#764 `gov_rag.py` conflict. The existing #524 copied-helper result remains `41 passed in 1.23s`, but was not falsely called a fresh rerun in an environment without Torch/Pytest. G6/G7 and real CUDA/MPS/full benchmark execution remain proposed or outside the boundary.
