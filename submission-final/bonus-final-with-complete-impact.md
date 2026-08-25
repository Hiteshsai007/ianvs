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
