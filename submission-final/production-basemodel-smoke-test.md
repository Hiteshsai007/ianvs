# PR #524 production `BaseModel` smoke test

**Executed:** 2026-08-25  
**Repository:** `kubeedge/ianvs`  
**Baseline:** `main@37a9c60a9747af0cfe3170f84249bc349c56e8d5`  
**PR head:** `#524@2d8343424dca334b88621ddffc7e811b818fd757`

## Purpose

This is a bounded production-path check for the existing Task 2 finding that PR #524's 41-test suite copies helper logic instead of importing production `BaseModel`.

The test imported the actual `examples/MOT17/multiedge_inference_bench/pedestrian_tracking/testalgorithms/reid/m3l/basemodel.py` from each pinned revision. It instantiated the real `BaseModel` class and called its real `load()` method with a temporary `model_resnet.pth` fixture. Import-only stubs supplied unavailable `numpy`, `torch`, `sedna`, `reid`, and `loguru` boundaries. The test did not copy `_get_device()`, `load()`, or any other production helper into the test.

The fake model records the production call made to it. The fake `torch.load()` returns an empty state dictionary so checkpoint parsing can reach the production device-transfer line without downloading a model.

## Baseline result — `main@37a9c60`

```text
SOURCE: /tmp/ianvs-main-smoke/examples/MOT17/multiedge_inference_bench/pedestrian_tracking/testalgorithms/reid/m3l/basemodel.py
CLASS: production_basemodel.BaseModel
DEVICE: <not defined>
LOAD_RESULT: AttributeError 'ProductionModel' object has no attribute 'cuda'
EVENTS:
  numpy.seed(1)
  torch.manual_seed(1)
  models.create(resnet)
```

The real baseline production `BaseModel.load()` calls `.cuda()` unconditionally. The intentionally CPU-only fake model has no `.cuda()` method, so the failure is raised at the production call site.

## PR #524 result — `2d834342`

```text
SOURCE: /tmp/ianvs-pr524/examples/MOT17/multiedge_inference_bench/pedestrian_tracking/testalgorithms/reid/m3l/basemodel.py
CLASS: production_basemodel.BaseModel
DEVICE: Device('cpu')
LOAD_RESULT: success
EVENTS:
  numpy.seed(1)
  torch.manual_seed(1)
  models.create(resnet)
  torch.load(cpu)
  logger.info(=> Loaded checkpoint '/tmp/tmpeqsckxb4/model_resnet.pth')
  production_model.load_state_dict
  production_model.to(cpu)
```

The PR-head production class selects CPU when CUDA and MPS are unavailable, loads the checkpoint through the real `load()` method, and transfers the model with `.to(cpu)`. No copied test helper was involved.

## Verification boundary

This establishes the production constructor/load device behavior at the pinned PR head under an import-stubbed, CPU-only harness. It does not establish real CUDA/MPS behavior, real checkpoint compatibility, model accuracy, the full MOT17 pipeline, `predict()` execution, multiprocessing, or end-to-end benchmark restoration. The empty checkpoint state and fake model deliberately stop at the device-transfer contract.

## Copy-ready Task 2 update

> **Production-path follow-up for PR #524.** The original 41-test suite remains a copied-helper suite and is not production integration evidence. To test the production path separately, I imported the real `m3l/basemodel.py` from `main@37a9c60` and PR #524 at `2d834342`, instantiated the real `BaseModel`, and called its real `load()` method with a temporary checkpoint fixture. With unavailable dependencies stubbed only at import boundaries, `main` failed at the production `.cuda()` call (`AttributeError: 'ProductionModel' object has no attribute 'cuda'`), while PR #524 selected `Device('cpu')` and completed `production_model.to(cpu)` successfully. This is bounded CPU production-path evidence; it is not a claim of real CUDA/MPS, checkpoint, model-accuracy, or full MOT17 success.
