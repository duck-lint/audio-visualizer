# AGENTS.md

## Project Purpose

This repository implements a browser-first mathematical relation explorer for audible signals.

It is not a DAW.

Primary goal:

`generate signal → hear signal → observe actual samples → apply explicit relation → render geometry`

---

## Project Authority Chain

The project preserves this explicit provenance chain:

```text
Lean ideal model
        ↓
explicit sampling / discretization bridge
        ↓
Python numerical reference
        ↓
runtime contract + tests
        ↓
TypeScript / Web Audio implementation
        ↓
AudioWorklet sample tap
        ↓
p5.js visualization
```

All decisions should first defer to the above auth models and their relevant/associated sources and documentation before bringing a human into the loop.

### Lean

Licenses claims about the **ideal mathematical object**.

### Python

Predicts finite-sample behaviour and quantifies numerical approximation/error under explicit discretization assumptions.

### TypeScript / Web Audio

Produces the **actual runtime signal**.

Agreement with Python must be established through runtime contracts and tests, not assumed from equivalent formulas.

### AudioWorklet

Observes concrete runtime samples and preserves their audio-timeline identity.

### p5.js

Renders geometry of the **ideal mathematical object**, derived from observed samples.

It has no authority to infer or regenerate what the signal should look like.

The arrows are explicit bridges, not equality claims.

---

## Repository Document Precedence

When repository instructions conflict, use:

1. `harness/project-spec/project-spec.md`
2. `harness/project-spec/authority.md`
3. `harness/project-spec/decision-register.md`
4. `harness/project-spec/mvp-implementation-plan.md`
5. `harness/canon/*`
6. tests and accepted runtime contracts
7. existing implementation
8. local conventions and agent inference

This is **instruction precedence**, not the mathematical/runtime authority chain above.

Existing code does not override the harness.

If implementation conflicts with a locked decision, surface the conflict rather than silently preserving existing behavior.

---

## Critical Rules

* The visualizer must derive geometry from actual runtime samples.
* p5 must not independently recreate what the signal is expected to look like.
* Audio time is authoritative over UI/render time.
* Do not hardcode sample rate.
* Do not assume AudioWorklet blocks are permanently 128 samples.
* Integer sample delay is the MVP primitive.
* Do not silently approximate fractional delay.
* Keep signal transformations separate from representational transformations.
* Preserve distinctions between ideal, numerical-reference, runtime-observed, and rendered results.
* Do not broaden Lean formalization merely because a mathematical definition exists.
* Do not add infrastructure for hypothetical future complexity.

---

## Scope Control

Explicitly deferred unless evidence justifies promotion:

* MIDI
* SharedArrayBuffer
* WebGL/WebGPU
* WASM DSP
* arbitrary node graph
* desktop packaging
* presets/session system
* DAW functionality

Do not implement deferred features opportunistically.

---

## Change Classification

Before modifying behavior, classify the change as:

* implementation defect
* conformance repair
* mechanical cleanup
* new design choice
* scope expansion

Do not disguise a new design choice as cleanup.

New design choices affecting locked authority boundaries require an explicit harness update.

---

## Testing Expectations

Tests must state what they establish.

* Lean theorem → ideal mathematical relation
* Python fixture → numerical reference behaviour
* `OfflineAudioContext` / runtime test → Web Audio implementation behaviour
* UI test → application/render behaviour

One evidence class must not be described as proving another.

---
