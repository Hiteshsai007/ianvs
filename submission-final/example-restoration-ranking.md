# Comparative ranking — Show-and-tell / Example Restoration Discussions

**Snapshot date:** 2026-08-25 (Asia/Calcutta)

**Scope:** all 41 current repository `Show and tell` Discussions whose title identifies them as `Example Restoration`. Simulation Sandbox, Q&A, template/placeholder, and unrelated Show-and-tell threads are excluded.

**Census continuity:** The earlier 36-thread baseline is retained in full. The five current additions are **#921, #922, #923, #924, and #925**; they are separately identified in the table and summarized below. #864 remains in the census but is excluded from substantive scoring.

**Important:** these are comparative estimates, not official evaluator grades. They are based on the public Discussion bodies/comments and the target-specific links/comments visible in the repository-wide snapshot. A target-specific comment that is only claimed but not linked or independently matched is marked as partial/unverified and lowers confidence.

## Scoring method

The official structure is preserved:

- Task 1 — Root Problem Analysis: **30**
- Task 2 — Multi-PR Code Review: **30**
- Task 3 — Repair Boundary Analysis: **20**
- Task 4 — Restoration Path Design: **20**
- Bonus: **up to 15**
  - 4–6 additional reviews: +5
  - 7–9: +8
  - 10+: +10
  - quality/impact: up to +5

For consistent comparison, each task estimate uses a balanced internal split (not an official evaluator sub-rubric):

- Task 1: root-cause/causal chain 10, evidence/reproduction 10, impact/uniqueness 10.
- Task 2: target/coverage discipline 6, review depth and recommendations 16, evidence/cross-PR reasoning 8.
- Task 3: boundary decision 8, trade-offs/impact 6, scope/uniqueness 6.
- Task 4: sequencing/path 8, verification/rollback gates 8, scope/clarity 4.

A video is not a separate rubric category. Where present, it strengthens the relevant evidence assessment. Missing target-specific comments are treated as an evidence/qualification problem, not as proof that a contributor did not post them.

**Fresh audit basis (2026-08-25):** I fetched all 41 in-scope Discussions and their 128 comments (about 1.4 million characters), then reconciled 323 unique mentioned URLs. GitHub repository/issue/PR links were checked through the GitHub API; Discussion comment anchors were matched against the fetched comment lists; image/raw-asset links were opened or checked through their GitHub redirect/API paths. The YouTube link could not be content-fetched from this sandbox because of a TLS transport failure, so it is counted only as linked evidence, not as independently verified playback.

**Scoring exclusion:** Discussion #864 is retained as one of the 41 current threads for census completeness, but is excluded from comparative scoring at the candidate's request. Its substantive claims, task points, Bonus points, and ranking position are not used; the table records it as **Excluded** rather than assigning replacement zeroes.

## Midpoint ranking

