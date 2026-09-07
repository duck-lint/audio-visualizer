# Audio Visualizer — Implementation Plan

## Goal

Reach MVP through vertical slices that preserve the project’s authority model:

`ideal math → numerical reference → runtime signal → observed samples → rendered geometry`

Do not build broad infrastructure ahead of demonstrated need.

---

## Slice 0 — Repository Setup

### Deliver

* TypeScript + Vite application shell
* Python reference area
* Lean project / mathlib setup
* basic test commands

### Acceptance

* app builds
* TypeScript tests run
* Python reference script runs
* Lean project checks
* production Vite build succeeds

### Stop condition

Do not begin feature work until all three environments are independently runnable.

---

## Slice 1 — First Complete Signal Path

Implement one sine oscillator:

```text
OscillatorNode
    ↓
master gain
    ├→ audio output
    └→ AudioWorklet tap
            ↓
       sample buffer
            ↓
      delayed-self XY
            ↓
           p5
```

Controls:

* frequency
* amplitude
* integer delay samples
* audio start/stop

Readouts:

* runtime sample rate
* delay samples
* derived delay milliseconds

### Acceptance

* signal is audible
* actual runtime samples reach the visualizer
* changing frequency changes sound and geometry
* changing delay changes geometry
* renderer does not synthesize its own sine trajectory
* production build loads the worklet correctly

### Stop condition

Do not add more oscillators until runtime sample provenance is demonstrably correct.

---

## Slice 2 — Quarter-Period Reference Case

### Lean

Define the minimal ideal objects required for:

* signal delay
* delayed-self projection
* sine quarter-period relation

Prove the selected quarter-period sine identity.

### Python

Generate the equivalent finite-sample case for explicit:

* frequency
* sample rate
* integer delay

Measure deviation from the ideal circle.

### Runtime

Add a test/reference configuration reproducing the same case.

### Acceptance

The application can distinguish:

* ideal circular relation
* finite numerical approximation
* observed Web Audio result

No layer is described as identical without evidence.

---

## Slice 3 — Oscillator Bank

Add three independently controllable oscillators.

Each supports:

* sine
* triangle
* square
* saw
* frequency
* amplitude
* initial phase
* mute

Add mixer and master gain.

### Acceptance

* oscillators operate independently
* mixed output is audible
* visualization derives from mixed runtime samples
* oscillator state changes do not introduce avoidable uncontrolled clicks

---

## Slice 4 — Projection Modes

Add:

1. signal vs delayed self
2. oscillator A vs oscillator B
3. dry vs processed

Projection logic must remain separate from rendering.

### Acceptance

Each projection has an explicit mathematical definition and identifiable runtime inputs.

p5 receives coordinates or sample-derived projection data only.

---

## Slice 5 — Effects

Add:

* one biquad filter
* one waveshaper / clipping transform

Classify operations correctly:

* waveshaping: pointwise
* filtering: temporal/stateful

### Acceptance

* dry vs processed comparison works
* UI does not describe filter behavior as a pointwise mapping
* effects remain observable through actual runtime samples

---

## Slice 6 — Visual Instrument Controls

Add:

* trace persistence
* zoom
* freeze
* derived phase readout
* mathematical operation readout

Representational controls must remain separate from signal transformations.

### Acceptance

Changing zoom, persistence, or display settings never changes audio or projection semantics.

---

## Slice 7 — Cross-Layer Tests

Add:

* TypeScript unit tests
* Python numerical fixtures
* `OfflineAudioContext` browser tests
* production worklet smoke test

Test at least:

* zero-delay sine
* quarter-period-equivalent sine
* half-period-equivalent sine
* mixed oscillator case
* dry vs waveshaped signal

### Acceptance

Each test states which layer it verifies.

Numerical fixtures are not labeled formal proofs.

Runtime tests are not treated as validation of the Lean model itself.

---

## Slice 8 — MVP Hardening

Verify:

* autoplay/startup state
* variable AudioWorklet block lengths
* runtime sample-rate propagation
* stale visual packet dropping
* no hidden 48 kHz assumptions
* no hidden 128-frame assumptions
* no dependency on `SharedArrayBuffer`
* pinned p5 version
* browser production build

### MVP Completion

MVP is complete when the acceptance criteria in `SPEC.md` are satisfied.

---

## Development Rules

### Prefer vertical completion

Finish:

`signal → observation → projection → rendering → test`

before expanding horizontally.

### Measure before adding infrastructure

Do not introduce:

* SharedArrayBuffer
* WebGL/WebGPU
* WASM
* desktop packaging
* generalized node graphs

without evidence that the current implementation blocks a required goal.

### Preserve disagreement

When Lean, Python, or runtime observations differ, locate the bridge where divergence enters before changing code or definitions.

### Formalize selectively

Lean follows stable mathematical meaning.

Experimental transformations may exist in Python or TypeScript without formalization.

### Keep provenance visible

Every important mathematical or numerical claim should make clear whether it is:

* ideal
* numerical reference
* runtime observed
* rendered
