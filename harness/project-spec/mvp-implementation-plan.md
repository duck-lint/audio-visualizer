# Audio Visualizer — Implementation Plan

## Goal

Reach MVP through dependency-ordered slices that preserve the project authority chain:

`ideal math → numerical reference → runtime signal → observed samples → rendered geometry`

The ideal/numerical foundation is accepted before runtime feature implementation.

---

## Foundation Gate — Ideal and Numerical Substrate

### Deliver

* Lean + mathlib project setup;
* Python numerical reference area;
* Lean definitions for continuous signal, sine, gain, mixing, causal delay, and delayed-self projection;
* proofs required by the formal-kernel contract in `project-spec.md`;
* Python numerical counterparts for the canonical sine/delay cases;
* small interactive Python development witness.

### Acceptance

* Lean project checks successfully;
* zero-delay identity and delay composition are proved;
* zero-, half-, and quarter-cycle sine projection relations are proved;
* the Python numerical reference exposes explicit sample rate/discretization assumptions;
* the development witness consumes the Python reference rather than duplicating signal mathematics;
* the witness visibly demonstrates the zero-, quarter-, and half-cycle cases and reports numerical error where defined;
* ideal, numerical-reference, and human-inspection claims remain explicitly distinguishable.

### Stop condition

Do not begin TypeScript/Web Audio feature implementation until this foundation gate is accepted.

---

## Slice 1 — Runtime Repository Setup

### Deliver

* TypeScript + Vite application shell;
* exact tested p5.js dependency pin;
* basic TypeScript test command;
* production build command.

### Acceptance

* app builds;
* TypeScript tests run;
* production Vite build succeeds.

---

## Slice 2 — First Complete Runtime Signal Path

Implement one sine oscillator:

```text
OscillatorNode
    ↓
master gain
    ├→ audio output
    └→ AudioWorklet tap
            ↓
       indexed sample buffer
            ↓
      causal delayed-self XY
            ↓
           p5
```

Controls:

* frequency;
* amplitude;
* nonnegative integer delay samples;
* audio start/stop.

Readouts:

* runtime sample rate;
* delay samples;
* derived delay milliseconds;
* derived phase lag at the selected reference frequency.

### Acceptance

* signal is audible;
* actual runtime samples reach the visualizer;
* changing frequency changes sound and geometry;
* changing delay changes geometry;
* delayed-self uses `y[n] = s[n-k]` for `k ≥ 0`;
* renderer does not synthesize its own sine trajectory;
* production build loads the worklet correctly.

### Stop condition

Do not add more oscillators until runtime sample provenance is demonstrably correct.

---

## Slice 3 — Numerical-to-Runtime Reference Case

Use the accepted Lean/Python foundation as upstream authority.

### Runtime

Render controlled sine cases with Web Audio / `OfflineAudioContext` and compare them against the Python numerical reference.

### Acceptance

The project can distinguish and trace:

* the ideal Lean relation;
* the finite Python numerical approximation;
* the observed Web Audio result.

Agreement is measured through explicit tests; no layer is described as identical without evidence.

---

## Slice 4 — Oscillator Bank

Add three independently controllable oscillators.

Each supports:

* sine;
* triangle;
* square;
* saw;
* frequency;
* amplitude;
* initial phase;
* mute.

Add mixer and master gain.

### Acceptance

* oscillators operate independently;
* mixed output is audible;
* visualization derives from mixed runtime samples;
* oscillator state changes do not introduce avoidable uncontrolled clicks.

---

## Slice 5 — Projection Modes

Add:

1. signal vs causal delayed self;
2. oscillator A vs oscillator B;
3. dry vs processed.

Projection logic remains separate from rendering.

### Acceptance

Each projection has an explicit mathematical definition and identifiable runtime inputs.

Multi-stream projections pair named observed streams by audio-timeline sample identity.

p5 receives coordinates or sample-derived projection data only.

---

## Slice 6 — Effects

Add:

* one biquad filter;
* one waveshaper / clipping transform.

Classify operations correctly:

* waveshaping: pointwise;
* filtering: temporal/stateful.

### Acceptance

* dry vs processed comparison works;
* UI does not describe filter behavior as a pointwise mapping;
* effects remain observable through actual runtime samples.

---

## Slice 7 — Visual Instrument Controls

Add:

* trace persistence;
* zoom;
* freeze;
* derived phase readout;
* mathematical operation readout.

Representational controls remain separate from signal transformations.

### Acceptance

Changing zoom, persistence, or display settings never changes audio or projection semantics.

---

## Slice 8 — Cross-Layer Tests

Add:

* TypeScript unit tests;
* Python numerical fixtures;
* `OfflineAudioContext` browser tests;
* production worklet smoke test.

Test at least:

* zero-delay sine;
* quarter-cycle-equivalent sine lag;
* half-cycle-equivalent sine lag;
* mixed oscillator case;
* dry vs waveshaped signal.

### Acceptance

Each test states which layer it verifies.

Numerical fixtures are not labeled formal proofs.

Runtime tests are not treated as validation of the Lean model itself.

---

## Slice 9 — MVP Hardening

Verify:

* autoplay/startup state;
* variable AudioWorklet block lengths;
* runtime sample-rate propagation;
* stale visual packet dropping;
* no hidden 48 kHz assumptions;
* no hidden 128-frame assumptions;
* no dependency on `SharedArrayBuffer`;
* pinned p5 version;
* current stable desktop Chromium production build.

### MVP Completion

MVP is complete when the acceptance criteria in `harness/project-spec/project-spec.md` are satisfied.
