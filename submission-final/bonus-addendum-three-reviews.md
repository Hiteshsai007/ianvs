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
