# Sequential process-global device-state probe

**Executed:** 2026-08-25  
**Baseline:** `main@37a9c60a9747af0cfe3170f84249bc349c56e8d5`  
**Corrective head:** PR #767 at `00ddb06dbfb679dc3800059dd7730720c5a50e77`

## Purpose

`CUDA_VISIBLE_DEVICES` is process-global. This probe runs the exact production constructor methods three times in one interpreter, without resetting the variable between cases:

1. `use_gpu=False`;
2. `use_gpu=True`;
3. `use_gpu` omitted.

The initial inherited value is `'7'`. The probe records the value before each constructor, every environment write, module-load order, and the value after each constructor.

## Output

```text
REVISION: 37a9c60a9747af0cfe3170f84249bc349c56e8d5
INITIAL: 7
false: before='7'; events=["LOAD module_type='basemodel'"]; after='7'
true: before='7'; events=["LOAD module_type='basemodel'", "SET '0'"]; after='0'
omitted: before='0'; events=["LOAD module_type='basemodel'", "SET '0'"]; after='0'

REVISION: 00ddb06dbfb679dc3800059dd7730720c5a50e77
INITIAL: 7
false: before='7'; events=["SET '-1'", "LOAD module_type='basemodel'"]; after='-1'
true: before='-1'; events=["SET '0'", "LOAD module_type='basemodel'"]; after='0'
omitted: before='0'; events=["LOAD module_type='basemodel'"]; after='0'

BOUNDARY: sequential constructor/event-order behavior only; no real CUDA/MPS hardware or end-to-end benchmark.
```

## Finding

On `main`, explicit `false` leaves the inherited value unchanged and module loading occurs before the `true`/omitted visibility write. On PR #767, each explicit request is applied before module loading, while omitted preserves the inherited value without performing a new write.

This demonstrates the process-global ordering/state contract. It does not claim real CUDA/MPS enumeration or end-to-end model execution.

## Copy-ready Task 4 addition

> **Sequential process-global state regression.** Because `CUDA_VISIBLE_DEVICES` is process-global, I ran the exact production constructor three times in one interpreter with inherited value `'7'`: `use_gpu=False`, `use_gpu=True`, then omitted. On `main@37a9c60`, `false` left `'7'` unchanged and the `true`/omitted cases loaded the module before writing visibility. On PR #767 at `00ddb06`, `false` wrote `'-1'` before load, `true` wrote `'0'` before load, and omitted preserved the inherited `'0'` without a new write. This is sequential constructor/event-order evidence only; real CUDA/MPS hardware and end-to-end execution remain outside the boundary.
