# PR #536 production Government RAG smoke test

**Executed:** 2026-08-25  
**Repository:** `kubeedge/ianvs`  
**Baseline:** `main@37a9c60a9747af0cfe3170f84249bc349c56e8d5`  
**PR head:** `#536@d2d062d21533dc3ab2879a495d5ad4eeafa57c42`

## Purpose

This is a bounded production-path check for the device-resolution change in PR #536. The actual `GovernmentRAG` class and its actual `__init__` path were imported from each pinned revision. No replacement `default_device()` implementation was used.

The harness supplied import-boundary stubs for LangChain, Torch, Transformers, and tqdm, created a minimal `dataset/Karnataka` fixture, and supplied an existing temporary Chroma directory so the constructor exercised its real validation, province selection, device resolution, embeddings construction, and persisted-store load without downloading models or documents.

The fake Torch environment reported **CUDA unavailable** and **MPS available**, making the baseline's `cuda-or-cpu` default distinguishable from PR #536's `cuda-or-mps-or-cpu` resolver.

## Baseline result — `main@37a9c60`

```text
SOURCE: /tmp/ianvs-main-smoke/examples/government_rag/singletask_learning_bench/testalgorithms/gov_rag.py
CLASS: production_gov_rag.GovernmentRAG
PROVINCES: ['Karnataka']
EMBEDDINGS_DEVICE: HuggingFaceEmbeddings(device=cpu)
EVENTS:
  HuggingFaceEmbeddings(device=cpu)
  Chroma(existing_store=True)
```

## PR #536 result — `d2d062d`

```text
SOURCE: /tmp/ianvs-pr536/examples/government_rag/singletask_learning_bench/testalgorithms/gov_rag.py
CLASS: production_gov_rag.GovernmentRAG
PROVINCES: ['Karnataka']
EMBEDDINGS_DEVICE: HuggingFaceEmbeddings(device=mps)
EVENTS:
  HuggingFaceEmbeddings(device=mps)
  Chroma(existing_store=True)
```

The actual PR production constructor selected the MPS fallback and passed that resolved device into the actual embeddings construction. The baseline constructor selected CPU under the same CUDA-unavailable/MPS-available environment.

## Verification boundary

This establishes the production `GovernmentRAG.__init__` device-resolution path and selected-province/persisted-store branch under a controlled import-stubbed environment. It does not establish real MPS execution, embedding-model compatibility, Chroma retrieval quality, document ingestion, API behavior, or the full Government RAG benchmark. The #536/#764 conflict and province-filter preservation remain separate merge-resolution requirements.

## Copy-ready Task 2 update

> **Production-path follow-up for PR #536.** I imported the actual `GovernmentRAG` production class from `main@37a9c60` and PR #536 at `d2d062d`, then exercised the real constructor with a minimal province fixture and an existing persisted-store branch. Under a controlled environment with CUDA unavailable and MPS available, `main` passed `device=cpu` to the real embeddings constructor, while #536's real `default_device()` path passed `device=mps`. This is bounded production-constructor evidence with import-boundary stubs; it is not a claim of real MPS execution, model download, retrieval quality, or full Government RAG success.
