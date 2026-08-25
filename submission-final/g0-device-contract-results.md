# G0 red contract test — device ordering

**Executed:** 2026-08-25  
**Baseline:** `main@37a9c60a9747af0cfe3170f84249bc349c56e8d5`  
**Corrective head:** PR #767 at `00ddb06dbfb679dc3800059dd7730720c5a50e77`

## Contract under test

Before a production algorithm module is loaded:

- `use_gpu` omitted: leave inherited CUDA visibility unchanged;
- `use_gpu=True`: set `CUDA_VISIBLE_DEVICES='0'` before module load;
- `use_gpu=False`: set `CUDA_VISIBLE_DEVICES='-1'` before module load.

The test uses the exact production `ParadigmBase.__init__`, `_get_module_instances`, and `SingleTaskLearning.__init__` methods extracted from each pinned Git revision. It is a contract probe, not a reimplementation of the production logic.

## Result

```text
main: RED (expected contract failure) — 37a9c60a9747af0cfe3170f84249bc349c56e8d5 omitted: events=["LOAD module_type='basemodel'", "SET CUDA_VISIBLE_DEVICES='0'"]
pr767: GREEN (contract passes)
```

The baseline fails because it loads the module before applying visibility and treats omitted `use_gpu` as enabled. PR #767 satisfies all three expected cases in the contract probe.

## G0 gate status

**G0 is complete as a red/green contract gate:** baseline `main` fails for the selected device-ordering reason, and the pinned PR #767 head passes. No model, dataset, GPU, MPS, Sedna runtime, or external API was required.

## Cumulative gate rule for Task 4

After every restoration stage, rerun that stage's gate and every preceding gate. A later passing gate cannot mask a regression in constructor ordering, device semantics, production `BaseModel` integration, or the #536/#764 reconciliation.

## Copy-ready Task 4 update

> **G0 red contract test.** Before implementation, I converted the omitted/true/false device-policy behavior into a contract gate using the exact production constructor/control-flow methods from `main@37a9c60` and PR #767 at `00ddb06`. `main` is intentionally **RED** for the selected root-problem reason: it loads the module before setting visibility and treats omitted `use_gpu` as enabled. PR #767 is **GREEN** for all three cases: omitted leaves visibility unchanged, `true` sets `'0'` before load, and `false` sets `'-1'` before load. After every later restoration stage, this G0 test and all earlier gates must be rerun before proceeding.
