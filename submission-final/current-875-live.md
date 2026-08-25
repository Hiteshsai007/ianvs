# Current live source — Discussion #875

Fetched from GitHub on **2026-08-25**. This file is a local snapshot of the live Discussion used for the final package.

- URL: https://github.com/kubeedge/ianvs/discussions/875
- Updated at: 2026-08-25T11:51:24Z (Bonus comment edit)
- Author: Hiteshsai007
- Comment count: 5
- Execution video: https://youtu.be/VJqXbzr75cs
- Target-specific URLs matched: 18

---

## Pre-test Submission — LFX Mentorship 2026 Term 3

**Candidate:** [Hiteshsai007](https://github.com/Hiteshsai007)

**Root Problem:** Ianvs expresses device policy through implicit process-level CUDA visibility while Examples independently hardcode or resolve runtime devices.

**Target Issues:** #765, #535, #523

**Mandatory PRs:** #767 (critical Core PR), #536, #524

**Supplementary conflict evidence:** #764

**Demonstrated reviewer impact:** My [review of PR #802](https://github.com/kubeedge/ianvs/pull/802#issuecomment-5386647117) identified silent partial-round continuation after federated-client failure. The [author implemented the requested fail-fast handling](https://github.com/kubeedge/ianvs/pull/802#issuecomment-5387002670), then [adopted my follow-up hardening suggestion](https://github.com/kubeedge/ianvs/pull/802#issuecomment-5387170752) to reject unexpected client IDs. My [final verification](https://github.com/kubeedge/ianvs/pull/802#issuecomment-5387198216) confirmed that all findings and follow-up suggestions are addressed at [`89c2a01`](https://github.com/kubeedge/ianvs/commit/89c2a013de1185d9ec137e1045c8337334aed861).

### Submission index

| Section | Discussion comment | Status |
|---|---|---|
| Task 1 — Root Problem Analysis | [open](https://github.com/kubeedge/ianvs/discussions/875#discussioncomment-18121621) | Complete; executed figures attached |
| Task 2 — Multi-PR Code Review | [open](https://github.com/kubeedge/ianvs/discussions/875#discussioncomment-18121643) | Written analysis and execution video attached |
| Task 3 — Repair Boundary Analysis | [open](https://github.com/kubeedge/ianvs/discussions/875#discussioncomment-18121677) | Complete |
| Task 4 — Restoration Path Design | [open](https://github.com/kubeedge/ianvs/discussions/875#discussioncomment-18121695) | Complete |
| Bonus — four supplementary reviews | [open](https://github.com/kubeedge/ianvs/discussions/875#discussioncomment-18121701) | Complete; all four cross-posted |

### Permanent target comments

| Task | Target | Permanent comment |
|---|---|---|
| Task 1 | Issue #765 | https://github.com/kubeedge/ianvs/issues/765#issuecomment-5384535569 |
| Task 1 | Issue #535 | https://github.com/kubeedge/ianvs/issues/535#issuecomment-5384537091 |
| Task 1 | Issue #523 | https://github.com/kubeedge/ianvs/issues/523#issuecomment-5386498840 |
| Task 2 | PR #767 | https://github.com/kubeedge/ianvs/pull/767#issuecomment-5384561099 |
| Task 2 | PR #536 | https://github.com/kubeedge/ianvs/pull/536#issuecomment-5386505503 |
| Task 2 | PR #524 | https://github.com/kubeedge/ianvs/pull/524#issuecomment-5386501544 |
| Supplementary | PR #764 | https://github.com/kubeedge/ianvs/pull/764#issuecomment-5384563777 |
| Bonus | PR #808 | https://github.com/kubeedge/ianvs/pull/808#issuecomment-5386552612 |
| Bonus | PR #827 | https://github.com/kubeedge/ianvs/pull/827#issuecomment-5386637841 |
| Bonus | PR #775 | https://github.com/kubeedge/ianvs/pull/775#issuecomment-5386643381 |
| Bonus | PR #802 | https://github.com/kubeedge/ianvs/pull/802#issuecomment-5386647117 |

### Verification boundary

The real Ianvs Core constructor order was executed on `main@37a9c60a9747af0cfe3170f84249bc349c56e8d5` and `pull/767/head@00ddb06dbfb679dc3800059dd7730720c5a50e77`, with unavailable Sedna/ONNX dependencies replaced only at import boundaries.

PR #524’s added tests were executed on CPU, but they test copied helpers rather than production `BaseModel`.

Full Government RAG/MOT17 benchmarks and real CUDA/MPS verification were not performed and are not claimed as passed.

### **Task 1 — Root Problem Analysis**
 
---
 
### 1. Problem Definition
 
**Selected Existing Issues** (all open, Target Rule compliant: #348–#846)
 
| Issue | Example | Surface symptom |
|---|---|---|
| #765 | `llm-edge-benchmark-suite/single_task_bench_with_compression` | `use_gpu: false` is a no-op; `use_gpu: true` is applied after the base model is already constructed |
| #535 | `government_rag` | Hardcoded `device = "cuda"` → fails on CPU-only and Apple Silicon machines |
| #523 | `MOT17/multiedge_inference_bench` (M3L ReID) | Unconditional `.cuda()` transfers with no CPU/MPS fallback, plus related compatibility/reliability defects in `basemodel.py` |
 
**Root Problem**
 
> Ianvs expresses device policy primarily through process-level CUDA visibility, while Examples independently hardcode or resolve runtime devices. The contract is implicit, timing-sensitive, CUDA-specific, and inconsistent across Examples and runtimes.
 
#765, #535, and #523 are related manifestations of this gap, not identical problems:
 
- **#765 – Core ordering/default defect.** `use_gpu` is declared on `TestEnv` and parsed from `testenv.yaml`, but is consumed in only one paradigm (`singletask_learning`), has no `else` branch for `false`, and the environment write happens *after* the example's `basemodel.py` has already been imported and instantiated.
- **#535 – Baseline Example hardcode.** `government_rag` hardcodes `device = "cuda"` at module level and in every constructor call. It never reads `use_gpu` or any Core mechanism.
- **#523 – Second Example's unconditional CUDA transfers.** `MOT17`'s M3L ReID model calls `.cuda()` unconditionally in two places with no fallback, independent of `government_rag` and independent of the Core `use_gpu` mechanism.
**Why this matters**
Open PR #767 (credited to `vjuhhii`) implements the fix for the Core-side ordering/default defect described in #765 — it moves the logic into `ParadigmBase`, adds three-state handling, and uses `CUDA_VISIBLE_DEVICES="-1"` for the `false` case. That Problem and that Solution are not new findings of this analysis; Discussion #856 (`nirdesho6o`, posted Aug 22) independently identified the same Core defect first, and this analysis credits that priority explicitly (see Uniqueness).
 
Open PR #536 removes the `government_rag` hardcodes and adds a local `default_device()` (cuda → mps → cpu). It does not directly consume `use_gpu`. However, `default_device()`'s own `torch.cuda.is_available()` call may indirectly observe the CUDA visibility that PR #767 establishes at the process level — meaning the two fixes are not necessarily incompatible. The remaining architectural weakness is that this relationship is implicit, undocumented, CUDA-specific, and not uniformly applicable to MPS, import-time initialization, explicit device strings, unconditional `.cuda()` calls, ONNX providers, indexed/multi-GPU behavior, or non-PyTorch runtimes. Real CUDA hardware interaction between #767 and #536 was not independently verified.
 
Open PR #524 addresses #523's unconditional `.cuda()` calls in MOT17/ReID the same way #536 addresses `government_rag`: with a local, Example-scoped `_get_device()` resolver, unconnected to any Core mechanism.
 
**Examples involved**
Primary: `llm-edge-benchmark-suite/...` (#765), `government_rag` (#535), `MOT17/multiedge_inference_bench` (#523).
Systemic: additional examples also contain unconditional `.cuda()` calls with no fallback (see E4); auto-detection resolvers in Examples may indirectly observe Core's CUDA masking, but unconditional CUDA calls and import-time decisions remain problematic regardless.
 
---
 
### 2. Evidence
 
**E1 – Core ordering/default defect (main @ 37a9c60)**
 
- Declaration/parsing: `core/testenvmanager/testenv/testenv.py:49, 68-69` (default `False`; absent and `false` are indistinguishable at declaration).
- Sole consumer: `core/testcasecontroller/algorithm/paradigm/singletask_learning/singletask_learning.py:55-56`
```python
  if kwargs.get("use_gpu", True):
      os.environ["CUDA_VISIBLE_DEVICES"] = "0"
```
  No `else` branch → `use_gpu: false` is a silent no-op at this call site.
- Ordering: `singletask_learning.py:50` calls `ParadigmBase.__init__` → `base.py:56` `_get_module_instances()` imports and instantiates the example basemodel **before** the environment variable is set.
- Prevalence: only **2 of 48** `examples/**/testenv*.yaml` files declare `use_gpu`.
**E1 addendum — executed, not just read.**
Ran the real `ParadigmBase.__init__` / `SingleTaskLearning.__init__` from `main@37a9c60a9747af0cfe3170f84249bc349c56e8d5` and `pull/767/head@00ddb06dbfb679dc3800059dd7730720c5a50e77`, with `os.environ` instrumented to record `CUDA_VISIBLE_DEVICES` writes and a stand-in algorithm-module loader recording module construction. Sedna and ONNX were replaced with import-time no-op stand-ins because they were unavailable and were not exercised by this code path; no line under `core/` was modified; PyTorch was not mocked. Verbatim result:
 
```text
main:
 
use_gpu omitted:
LOAD
SET CUDA_VISIBLE_DEVICES='0'
 
use_gpu=True:
LOAD
SET CUDA_VISIBLE_DEVICES='0'
 
use_gpu=False:
LOAD
CUDA_VISIBLE_DEVICES remains unset
 
 
PR #767:
 
use_gpu omitted:
LOAD
CUDA_VISIBLE_DEVICES remains unchanged
 
use_gpu=True:
SET CUDA_VISIBLE_DEVICES='0'
LOAD
 
use_gpu=False:
SET CUDA_VISIBLE_DEVICES='-1'
LOAD
```

**Figure 1 — Executed baseline constructor ordering (`main@37a9c60`).**


<img width="1458" height="752" alt="figure 1" src="https://github.com/user-attachments/assets/3edbe53f-3a60-4b96-83bc-7cc7708fe821" />


**Figure 2 — Executed corrected constructor ordering (PR #767 at `00ddb06`).**

<img width="1458" height="854" alt="figure2" src="https://github.com/user-attachments/assets/5d124796-8cc2-4627-8504-56986334d71f" />


This confirms the ordering and no-op defects on `main`, and confirms PR #767's fix reorders the write before the load in all three states on the PR branch. It also surfaces a finding not previously stated: on `main`, `use_gpu` **absent** behaves identically to `True` (env set to `'0'`), which contradicts `TestEnv`'s own default of `self.use_gpu = False` (`testenv.py:49`).
 
**Verification boundary:** the CPU-only environment used for this probe could not independently verify how real CUDA device enumeration responds to pre-import versus post-import environment changes on actual GPU hardware; the result establishes the *ordering and write behaviour* of the Ianvs code, not the downstream CUDA runtime effect.
 
**E2 – Baseline Example hardcode: `government_rag` (main @ 37a9c60)**
 
- Hardcoded assignment: `examples/government_rag/.../basemodel.py:38` (`device = "cuda"`).
- Four additional constructor calls also force `device="cuda"` (lines 160, 179, 181, 184).
- Zero references to `use_gpu` or any Core device API.
**E3 – Second Example's unconditional CUDA transfers: MOT17/ReID (main @ 37a9c60)**
 
On baseline `main@37a9c60`, the M3L production model uses two unconditional CUDA transfers:
- `self.model.cuda()` during model loading (`basemodel.py:62`);
- `to_torch(inputs).cuda()` during feature extraction (`basemodel.py:112`).
These paths have no CPU/MPS fallback and match Issue #523's portability report. (Note: this Task does not claim MOT17/ReID calls `torch.cuda.device_count()` or initializes distributed topology — that claim is unsupported for this baseline path and is not made here.)
 
**E4 – Open PR behaviour (branches fetched and inspected)**
 
| PR | What it changes | Still missing relative to Root Problem |
|---|---|---|
| #767 | Moves logic to `ParadigmBase.__init__` before module load; three-state (`None`/`true`/`false`); uses `"-1"` | No enforcement that Examples must honour the mechanism; no permanent unit test for the ordering behaviour |
| #536 | Removes hardcodes; adds `default_device()` (cuda→mps→cpu), whose `torch.cuda.is_available()` call may indirectly observe #767's CUDA masking | Does not directly read `use_gpu`; implicit, CUDA-specific relationship to Core only |
| #524 | Removes hardcodes in M3L ReID; adds equivalent local `_get_device()` resolver; ships 41 CPU-executable tests | Tests exercise copied logic, not production `BaseModel` (see Task 2); does not read `use_gpu` |
| #764 (supplementary) | LangChain modernisation of `government_rag` | Leaves every `device="cuda"` hardcode untouched; conflicts with #536 in `gov_rag.py` |
 
**E5 – Systemic scope**
 
Cross-repo scan found unconditional `"cuda"` hardcodes in multiple additional example families beyond the three primary targets. Only `llm_simple_qa/basemodel.py:30` currently implements a proper multi-backend path. Where an Example does add local auto-detection (as #536 and #524 both now do), that detection may indirectly observe Core's CUDA masking via `torch.cuda.is_available()` — so these Examples are not best described as a complete "bypass" of Core; the more accurate framing is that unconditional CUDA calls and import-time device decisions in the *remaining* unfixed examples are what stay problematic, and even the fixed examples' relationship to Core is implicit rather than contractual.
 
Independently re-verified by grepping every `core/testcasecontroller/algorithm/paradigm/*/*.py` file for `use_gpu`/`CUDA_VISIBLE_DEVICES`: only `singletask_learning.py` contains either term. `federated_learning.py`, `federated_class_incremental_learning.py`, `incremental_learning.py`, `joint_inference.py`, `lifelong_learning.py`, and `multiedge_inference.py` contain zero references — 6 of 7 paradigm modules never consult the setting at all. This corroborates the count independently identified in Discussion #856 (credited to `nirdesho6o`) via independent re-verification rather than by citation alone.
 
**Verification boundary (overall)**
All file:line claims and branch diffs were verified on a fresh clone at commit `37a9c60`. The E1 ordering/no-op claims were additionally verified by executing the real `ParadigmBase`/`SingleTaskLearning` control flow on both `main` and `pull/767/head` (Sedna/ONNX stubbed at import time only). PR #524's added tests were executed on CPU (see Task 2). Full end-to-end GPU/MPS benchmark runs of `government_rag` and MOT17/ReID were not executed; those claims remain limited to observable code paths, static configuration audit, and the CPU-executable test evidence described above. Figures 1–2 preserve the executed constructor-ordering evidence for `main@37a9c60` and PR #767 at `00ddb06`. The probe executes the exact relevant production constructor methods while excluding unavailable external-import paths; it verifies Ianvs ordering and environment writes, not real CUDA/MPS hardware behavior.
 
---
 
### 3. Analysis
 
**(a) Scope**
Hybrid. The Core mechanism exists and is partially correct once #767 lands, but nothing requires an Example to use it. #535 and #523 show two independent Examples that currently do not use it at all; #536 and #524 show two independent Examples moving toward local resolvers that may *indirectly* interact with Core's CUDA masking, without a documented contract.
 
**(b) Relationship between the issues**
Independent code locations, same missing contract. #765 = Core mechanism attempted and (with #767) partially completed. #535 and #523 = two separate Examples that historically ignored the mechanism outright, now each growing their own local resolver (#536, #524) rather than converging on a shared one.
 
**(c) Risk of fixing separately**
Already visible: PR #767 makes Core correct and paradigm-wide for the one paradigm that reads `use_gpu`. PR #536 and PR #524 each independently implement a *local* cuda→mps→cpu resolver for their own Example, rather than calling into a shared mechanism — meaning Ianvs is on track to end up with at least three parallel device-selection implementations (Core's `use_gpu`, `government_rag`'s `default_device()`, MOT17/ReID's `_get_device()`) that happen to look similar but are not connected. PR #536 also conflicts with supplementary PR #764 in `gov_rag.py`.
 
---
 
### 4. Uniqueness
 
Checked against existing Show-and-tell Discussions (dataset-schema, unguarded config lookup, Sedna installation, incremental-learning engine, dependency manifests), Discussion **#856** (`nirdesho6o`, posted Aug 22 — one day before this submission), and the full threads on #765 / #535 / #523 / #767 / #536 / #524 / #764 as of the submission date:
 
| Finding | Existing discussion | This analysis |
|---|---|---|
| Core ordering/default defect in #765 | Independently identified first in Discussion #856; also the subject of #767 (`vjuhhii`) | Not claimed as new; independently re-verified with exact lines and by execution |
| `use_gpu` absent silently behaves as `True`, contradicting `TestEnv`'s own `False` default | Not stated in #765, #767, or Discussion #856 | New — confirmed by execution, not inference |
| Only 1 of 7 paradigm modules consult `use_gpu` at all | Independently identified in Discussion #856 | Independently re-verified by direct grep of every paradigm file; credited to #856 for priority |
| Hardcoded device in `government_rag` (#535) | Stated in the issue | Independently verified with all five sites |
| Hardcoded device in MOT17/ReID (#523) | Stated in the issue | Independently verified with both sites (`basemodel.py:62, 112`) |
| #536's `default_device()` may indirectly observe #767's CUDA masking via `torch.cuda.is_available()` | Not stated in either PR thread | New — and a correction of an earlier draft of this analysis, which had overstated the two PRs as incompatible |
| #524's added tests exercise copied logic rather than production `BaseModel` | Not raised | New (see Task 2 for full evidence) |
| Three independent Examples (`government_rag`, MOT17/ReID, and any future Example) are converging on similar-but-disconnected local resolvers instead of one shared mechanism | Not raised in any current Discussion, including #856 | **Primary contribution** |
| #536 vs #764 conflict in `gov_rag.py` | Not clearly contrasted | New |
 
Discussion #856 has priority on the Core-side defect in #765 and on the "most paradigms don't read `use_gpu`" count; both are credited here rather than re-claimed. This analysis' distinct contribution is treating #765, #535, and #523 as three related-but-independent instances of the same missing repository-wide device contract, showing that the Example-side response to that gap is now trending toward multiple disconnected local resolvers rather than one shared one, and backing the Core-side claims with an executed reproduction rather than a code read alone.
 

### **Task 2 — Multi-PR Code Review**
 
---
 
### 1. Selection
 
| PR | Layer | Connection to Task 1 Root Problem | Critical? |
|---|---|---|---|
| **#767** | Core (`ParadigmBase`) | Fixes the Core-side ordering/default defect from #765 | **Yes** – touches shared Core used by every paradigm |
| **#536** | Example (`government_rag`) | Removes hardcoded `device="cuda"`; adds local `default_device()` | No (Example-local) |
| **#524** | Example (`MOT17/ReID`) | Removes hardcoded `.cuda()` calls from #523; adds local `_get_device()` resolver and a CPU-executable test suite | No (Example-local) |
 
**Supplementary only:** PR #764 (`government_rag` LangChain modernisation; overlapping/conflict evidence with #536 — see note at the end of this comment). PR #764 is not one of the three mandatory PRs.
 
**Critical PR:** #767. It moves device-visibility logic into `ParadigmBase.__init__`, so the change affects every paradigm, not a single Example directory.
 
---
 
### 2. Motivation
 
These three PRs test the Root Problem from three angles: #767 attempts to fix the **Core** side of the missing contract; #536 and #524 each independently attempt to fix the **Example** side, for two unrelated Examples (`government_rag`, MOT17/ReID). Selecting both #536 and #524 — rather than #536 and its overlapping #764 — lets us test whether the *pattern* of Example-side fixes converges on anything shared, or whether each Example is independently reinventing the same cuda→mps→cpu resolver.
 
---
 
### 3. Recommendation
 
| PR | Recommendation | Reason |
|---|---|---|
| **#767** | `minor revision` | Correct Core fix; add permanent omitted/true/false ordering tests |
| **#536** | `minor revision` | Valuable Example repair; document the implicit semantics of its relationship to #767, and preserve province filtering while reconciling with #764 |
| **#524** | `major revision` | New tests execute copied helpers instead of production code; integration remains untested |
 
**Alternative explanation considered**
 
> `CUDA_VISIBLE_DEVICES` may already be a sufficient contract because compliant runtimes observe it automatically; an explicit Core-to-Example API may be unnecessary.
 
This explanation is partially supported by #536, whose later `torch.cuda.is_available()` call may observe #767's CUDA masking. It is insufficient as a universal cross-runtime contract because it does not define MPS behaviour, initialization timing, explicit device strings, unconditional `.cuda()` calls, ONNX providers, indexed/multi-GPU behaviour, or non-PyTorch runtimes — all of which are already present across the Example set (E5 in Task 1).
 
---
 
### 4. Review
 
**4.1 Does the PR resolve the Root Problem?**
#767 resolves the Core-side ordering/default defect cleanly, and is the correct direction, but does not by itself resolve the Root Problem: nothing requires an Example to read `use_gpu`. #536 and #524 each resolve their own Example's hardcode, but each does so with an independent local resolver rather than a shared one.
 
**4.2 Symptom vs root**
#536 and #524 are both, in isolation, symptom fixes at the Example layer — correct and valuable, but not connected to a shared mechanism. #767 is a real root-layer improvement for Core, but still leaves the contract unenforced and undocumented.
 
**4.3 Duplication / conflict / inconsistency**
#536 and #764 both modify `gov_rag.py` → a real merge conflict requiring reconciliation. #536 and #524 do not conflict at the file level (different Examples) but independently duplicate the same cuda→mps→cpu resolution logic. No file overlap between #767 and either Example PR.
 
**4.4 Edge cases, regression risk, cross-example impact**
 
| PR | Regression risk | Notes |
|---|---|---|
| #767 | Low–medium | Three-state design preserves "key absent = leave environment untouched." No permanent unit test for the ordering behaviour yet exists in the repo. |
| #536 | Low | Additive local change; affects `government_rag` only. Must be reconciled with #764 without losing province filtering. |
| #524 | Low for the Example itself; higher for confidence in the fix | Additive local change; but the new tests do not exercise the production class the fix is supposed to protect (see 4.7 and the copied-test finding below). |
 
**4.5 Should the change stay Example-local or move to Core?**
Device-visibility *policy* belongs in Core (#767 is the right direction). The specific device-resolution logic that #536 and #524 have each written independently (`cuda` → `mps` → `cpu`) is a strong candidate to move into a shared utility, since both Examples arrived at nearly the same logic separately (see Task 3).
 
**4.6 Merging multiple PRs simultaneously**
Merging #767 + #536 + #524 together is safe at the file level (no overlapping files across the three). Merging #536 together with #764 requires conflict resolution in `gov_rag.py` and must preserve the province-scoped retrieval filter that #536 added in `d2d062d` after an earlier review found cross-province leakage.
 
**4.7 Evidence Standard**
Claims above are based on direct inspection of the PR diffs, the main-branch code paths verified in Task 1, an executed reproduction of #767's control-flow fix, and an executed run of #524's added test suite (below).
 
---
 
### 5. Reproduce
 
**#767 — executed.**
 
Traced `os.environ` writes against the real `ParadigmBase.__init__` / `SingleTaskLearning.__init__` on both `main@37a9c60a9747af0cfe3170f84249bc349c56e8d5` and `pull/767/head@00ddb06dbfb679dc3800059dd7730720c5a50e77`. Sedna and ONNX were replaced with import-time no-op stand-ins because they were unavailable and were not exercised by this code path. No line under `core/` was modified. PyTorch was not mocked — this reproduction does not depend on torch at all, since it only traces environment-variable writes and object construction order.
 
```python
import sys, os
sys.path.insert(0, "./fake_sedna")
sys.path.insert(0, "./ianvs")          # or "./ianvs-pr767"
 
events = []
_real_setitem = os.environ.__class__.__setitem__
def traced_setitem(self, key, value):
    if key == "CUDA_VISIBLE_DEVICES":
        events.append(f"SET  CUDA_VISIBLE_DEVICES={value!r}")
    return _real_setitem(self, key, value)
os.environ.__class__.__setitem__ = traced_setitem
 
from core.testcasecontroller.algorithm.paradigm.base import ParadigmBase
from core.testcasecontroller.algorithm.paradigm.singletask_learning.singletask_learning import SingleTaskLearning
 
class DummyAlgorithmModule:
    def get_module_instance(self, module_type):
        events.append(f"LOAD module_type={module_type!r}")
        return object()
 
def run_case(use_gpu_kwarg):
    events.clear()
    os.environ.pop("CUDA_VISIBLE_DEVICES", None)
    kwargs = {"modules": {"basemodel": DummyAlgorithmModule()}, "dataset": None}
    if use_gpu_kwarg is not None:
        kwargs["use_gpu"] = use_gpu_kwarg
    SingleTaskLearning(workspace="/tmp/ws", **kwargs)
    return list(events), os.environ.get("CUDA_VISIBLE_DEVICES", "<unset>")
 
for label, val in [("use_gpu absent", None), ("use_gpu=True", True), ("use_gpu=False", False)]:
    ev, final = run_case(val)
    print(f"-- {label} --")
    for e in ev: print("   ", e)
    print(f"   CUDA_VISIBLE_DEVICES after __init__: {final}\n")
```
 
Real output — `main`:
```text
use_gpu omitted:  LOAD → SET CUDA_VISIBLE_DEVICES='0'
use_gpu=True:     LOAD → SET CUDA_VISIBLE_DEVICES='0'
use_gpu=False:    LOAD → CUDA_VISIBLE_DEVICES remains unset
```
 
Real output — `pull/767/head`:
```text
use_gpu omitted:  LOAD → CUDA_VISIBLE_DEVICES remains unchanged
use_gpu=True:     SET CUDA_VISIBLE_DEVICES='0' → LOAD
use_gpu=False:    SET CUDA_VISIBLE_DEVICES='-1' → LOAD
```
 
The CPU-only environment used here could not independently verify how real CUDA device enumeration responds to pre-import versus post-import environment changes; this establishes the Ianvs-side ordering and write behaviour, not the downstream CUDA runtime effect. This zero-hardware result is not positive evidence of a GPU-visible effect — it is scoped to what it actually shows.
 
**#524 — executed.**
 
Tested head: `2d8343424dca334b88621ddffc7e811b818fd757`
 
```text
Command:
python -m pytest test_basemodel.py -q

PR #524: 41 passed in 1.23s, exit code 0

```
 **Figure 1 — Fresh execution of PR #524’s added CPU-compatible test suite at reviewed commit `2d83434`.**

<img width="1182" height="557" alt="figure3" src="https://github.com/user-attachments/assets/915f6ab0-9565-407f-8ff6-c8c957150ba4" />


This figure records execution of the PR-added test file. As established below, the suite tests copied helper implementations rather than production `BaseModel`; it is not evidence that production MOT17 inference passed.
This is execution of a PR-added test file, not a pre/post production-behaviour comparison.
 
**Primary #524 finding: the test file copies logic instead of importing production `BaseModel`.**
 
- copied [`_get_device()` (test lines 35–41)](https://github.com/kubeedge/ianvs/blob/2d8343424dca334b88621ddffc7e811b818fd757/examples/MOT17/multiedge_inference_bench/pedestrian_tracking/testalgorithms/reid/m3l/test_basemodel.py#L35-L41) vs. [production `_get_device()` (lines 43–49)](https://github.com/kubeedge/ianvs/blob/2d8343424dca334b88621ddffc7e811b818fd757/examples/MOT17/multiedge_inference_bench/pedestrian_tracking/testalgorithms/reid/m3l/basemodel.py#L43-L49);
- copied [regex helpers (test lines 44–57)](https://github.com/kubeedge/ianvs/blob/2d8343424dca334b88621ddffc7e811b818fd757/examples/MOT17/multiedge_inference_bench/pedestrian_tracking/testalgorithms/reid/m3l/test_basemodel.py#L44-L57) vs. [production regex parsing (lines 67–75)](https://github.com/kubeedge/ianvs/blob/2d8343424dca334b88621ddffc7e811b818fd757/examples/MOT17/multiedge_inference_bench/pedestrian_tracking/testalgorithms/reid/m3l/basemodel.py#L67-L75);
- copied [pairwise-distance functions (test lines 60–83)](https://github.com/kubeedge/ianvs/blob/2d8343424dca334b88621ddffc7e811b818fd757/examples/MOT17/multiedge_inference_bench/pedestrian_tracking/testalgorithms/reid/m3l/test_basemodel.py#L60-L83) vs. production [`_pairwise_distance()`](https://github.com/kubeedge/ianvs/blob/2d8343424dca334b88621ddffc7e811b818fd757/examples/MOT17/multiedge_inference_bench/pedestrian_tracking/testalgorithms/reid/m3l/basemodel.py#L189).
No test imports or constructs production `BaseModel`, and none exercises production `load()`, `predict()`, checkpoint loading, `DataParallel`, or model/tensor device integration.
 
**Impact:** because the tests duplicate rather than import production logic, all 41 tests can remain green if production code drifts from the copied helpers or an integration path breaks.
 
**Conditional `train_dataset` finding:**
 
> The copied Core stub demonstrates that the normal path supplies `train_dataset` and the model-parallel path does not. A separate copied M3L fragment demonstrates that dereferencing an absent `train_dataset` would raise `AttributeError`. Neither production method was executed, and the model-parallel stub call itself did not fail.
>
> The checked-in M3L configuration does not enable `model_parallel`, so this is a conditional interface risk rather than a reproduced default-path failure. The gap appears pre-existing and unaddressed by #524, not introduced by #524.
 
**Proposed repair:** move the pure logic into production helpers and import those helpers from the test suite; add a production-module CPU smoke test with external dependencies stubbed only at import boundaries, plus a Core-to-`predict()` contract test.
 
**Verification boundary:** PR-added copied-helper tests passed on CPU (41/41). Production M3L inference, CUDA, and real MPS were not executed. Environment: Python 3.11.2, PyTorch 2.13.0+cu130, pytest 9.1.1.

**#536 (Example-local, static)**
Diff removes every `device="cuda"` hardcode and adds `default_device()` (cuda → mps → cpu). Does not reference `use_gpu` or `ParadigmBase` directly, but its `torch.cuda.is_available()` call may indirectly observe #767's CUDA masking (see Task 1). (Static diff inspection only — not executed; `government_rag`'s full pipeline needs torch/transformers/chromadb, out of scope here.)
 
**#764 (supplementary, static)**
Diff leaves all `device="cuda"` sites unchanged; overlaps with #536 in `gov_rag.py`. Not one of the three mandatory PRs — analysed further as supplementary conflict evidence. (Static diff inspection only.)
 
### Execution Video

[Fresh PR #524 test execution](https://youtu.be/VJqXbzr75cs)

The recording shows a fresh execution of PR #524's test file at reviewed commit `2d834342`, with all 41 PR-added tests passing. It does not claim production MOT17 inference, full benchmark execution, or real CUDA/MPS hardware verification.
 

---
 
### 6. Uniqueness
 
Checked against existing reviews on #767, #536, #524, and #764:
 
| Finding | Previously raised? | This review |
|---|---|---|
| #767 correctly implements three-state + ordering | Yes (PR body; also independently identified in Discussion #856) | Confirmed by execution on both branches; not claimed as new |
| #536's `default_device()` may indirectly observe #767's CUDA masking rather than being fully incompatible | Not raised | New — and a correction of an earlier overstatement |
| #524's tests copy production logic instead of importing it | Not raised | **New**, with pinned line-level evidence |
| Conditional `train_dataset` gap in M3L's model-parallel path | Not raised as a reproduced or copied-evidence finding | New, clearly scoped as a conditional interface risk, not a reproduced failure |
| Two independently-written local resolvers (#536, #524) for two unrelated Examples, converging on similar but disconnected logic | Not raised | **Primary new finding** |
| #536 and #764 conflict in `gov_rag.py`; province filtering must be preserved | Not clearly stated in either PR thread | New |
 
The technically meaningful finding not already explicit in the PR discussions: two unrelated Examples are independently reinventing the same device-resolution logic, and the one PR-added test suite that claims to protect this behaviour (#524) does not actually test the production code path — demonstrated by execution, not by reading the diff.

### **Task 3 — Repair Boundary Analysis**
 
---
 
### 1. Problem Definition
 
Tasks 1–2 show the same missing device-policy contract manifesting in Ianvs Core (#765/#767) and in at least two independent Examples (`government_rag` #535/#536, MOT17/ReID #523/#524). Both Examples are now independently writing near-identical cuda→mps→cpu resolvers rather than sharing one.
 
Choosing the wrong repair boundary here has a concrete cost already visible in this repository: fixing purely at Example-local layer (as #536 and #524 both currently do) produces duplicated, drifting logic; fixing purely by injecting a new mechanism into every constructor immediately risks breaking every existing Example that has not opted in.
 
**Scope:** this analysis considers the Core, Shared utility, Example-local, CI/Validation, and Dependency/Packaging layers. Dependency/Packaging is considered and excluded as the *primary* boundary (justified below), but is not ignored — it re-enters as a testing requirement once MPS/ONNX support exists.
 
---
 
### 2. Evidence
 
A common architectural weakness exists across the involved Examples: `government_rag` (#536) and MOT17/ReID (#524) each independently implement the same three-tier fallback (`cuda` → `mps`/or `cpu` → `cpu`) with no shared code between them — confirmed by diffing both PRs' new resolver functions, which are structurally near-identical but textually independent. Neither reads `use_gpu` from Core. Each has its own test surface (#524 has 41 unit tests on the copied logic; #536 has no dedicated unit tests in the diff). Impact: any future correction to the fallback order (e.g., adding a new backend, changing MPS detection) must be made twice today, with no guarantee the two stay consistent.
 
---
 
### 3. Scope Decision
 
**Boundary determination — backward-compatible hybrid:**
 
- **Ianvs Core** owns: legacy `use_gpu` semantics, validation, and pre-import CUDA-visibility ordering (this is what #767 fixes).
- **Shared utility** (new, e.g. `core/common/device.py`) owns: standardized resolution for `auto`, `cpu`, `cuda`, `cuda:N`, `mps`.
- **Example-local layer** owns: model `.to(device)`, tensor placement, ONNX provider configuration, framework-specific adapters, and clear errors for unsupported device capabilities.
- **CI/Validation** starts advisory and may become blocking after migration.
- **Dependency/Packaging** is *not* the primary boundary, because the Root Problem is policy/initialization behaviour, not package installation. However, backend-version constraints (e.g. minimum PyTorch version for `mps` support) must be tested when MPS/ONNX support is added to the shared resolver.
Alternatives considered and rejected: pure Example-local (rejected — this is the status quo and is what produced the duplication in Task 2); pure Ianvs Core enforcement injected into every constructor immediately (rejected — no migration path, breaks any Example not expecting a new required kwarg); pure Dependency/Packaging fix (rejected — doesn't address the ordering/policy behaviour that is the actual defect).
 
**Trade-off analysis:** a shared resolver costs one-time design and migration effort, but removes the duplication already visible between #536 and #524, and gives future Examples one thing to call instead of reinventing the fallback chain. The regression risk of a shared utility is lower than a Core-injected mandatory change, because adoption is opt-in per Example rather than forced.
 
**Cross-Example impact:** a shared resolver would affect any Example that adopts it (initially `government_rag` and MOT17/ReID, opt-in); it does not affect Examples that don't opt in, so regression risk to the other ~46 examples is close to zero at introduction time.
 
**Repair strategy (shareable, minimal):** extract the common `cuda → mps → cpu` logic already independently written in #536 and #524 into `core/common/device.py`; have both PRs (or a follow-up) call it instead of their local copies; keep the existing `use_gpu` legacy path in Core intact for backward compatibility.
 
**Device semantics:**
 
```text
use_gpu omitted:  preserve inherited environment behavior
use_gpu true:     legacy CUDA-enabled behavior
use_gpu false:    disable CUDA; does not necessarily force CPU if MPS remains available
device:auto:      use documented fallback priority
device:cpu:       force CPU/no accelerator
device:cuda:      require CUDA; fail clearly if unavailable
device:cuda:N:    require the selected CUDA index; fail clearly if unavailable
device:mps:       require MPS; fail clearly if unavailable
```
 
Only `device:auto` may silently choose a documented fallback. Explicit, unavailable devices must fail clearly rather than silently degrading. A new `device` kwarg is not injected into every constructor immediately — explicit propagation is opt-in during migration, consistent with the Example-local layer choosing when to adopt the shared resolver.
 
**Suggested affected modules:**
```text
core/testenvmanager/testenv/testenv.py
core/testcasecontroller/algorithm/paradigm/base.py
core/common/device.py                 # proposed shared resolver
Core unit tests for policy semantics
Example-specific adapters under government_rag and MOT17/ReID
CI validator fixtures
```
 
**Regression-risk matrix:**
 
| Case | Risk if shared resolver is introduced | Notes |
|---|---|---|
| Runtime auto-detection | Low | Matches what #536/#524 already do locally |
| Import-time detection | Medium | Ordering still matters; must follow #767's precedent of setting visibility before module load |
| Unconditional `.cuda()` (remaining examples) | Unaffected until migrated | Opt-in migration means unmigrated examples keep current (broken) behaviour until touched |
| Explicit CUDA-only algorithms | Low | `device:cuda` semantics fail clearly, matching current expectations |
| ONNX providers | Medium | Not currently covered by either #536 or #524; needs explicit design |
| Distributed/indexed GPUs | Medium–High | Neither current Example fix addresses `cuda:N`; needs new logic, not just extraction |
| MPS | Low | Already implemented independently twice; extraction is close to free |
| Unknown/non-PyTorch runtimes | High | Out of scope for a PyTorch-shaped resolver; must fail clearly rather than silently mis-resolve |
 
---
 
### 4. Uniqueness
 
The specific proposal to extract the *already-duplicated* `government_rag`/MOT17 resolver logic into a shared utility — rather than proposing a new mechanism in the abstract — is a repair strategy grounded directly in Task 2's evidence of independent duplication, and has not been proposed in Discussion #856, #767, #536, or #524. The device-semantics table's explicit-fails-clearly / auto-falls-back-silently distinction is a novel boundary argument not present in those threads. At least one major finding here (the structural near-duplication between #536's and #524's resolvers) meets the Evidence Standard via direct diff comparison.

### 5. Verification Boundary

The #536/#524 resolver comparison and proposed repair boundary are based on static diff inspection. The shared resolver, explicit `device` configuration, CI validator, and migration strategy are design proposals and were not implemented or executed.

Real CUDA and MPS hardware behavior was not independently verified. The proposed boundary therefore establishes architecture, semantics, regression groups, and migration trade-offs—not a claim that the shared repair has already passed.
 

### **Task 4 — Restoration Path Design**
 
---
 
### 1. Problem Definition
 
**Blockers:**
 
| ID | Blocker |
|---|---|
| B1 | #767 — Core visibility ordering |
| B2 | #536 — Government RAG migration |
| B3 | Missing documented/shared device semantics |
| B4 | Missing advisory CI guard |
| B5 | #536/#764 reconciliation |
| B6 | #524 — MOT17/ReID portability and production-test gap |
 
**Why a structured path matters:** B1–B6 have real dependency relationships (below). Fixing them in an arbitrary order risks re-breakage (e.g., building B3's shared resolver before B1's ordering fix lands would inherit the same load-before-set defect), duplicated work (B2 and B6 already independently duplicate logic B3 is meant to centralize), and hidden regressions if B5's conflict is resolved without preserving B2's province filtering.
 
**Scope:** covers `government_rag` and MOT17/ReID (the two Examples with active PRs) plus the Core mechanism they depend on. Other Examples with the same class of defect (E5 in Task 1) are excluded from this restoration path — they are not blocked on the same open PRs and would require separate targets.
 
**Dependency graph:**
 
```text
B1 (#767 — Core visibility ordering)
        │ enables
        ▼
B3 (document/shared resolver semantics)
        │
        ├── enables ──► opt-in migration for B2 (#536)
        ├── enables ──► opt-in migration for B6 (#524)
        └── enables ──► B4 (advisory CI guard)
 
B5 (#536/#764 reconciliation) ── must precede ──► B2 merge
 
Can proceed in parallel immediately:
  • B1 Core repair
  • B5/B2 Government RAG reconciliation
  • B6 #524 portability repair and production-test revision
 
Critical architectural path:
B1 → B3 → opt-in Example migration → B4
```
 
---
 
### 2. Evidence
 
| Blocker | Evidence of current impact |
|---|---|
| B1 | E1 in Task 1 — executed reproduction shows load-before-set ordering and the `absent≈True` contradiction on `main` |
| B2 | E4 in Task 1 — #536 diff removes hardcodes, adds local resolver; not yet merged |
| B3 | Task 3 — #536 and #524 independently duplicate near-identical resolver logic; no shared module exists |
| B4 | No CI check currently flags new unconditional `.cuda()` additions (absence confirmed by inspection of CI config; not separately re-verified here) |
| B5 | #536 and #764 both modify `gov_rag.py` (Task 2, section 4.3) |
| B6 | E3 in Task 1 — `basemodel.py:62, 112`; #524's tests pass (41/41) but only test copied logic, not production (Task 2) |
 
Status of existing PRs: #767 (open, addresses B1), #536 (open, addresses B2), #524 (open, addresses B6) — none merged as of this submission; no other open PR addresses B3, B4, or B5. For missing fixes (B3, B4), no existing PR was found addressing the gap.
 
**Counter-evidence / fallback:** if #767 introduces a regression in the three-state ordering (e.g., a future refactor reintroduces load-before-set), the fallback is a direct `git revert` of the Core change, since B2/B6's Example-local resolvers do not depend on #767 being merged — they call `torch.cuda.is_available()` independently and would continue to function (indirectly observing whatever CUDA visibility is currently in effect) even if #767 is reverted.
 
---
 
### 3. Path Design
 
**Fix order:** B1 → B5/B2 (parallel with B1) → B6 (parallel with B1) → B3 → opt-in migration of B2/B6 onto B3 → B4.
 
**Critical path:** B1 → B3 → opt-in migration → B4. This is the longest dependent sequence because B3 cannot be meaningfully designed before B1 establishes the ordering it must respect, and B4's CI guard is only useful once B3 exists for it to check against.
 
**Parallelization:** B5/B2 and B6 can be reviewed and repaired independently of B3's shared API; they do not need to wait for B3. Each must still satisfy its own review gates—especially the production-test revisions required for #524—before merging. B3 only gates the later migration of B2/B6 onto a shared resolver.

**Repair strategy sketch:** (1) merge #767 for B1; (2) resolve the B5 conflict between #536 and #764 in `gov_rag.py`, preserving the province-scoped filter added in `d2d062d`, then merge #536 for B2; (3) merge #524 for B6, addressing the Task 2 major-revision items (import production `BaseModel` from tests, add a CPU smoke test and a Core-contract test) before or shortly after merge; (4) design and land `core/common/device.py` for B3, using #536/#524's resolvers as the reference implementation; (5) add an advisory CI check for B4 that flags new unconditional `.cuda()`/`"cuda"` literals without failing the build initially.
 
**Rollback strategy:** each stage's rollback is a `git revert` of that stage's merge commit; because B2 and B6 do not depend on B3 existing, reverting B3 does not require reverting B2/B6.
 
**Cross-Example coordination:** B3's eventual adoption by B2/B6 should be staggered (migrate one Example, verify, then the other) rather than simultaneous, to isolate any regression to a single Example's test suite.
 
---
 
### 4. Verification
 
**Verification Gates**
 
| Gate | After stage | Action | Expected | Failure condition | Status |
|---|---|---|---|---|---|
| G1 | B1 (#767) | Run the executed Core ordering probe (Task 2 → Reproduce) on `main` and `pull/767/head` | `main` `false` → LOAD, environment unset; `#767` `false` → SET `-1` then LOAD; `#767` omitted → environment unchanged | Order reversed, or wrong value | **EXECUTED** |
| G2 | B6 (#524) | `pytest examples/MOT17/multiedge_inference_bench/pedestrian_tracking/testalgorithms/reid/m3l/test_basemodel.py -v` | 41 PR-added tests pass | Any failure | **EXECUTED ON CPU** — boundary: copied-helper tests only, not integration |
| G3 | B6 (#524, follow-up) | Import production `basemodel.py` with external dependencies stubbed only at import boundaries; construct/load `BaseModel` on CPU | No immediate CUDA demand / clean fallback | Still forces `"cuda"` | NOT EXECUTED |
| G4 | B3/B6 | Invoke real `MultiEdgeInference._inference()` and `_inference_mp()` with a capturing job | Required dataset context is provided, or unsupported model-parallel use fails clearly | Silent `AttributeError` or wrong result | NOT EXECUTED |
| G5 | B2/B5 | Create a synthetic two-province persisted store, reload with one province selected, perform retrieval | No document from the unselected province is returned | Cross-province leakage | NOT INDEPENDENTLY EXECUTED (PR author reported equivalent validation) |
| G6 | B3 | Policy semantics matrix across omitted/true/false/auto/cpu/cuda/cuda:N/mps | All cases match Task 3's semantics table | Any case silently mismatches | NOT EXECUTED |
| G7 | B4 | Advisory CI fixture with an unsafe (new unconditional `.cuda()`) and a guarded case | Unsafe fixture warned; guarded fixture accepted | Silent on unsafe fixture | NOT EXECUTED |
 
Only G1 and G2 currently have execution results; all others are proposed and explicitly marked not executed, per the Verification Boundary standard.
 
**Independent verification:** G1 and G2 are directly tied to the ordering and three-state findings of Task 1 and the review of #767/#524 in Task 2, and — unlike a purely descriptive gate — both have actually been run against real `main`/`pull/767/head`/`pull/524/head` code, not just described in the abstract.
 
**Incremental regression testing:** after G1 passes, G2 should be re-run to confirm B6's fix is unaffected by B1's change (the two touch unrelated code paths, so no interaction is expected, but this has not yet been executed together in one pass — noted as a gap to close before final submission).
 
**Final success criteria:** holistic restoration is declared only when G1–G7 all pass (not just G1/G2), `government_rag` and MOT17/ReID both run their respective test suites against production code (not copied helpers), and documentation for the shared device semantics (Task 3) is published — not merely when one error message disappears.
 
**Verification Boundary**

Only G1 and G2 have execution results. G3–G7 remain proposed and must not be described as passed.

G2 executes PR-added copied-helper tests, not production MOT17 integration. Full Government RAG and MOT17 benchmarks require external datasets, checkpoints, models, and API credentials and were not run. Real CUDA and MPS hardware behavior remains outside the completed verification boundary.

---
 
### 5. Uniqueness
 
The dependency graph's explicit claim that B2 and B6 do **not** need to wait for B3 is a novel scope decision relative to a naive "fix Core first, then everything else" ordering, and follows directly from Task 2's evidence that both Example PRs already work independently of Core. The staggered-migration rollback strategy for B3 is not proposed elsewhere. G1 and G2 being genuinely executed — rather than described — is itself a novel verification strategy relative to the fully-prose gates typical of static analyses on this Root Problem.
 

# Bonus — Supplementary Open-PR Review Coverage

## Qualification and uniqueness

This Bonus contains **four additional reviews**. All four PRs are open, non-draft, within the permitted PR range, not authored by me, and distinct from my mandatory Task 2 set (#767/#536/#524). I also excluded the duplicated targets previously covered by Discussion #864. Each finding below was cross-posted to its target PR and is backed by executed evidence with an explicit verification boundary.

| PR | Reviewed commit | Decision | Unique finding | Permanent target comment |
|---|---|---|---|---|
| [#808](https://github.com/kubeedge/ianvs/pull/808) | [`28cff7e`](https://github.com/kubeedge/ianvs/commit/28cff7eb87b8e9a4b1d3f6544716f395e5b465bd) | Major revision | `np.isin` fix still samples label-array positions rather than real labels | [review](https://github.com/kubeedge/ianvs/pull/808#issuecomment-5386552612) |
| [#827](https://github.com/kubeedge/ianvs/pull/827) | [`beb23be`](https://github.com/kubeedge/ianvs/commit/beb23be64977b14f09a45fbf4cc8bfc32dc9633f) | Major revision | Added no-op is behaviorally identical to the already-guarded current Core path | [review](https://github.com/kubeedge/ianvs/pull/827#issuecomment-5386637841) |
| [#775](https://github.com/kubeedge/ianvs/pull/775) | [`6803326`](https://github.com/kubeedge/ianvs/commit/6803326aed7876b47330a13949403d5ec3417f42) | Major revision | Test collection globally replaces NumPy and pandas in `sys.modules` | [review](https://github.com/kubeedge/ianvs/pull/775#issuecomment-5386643381) |
| [#802](https://github.com/kubeedge/ianvs/pull/802) | Original `a6c3b41`; final verified update [`89c2a01`](https://github.com/kubeedge/ianvs/commit/89c2a013de1185d9ec137e1045c8337334aed861) | **All findings and follow-up suggestions addressed** | Author implemented fail-fast worker handling, complete client-set validation, and regression tests after review | [original review](https://github.com/kubeedge/ianvs/pull/802#issuecomment-5386647117) · [first response](https://github.com/kubeedge/ianvs/pull/802#issuecomment-5387002670) · [first follow-up](https://github.com/kubeedge/ianvs/pull/802#issuecomment-5387138574) · [hardening response](https://github.com/kubeedge/ianvs/pull/802#issuecomment-5387170752) · [final verification](https://github.com/kubeedge/ianvs/pull/802#issuecomment-5387198216) |

## Review 1 — PR #808: sample real labels, not positions

At [`utils.py` lines 83–91](https://github.com/kubeedge/ianvs/blob/28cff7eb87b8e9a4b1d3f6544716f395e5b465bd/core/testenvmanager/dataset/utils.py#L83-L91), the PR derives `class_num` from `np.unique(y_data)` but samples from `range(class_num)`. Those integers are positions, not necessarily values in `y_data`; 1-based, sparse numeric, and string labels can therefore produce incomplete or empty partitions.

Executed with labels `[10, 10, 20, 20, 30, 30]`, ratio `2/3`, and `random.seed(7)`:

```text
unique labels       = [10, 20, 30]
PR selected values  = [1, 0]
PR indices/labels   = [] / []
fixed selected      = [20, 10]
fixed indices/labels= [0, 1, 2, 3] / [10, 10, 20, 20]
```

**Recommendation:** sample from `np.unique(y_data).tolist()` and define or reject the `sample_number == 0` case. Add non-zero-based and string-label tests.

**Verification boundary:** I executed the changed selection predicate with NumPy, but not the complete federated-learning pipeline or downstream empty-client handling.

## Review 2 — PR #827: current Core already handles the missing method

Current Core guards the call at [`singletask_learning.py` lines 113–116](https://github.com/kubeedge/ianvs/blob/37a9c60a9747af0cfe3170f84249bc349c56e8d5/core/testcasecontroller/algorithm/paradigm/singletask_learning/singletask_learning.py#L113-L116): if `preprocess` is absent, `_preprocess()` returns `None`. The PR's new method also returns `None`, so it cannot prevent the `AttributeError` claimed in the PR body on this call path.

I executed the exact production `_preprocess` function from `main@37a9c60`:

```text
Smart Coding object without preprocess: None
Object with PR's no-op preprocess:      None
```

**Alternative explanation considered:** Sedna or another external integration might call `BaseModel.preprocess()` directly, making the method useful even though Ianvs Core guards it. I cannot exclude that possibility from the local call path alone. However, the PR specifically attributes the failure to Ianvs single-task learning, identifies no external caller, and supplies no failing test. That alternative is therefore a reason to request a concrete reproduction—not evidence that this patch fixes the stated current-Core defect.

**Recommendation:** provide a failing current-Core reproduction through the real Smart Coding call path. If no other caller exists, close the PR as obsolete; otherwise identify and test that caller.

**Verification boundary:** I executed the current production function in isolation. I did not instantiate the model or rule out an unidentified external Sedna caller.

## Review 3 — PR #775: interpreter-global test contamination

At [`test_rank_sort.py` lines 19–21](https://github.com/kubeedge/ianvs/blob/6803326aed7876b47330a13949403d5ec3417f42/core/storymanager/rank/test_rank_sort.py#L19-L21), test-module import unconditionally assigns `MagicMock()` objects to `sys.modules['numpy']`, `sys.modules['pandas']`, and the visualization module. Pytest imports tests during collection, so later imports in the same process can receive mocks instead of real runtime dependencies. The tests also use a mock dataframe, so they do not verify real sorting results.

I imported the PR's actual test module after loading real NumPy 2.4.6:

```text
before numpy: 2.4.6 module
after numpy is MagicMock: True
after pandas is MagicMock: True
visualization is MagicMock: True
```

**Recommendation:** remove global replacements, test with a small real `pandas.DataFrame`, and scope any unavoidable visualization patch to a fixture/context manager that restores state.

**Verification boundary:** I executed the actual test-module import and confirmed contamination. I did not run the entire suite to identify a particular later failure under one collection order.

## Review 4 — PR #802: incomplete rounds originally continued silently; author fixed after review

My [original review](https://github.com/kubeedge/ianvs/pull/802#issuecomment-5386647117) found that `if i in train_info_map` silently skipped missing clients. Because the coordinator joined plain worker threads without propagating their exceptions, a failed worker could leave its result absent and permit partial helper-state distribution.

Executed against the originally reviewed implementation with two expected clients but only client 0 present:

```text
client 0 calls: [{'client_id': 0}]
client 1 calls: []
method returned without error despite missing client 1
```

**Recommendation made:** require the exact expected client-ID set after worker joins, capture worker exceptions, and fail before aggregation/helper distribution.

### Direct reviewer impact and follow-up

The [PR author acknowledged the finding](https://github.com/kubeedge/ianvs/pull/802#issuecomment-5387002670) and updated the PR. Current commit [`998290e`](https://github.com/kubeedge/ianvs/commit/998290ebd9b23629a3ebb28a5c99b7a034768506):

1. captures worker exceptions and raises a coordinator-level `RuntimeError` after joins;
2. validates missing client IDs before helper-state distribution; and
3. adds tests for incomplete `train_infos` and worker failure.

I independently executed the exact updated `helper_function()` with the same incomplete input:

```text
Expected clients: [0, 1]
Provided train_infos: {0: {'client_id': 0}}
RuntimeError: incomplete train_infos in helper_function: missing client(s) [1], expected [0, 1]
```

![PR #802 follow-up execution at 998290e](https://github.com/user-attachments/assets/c1212a54-c37e-42e4-9558-37b9ae769e24)

**First follow-up decision:** [original blocking finding resolved at `998290e`](https://github.com/kubeedge/ianvs/pull/802#issuecomment-5387138574). I left one non-blocking suggestion to reject unexpected client IDs as well as missing IDs.

The [author then adopted that hardening suggestion](https://github.com/kubeedge/ianvs/pull/802#issuecomment-5387170752), added an unexpected-ID regression test, and updated the PR to [`89c2a01`](https://github.com/kubeedge/ianvs/commit/89c2a013de1185d9ec137e1045c8337334aed861). I executed the exact final helper method with expected IDs `[0, 1]` and supplied IDs `[0, 1, 5]`:

```text
RuntimeError: invalid train_infos in helper_function: unexpected client(s) [5], expected [0, 1]
```

**Final decision:** [all findings and follow-up suggestions addressed at `89c2a01`](https://github.com/kubeedge/ianvs/pull/802#issuecomment-5387198216).

**Verification boundary:** I executed the exact original helper and both updated helper behaviors with controlled collaborators. I inspected the worker-exception path and added tests statically, but did not independently run the complete eight-test module or full FCIL benchmark.

## Combined value

These are four independent review findings across dataset partitioning, Example/Core contract validation, test isolation, and federated concurrency. They are not restatements of the PR bodies: each identifies a separate unresolved or newly introduced failure mode, demonstrates it with executed evidence, and provides a bounded repair recommendation. PR #802 additionally demonstrates a complete reviewer-impact loop: the author accepted the blocking finding, implemented both requested safeguards, added regression tests, then adopted the follow-up hardening suggestion; both corrected behaviors were independently re-executed.

---

## Bonus addendum — three further supplementary reviews

This is an **addendum** to the existing four-review Bonus section above; that section and its PR #802 reviewer-impact chain are preserved unchanged. These are Bonus reviews **5–7**, not replacements for the original four.

All three targets are open, non-draft PRs in the permitted range, are not authored by me, and are outside my mandatory Task 2 set (#767/#536/#524). The findings are distinct from the original four and from each other. I did not use PR #790 because its relevant issues are already covered by an existing human review. Each review was cross-posted to the target PR and includes a bounded verification statement.

| Bonus review | Pinned PR head | Decision | Distinct technical finding | Permanent target comment |
|---|---|---|---|---|
| **5 — [PR #773](https://github.com/kubeedge/ianvs/pull/773)** | [`835f4457`](https://github.com/kubeedge/ianvs/commit/835f4457f387eff218541ca8d433b465dd960765) | Request changes | The cloud cache key does not represent the constructor-level effective generation settings used by `_call_api()`. Requests with the same input/model/privacy values can therefore share a cache entry despite different effective generation kwargs. | [review](https://github.com/kubeedge/ianvs/pull/773#issuecomment-5409809791) |
| **6 — [PR #734](https://github.com/kubeedge/ianvs/pull/734)** | [`646b085`](https://github.com/kubeedge/ianvs/commit/646b085829fa09d83a4f0c2730102aaaaf50cf9a) | Major revision | `map50.py` and `map90.py` import `PIL.Image` without declaring `Pillow`; missing or unreadable images silently fall back to 640×480, which can corrupt normalized-box metrics for other image dimensions. | [review](https://github.com/kubeedge/ianvs/pull/734#issuecomment-5409817163) |
| **7 — [PR #701](https://github.com/kubeedge/ianvs/pull/701)** | [`77daad3`](https://github.com/kubeedge/ianvs/commit/77daad3a09868d57efab69d562bde38d0af3bd6b) | Major revision | The added JSONL records parse, but reference `resources/datasets/sample.pdf` and `resources/datasets/sample.png`, which are absent from the PR tree. | [review](https://github.com/kubeedge/ianvs/pull/701#issuecomment-5409823176) |

### Review 5 — PR #773: cache identity omits effective generation configuration

At the pinned head, `CloudModelAPI.inference()` derives the cache key from call-time kwargs, while `_call_api()` forwards the constructor's `self.kwargs`. The key therefore does not reliably encode the request configuration that is actually sent. Two requests with the same input, model, and privacy values can collide even when their effective generation settings differ.

**Recommendation:** construct the cache identity from one normalized effective request configuration—the same merged/defaulted settings passed to `_call_api()`—and add a regression test that issues identical input/model/privacy requests with different effective generation settings and asserts that they do not reuse one another's cached result.

**Verification boundary:** I inspected the pinned PR implementation and executed a focused local control-flow/key comparison. I did not call a real remote provider or run the complete cloud-model test suite.

### Review 6 — PR #734: undeclared image dependency and unsafe dimension fallback

At the pinned head, `map50.py` and `map90.py` import `PIL.Image`, but the repository requirements do not declare `Pillow`. In addition, missing or unreadable images silently use a 640×480 fallback. This is not dimension-neutral: an 800×600 normalized centered box maps to `(300,225,500,375)` under 640×480, rather than `(240,180,400,300)` under the actual dimensions; the resulting IoU is `0.179856` in the checked case.

**Recommendation:** declare `Pillow` in the supported dependency manifest, and fail explicitly or exclude the sample with a visible diagnostic when its image cannot be read instead of fabricating dimensions. Add tests for the dependency/import path, actual image dimensions, unreadable images, and non-640×480 normalized-coordinate conversion.

**Verification boundary:** I compiled the changed Python files, checked the repository dependency declarations, and executed the coordinate conversion/IoU example. I did not run the complete mAP pipeline or validate every external dataset layout.

### Review 7 — PR #701: JSONL fixtures reference absent assets

At the pinned head, both added JSONL records parse, but their `resources/datasets/sample.pdf` and `resources/datasets/sample.png` references do not resolve because those files are absent from the PR tree. A clean checkout therefore cannot execute the records as self-contained fixtures unless an undocumented dataset-preparation step supplies the assets.

**Recommendation:** add the referenced fixtures, or document and automate the dataset preparation required before running the records. Add a validation test or CI check that resolves every fixture path and fails with an actionable message when an asset is missing.

**Verification boundary:** I parsed both new JSONL records and inspected the pinned PR tree for the two referenced paths. I did not run the full example pipeline or infer whether an external download step exists outside the PR.

These three reviews add independent coverage of cache correctness, metric/dependency correctness, and fixture reproducibility. They are offered as evidence for Bonus qualification and reviewer value; no separate score is self-awarded.