| Rank | Discussion / candidate | T1 /30 | T2 /30 | T3 /20 | T4 /20 | Bonus /15 | Estimated total /115 | Evidence and qualification note |
|---:|---|---:|---:|---:|---:|---:|---:|---|
| 1 | [#924](https://github.com/kubeedge/ianvs/discussions/924) — akshhkaushik | 29 | 29 | 19 | 19 | 15 | **111** | High-quality full Task 1–4 package: executed Core/RoboDK lifetime evidence, mandatory PR reviews with merge/regression analysis, explicit ownership boundary, seven verification gates, and eleven distinct Bonus reviews plus a cross-target finding. |
| 2 | [#873](https://github.com/kubeedge/ianvs/discussions/873) — AdityaKumarSethia | 29 | 29 | 19 | 18 | 13 | **108** | Very strong executed unguarded-division analysis, merge-conflict reasoning, five mandatory PR reviews, seven additional reviews, and extensive target-specific comment evidence. |
| — | [#864](https://github.com/kubeedge/ianvs/discussions/864) — akshita317 | — | — | — | — | — | **Excluded** | Retained in the 41-thread census only; no substantive #864 content is used in this comparative score or in another candidate's assessment. |
| 3 | [#875](https://github.com/kubeedge/ianvs/discussions/875) — Hiteshsai007 | 29 | 29 | 18 | 19 | 12 | **107** | Complete dual-channel submission with pinned heads, executed Core/#767 evidence, new production-path smoke tests for #524 and #536, an executed #767/#536/#524/#764 merge matrix, G0 red/green contract evidence, authentic `41 passed in 1.23s` video, and seven distinct additional reviews. |
| 4 | [#921](https://github.com/kubeedge/ianvs/discussions/921) — MukandKrishna | 29 | 29 | 19 | 19 | 10 | **106** | Strong full package: four executed identity/schema reproductions, four detailed mandatory PR reviews, two maintainer-impact loops with 4- and 6-test focused reruns, explicit shared-contract boundary, and four Bonus reviews. Full model/training workflows remain outside its stated boundary. |
| 5 | [#869](https://github.com/kubeedge/ianvs/discussions/869) — whozahm3d | 28 | 29 | 18 | 18 | 5 | **98** | Strong incremental-learning/dependency/path synthesis, novel cross-PR type-contract and conflict analysis, direct target comments, and documented upstream acknowledgment/update. Two additional reviews limit quantity bonus. |
| 6 | [#904](https://github.com/kubeedge/ianvs/discussions/904) — zjr060424-lab | 28 | 28 | 19 | 18 | 4 | **97** | High-quality process-global runtime-context analysis, controlled execution, six images, six target-specific technical comments, and careful verification limits; no qualifying Bonus review set. |
| 7 | [#900](https://github.com/kubeedge/ianvs/discussions/900) — sheikhayaan | 27 | 22 | 17 | 17 | 12 | **95** | Technically very dense path-resolution/validator analysis with four mandatory and seven additional reviews. The public Discussion does not link most target-specific comments; direct matching is therefore incomplete and the estimate is medium confidence. |
| 8 | [#868](https://github.com/kubeedge/ianvs/discussions/868) — kavaljeetsingh-dev | 25 | 23 | 17 | 17 | 12 | **94** | Strong dependency-manifest/validator analysis and seven additional reviews. Target-specific posting is claimed and the author has matching target activity, but the Discussion itself contains few direct target links; score confidence is medium. |
| 9 | [#870](https://github.com/kubeedge/ianvs/discussions/870) — hibaa23 | 27 | 27 | 18 | 17 | 4 | **93** | Strong Sedna/install-boundary analysis, correct separation of packaging/docs/CI layers, three mandatory and three additional reviews, with target-specific comments visible. |
| 10 | [#862](https://github.com/kubeedge/ianvs/discussions/862) — Arijit429 | 25 | 22 | 17 | 16 | 12 | **92** | Substantial config-validation analysis, three mandatory PRs, eight additional reviews, and good technical detail. Only one target URL is present in the Discussion; the wider dual-channel record is not fully verifiable from the snapshot. |
| 11 | [#866](https://github.com/kubeedge/ianvs/discussions/866) — mwemmz | 24 | 22 | 16 | 16 | 9 | **87** | Strong executed path-drift evidence and a useful correction/erratum; five additional reviews. Target-specific posting is claimed, but the Discussion has no direct target-comment links and only partial author-side matching. |
| 12 | [#892](https://github.com/kubeedge/ianvs/discussions/892) — hira299 | 24 | 22 | 16 | 15 | 3 | **80** | Corrected, detailed non-IID/config-validation report with careful source corrections and useful review coverage. Target-specific comment evidence is sparse in the Discussion. |
| 13 | [#916](https://github.com/kubeedge/ianvs/discussions/916) — blackdragoon26 | 22 | 22 | 15 | 15 | 3 | **77** | Compact but technically good contract analysis with exact PR-head execution, three mandatory and three additional reviews. Target references are present, but target-specific comments are not linked. |
| 14 | [#856](https://github.com/kubeedge/ianvs/discussions/856) — nirdesho6o | 24 | 22 | 15 | 15 | 0 | **76** | Good executed `use_gpu`/CUDA boundary analysis and three mandatory PR reviews, but no Bonus section and no target-specific comments matched in the snapshot. |
| 15 | [#878](https://github.com/kubeedge/ianvs/discussions/878) — Omkeswani27 | 22 | 21 | 15 | 15 | 2 | **75** | Complete dynamic-loader analysis and two supplementary reviews, but target-specific comments are not evidenced by direct links. |
| 16 | [#861](https://github.com/kubeedge/ianvs/discussions/861) — diegodevxd | 22 | 20 | 15 | 15 | 2 | **74** | Complete, honest verification boundaries and useful reviewer-impact evidence; only two supplementary reviews and incomplete direct target-link evidence. |
| 17 | [#899](https://github.com/kubeedge/ianvs/discussions/899) — greenfield01 | 23 | 21 | 15 | 15 | 0 | **74** | Substantive validator-contract analysis with all four tasks, but no Bonus comment and no target-specific links visible in the snapshot. |
| 18 | [#917](https://github.com/kubeedge/ianvs/discussions/917) — yadavchiragg | 20 | 18 | 13 | 13 | 7 | **71** | All tasks and four additional reviews are present, but the report is comparatively thin and target-specific comments are not matched in the snapshot. |
| 19 | [#886](https://github.com/kubeedge/ianvs/discussions/886) — ishansurdi | 21 | 20 | 14 | 14 | 0 | **69** | Complete unsafe-algorithm-configuration analysis with some target links, but no Bonus section and less extensive evidence than the leading submissions. |
| 20 | [#877](https://github.com/kubeedge/ianvs/discussions/877) — PRANABKUMARNEOGI | 20 | 19 | 14 | 14 | 2 | **69** | Complete path-contract submission, but the three Bonus target links are placeholders (`#`), so target-specific/Bonus qualification is weak. |
| 21 | [#911](https://github.com/kubeedge/ianvs/discussions/911) — TanishChahal | 19 | 17 | 13 | 13 | 6 | **68** | Complete dependency/build submission with four additional reviews, but very short evidence package and no verifiable target-specific links. |
| 22 | [#909](https://github.com/kubeedge/ianvs/discussions/909) — Aryansingh-ai | 23 | 20 | 14 | 0 | 0 | **57** | Strong Task 1–3 dynamic-loader material and five claimed review targets, but a separate Task 4/restoration path is not present in the fetched Discussion comments. |
| 23 | [#903](https://github.com/kubeedge/ianvs/discussions/903) — Yash4616 | 23 | 20 | 13 | 0 | 0 | **56** | Substantial Task 1–3 content, but Task 4 and Bonus are not present as completed comments. |
| 24 | [#882](https://github.com/kubeedge/ianvs/discussions/882) — rajdhruvsingh | 25 | 24 | 0 | 0 | 0 | **49** | Very substantial Task 1–2 material, but the fetched Discussion contains only those two task comments; Task 3, Task 4, and Bonus are missing. |
| 25 | [#906](https://github.com/kubeedge/ianvs/discussions/906) — ijayhub | 16 | 8 | 0 | 0 | 0 | **24** | Only partial Task 1–2 material; no completed Task 3/4 or Bonus. |
| 26 | [#923](https://github.com/kubeedge/ianvs/discussions/923) — asadulla-h | 23 | 0 | 0 | 0 | 0 | **23** | Substantive Task 1 only: direct execution of the vendored RFNet path failure, byte-for-byte duplication across four Examples, and cross-PR scope analysis; no completed Task 2–4 or Bonus package. |
| 27 | [#922](https://github.com/kubeedge/ianvs/discussions/922) — Shikhar-Kesharwani | 22 | 0 | 0 | 0 | 0 | **22** | Substantive Task 1 only: two dependency-boundary failures independently reproduced with clear cross-issue analysis; no completed Task 2–4 or Bonus package. |
| 28 | [#894](https://github.com/kubeedge/ianvs/discussions/894) — christinaliyevav-coder | 21 | 0 | 0 | 0 | 0 | **21** | Task 1 plus a partial Bonus reference; mandatory Task 2–4 content is absent. |
| 29 | [#883](https://github.com/kubeedge/ianvs/discussions/883) — iron-prog | 18 | 0 | 0 | 0 | 0 | **18** | Task 1 only; the remaining rubric tasks are absent. |
| 30 | [#925](https://github.com/kubeedge/ianvs/discussions/925) — azaynul10 | 18 | 0 | 0 | 0 | 0 | **18** | Concise root-problem analysis of an unreachable/unsafe `save_mode` path across three issues, but no separate scored Task 2–4 or Bonus sections and no executed output shown in the fetched body. |
| 31 | [#867](https://github.com/kubeedge/ianvs/discussions/867) — hariomphulre | 17 | 0 | 0 | 0 | 0 | **17** | Task 1 only; no completed Task 2–4 or Bonus comments. |
| 32 | [#914](https://github.com/kubeedge/ianvs/discussions/914) — 31groot | 17 | 0 | 0 | 0 | 0 | **17** | Task 1 only; no completed Task 2–4 or Bonus package. |
| 33 | [#887](https://github.com/kubeedge/ianvs/discussions/887) — Muhammad-Hashir-Code | 15 | 0 | 0 | 0 | 0 | **15** | Task 1 only; no completed multi-PR, boundary, path, or Bonus sections. |
| 34 | [#915](https://github.com/kubeedge/ianvs/discussions/915) — ChiragSW | 15 | 0 | 0 | 0 | 0 | **15** | Root-problem analysis only; no completed Task 1–4/Bonus structure was present in the fetched snapshot. |
| 35 | [#907](https://github.com/kubeedge/ianvs/discussions/907) — Raghavbhola | 14 | 0 | 0 | 0 | 0 | **14** | A short Task 1 post and Bonus mention, without the required multi-PR/boundary/path package. |
| 36 | [#865](https://github.com/kubeedge/ianvs/discussions/865) — Prachi194agrawal | 10 | 0 | 0 | 0 | 0 | **10** | A scope/index post says the task comments will follow, but no task comments were present in the fetched snapshot. |
| 37 | [#855](https://github.com/kubeedge/ianvs/discussions/855) — suhaan-24 | 6 | 0 | 0 | 0 | 0 | **6** | Short introductory analysis only; no completed scored task package. |
| 38 | [#852](https://github.com/kubeedge/ianvs/discussions/852) — MooreZheng | 0 | 0 | 0 | 0 | 0 | **0** | Placeholder/task labels without substantive task content. |
| 39 | [#874](https://github.com/kubeedge/ianvs/discussions/874) — iamdevdhanush | 0 | 0 | 0 | 0 | 0 | **0** | Placeholder/introductory post without substantive task content. |
| 40 | [#885](https://github.com/kubeedge/ianvs/discussions/885) — unfunnycat | 0 | 0 | 0 | 0 | 0 | **0** | Very short root-problem placeholder; no completed task comments. |

## Five additions to the earlier 36-thread baseline

| Discussion | Current comparative result | Why it matters to the expanded census |
|---|---:|---|
| [#921](https://github.com/kubeedge/ianvs/discussions/921) | **106/115, rank 4** | Full Task 1–4 package with executed identity/schema evidence, mandatory review and maintainer-impact loops, plus four Bonus reviews. |
| [#922](https://github.com/kubeedge/ianvs/discussions/922) | **22/115, rank 27** | Substantive executed Task 1 dependency-boundary analysis; Tasks 2–4 and Bonus are absent. |
| [#923](https://github.com/kubeedge/ianvs/discussions/923) | **23/115, rank 26** | Substantive executed Task 1 vendored-RFNet analysis across five Examples and four PRs; Tasks 2–4 and Bonus are absent. |
| [#924](https://github.com/kubeedge/ianvs/discussions/924) | **111/115, rank 1** | Strongest new full package: executed ownership/lifetime evidence, mandatory PR integration analysis, seven gates, eleven Bonus reviews, and a cross-target finding. |
| [#925](https://github.com/kubeedge/ianvs/discussions/925) | **18/115, rank 30** | Concise Task 1 root-problem analysis; no completed Task 2–4 or Bonus package. |

## Interpretation

### Highest-confidence leading group

1. **#924** — strongest current combination of full task completeness, executed lifetime/ownership evidence, mandatory-PR integration analysis, fresh verification gates, and eleven high-coverage Bonus reviews.
2. **#873** — strongest repeated-pattern/root-cause analysis, real method and merge-conflict execution, five mandatory PR reviews, seven Bonus reviews, and extensive target-specific evidence.
3. **#875** — now strengthened by production-path smoke tests for #524/#536, the exact merge matrix, and G0 red/green evidence; it remains one point below #873 and above #921 on this comparative estimate.
4. **#921** — unusually strong identity-contract analysis with direct reproductions, two verified maintainer-impact loops, complete Tasks 1–4, and four qualifying Bonus reviews.

### High-quality but confidence-limited group

- **#869**, **#904**, and **#900** have strong technical depth; #900's and #868/#862's target-link coverage is less complete or less directly matched in the central Discussion.
- **#875** is now estimated at 107 after the new production-path probes, merge matrix, and G0 evidence; it remains four points below #924 and one below #873. Its Bonus quantity is already in the same 7–9 tier as #873, so further improvement should come from production integration or cross-stage execution evidence, not additional review count.
- **#864** is not compared or scored here. It is retained only to keep the current 41-thread census complete.

### Main reasons for lower placement

- Missing Task 3 or Task 4 is a hard completeness problem even when Task 1–2 are strong (#882, #903, #909).
- A claim that target comments were cross-posted is weaker than a live target-specific URL or an independently matched target comment.
- Placeholder links and unverified Bonus claims do not provide the same evidence as permanent target URLs.
- Short/index-only threads are not comparable to completed submissions (#852, #855, #865, #874, #885, #925).
- #922 and #923 contain substantive, executed Task 1 material but no completed Task 2–4/Bonus package; they are therefore not placed near full submissions.

## #875 placement note

The #875 estimate is deliberately not treated as an official grade. Its strongest differentiators are complete Task 1–4/Bonus structure, verified dual-channel target comments, explicit execution boundaries, executed PR #767 evidence, authentic PR #524 `41 passed in 1.23s` evidence, and the linked execution video. The video is an evidence-strength factor within Task 2; it is not a separate Bonus category.

The seven Bonus reviews place #875 in the 7–9 quantity band (+8). The new bounded production probes for #524 and #536, the executed merge matrix, and the G0 red/green contract gate strengthen Task 1 and Task 4 evidence, moving the comparative estimate to **107/115**. That remains below #873's estimated 108 and #924's estimated 111, while placing #875 above #921's estimated 106. The remaining path above #873 is fresh, bounded production integration or cross-stage execution evidence, not additional shallow or duplicated reviews.
