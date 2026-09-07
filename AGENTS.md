## Project Purpose

This repository implements a browser-first mathematical relation explorer for audible signals.

It is not a DAW.

Primary goal:

`generate signal → hear signal → observe actual samples → apply explicit relation → render geometry`

---

## Authority Hierarchy

When sources conflict, use this order:

1. `SPEC.md` — product scope and required behavior
2. `AUTHORITY.md` — semantic, mathematical, and provenance boundaries
3. `DECISIONS.md` — locked, deferred, and open design decisions
4. `IMPLEMENTATION_PLAN.md` — current implementation sequence
5. tests and accepted runtime contracts
6. existing implementation
7. local conventions and agent inference

Existing code does not override the harness.

If implementation conflicts with a locked decision, surface the conflict rather than silently preserving existing behavior.

---

## Mathematical Authority

Keep these layers distinct:

### Lean

Ideal mathematical definitions and selected verified relations.

### Python

Numerical reference implementation, discretization experiments, error analysis, and fixtures.

### TypeScript / Web Audio

Actual runtime implementation.

### AudioWorklet

Observation boundary for concrete runtime samples.

### p5.js

Rendering only.

Do not treat agreement between layers as automatic.

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
* Preserve distinctions between ideal, numerical, runtime-observed, and rendered results.
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

Before modifying behavior, identify the change as one of:

* implementation defect
* conformance repair
* mechanical cleanup
* new design choice
* scope expansion

Do not disguise a new design choice as cleanup.

New design choices that affect locked authority boundaries require an explicit harness update.

---

## Testing Expectations

Tests must state what they establish.

Examples:

* Lean theorem → ideal relation
* Python fixture → numerical reference
* `OfflineAudioContext` test → browser runtime behavior
* UI test → application/render behavior

One evidence class must not be described as proving another.

---

## Agent Working Style

Prefer:

* small vertical slices
* explicit interfaces
* deterministic tests
* descriptive names
* comments explaining why
* measured evidence before optimization

Avoid:

* speculative abstractions
* compatibility paths not required by the harness
* silent fallbacks
* duplicated mathematical semantics without provenance
* premature generalized frameworks

When uncertain, preserve the seam and surface the decision rather than collapsing it.
