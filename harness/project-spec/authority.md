# Audio Visualizer — Authority and Provenance

## 1. Core Rule

**No representation may silently substitute itself for its source.**

Shared naming does not establish identity between mathematical, numerical, runtime, and rendered objects.

---

## 2. Authority Layers

```text
Lean ideal model
        ↓
explicit discretization / realization bridge
        ↓
Python numerical reference
        ↓
runtime contract + tests
        ↓
TypeScript / Web Audio implementation
        ↓
AudioWorklet observed samples
        ↓
p5 visualization
```

The arrows are bridges, not equality signs.

### Multi-stream runtime observation

A projection comparing multiple runtime signals must identify each observed
signal by source and pair samples on the shared audio timeline.

Message arrival time, UI time, and render-frame identity do not establish
sample correspondence.

Observation transport must preserve enough information to detect gaps and
establish sample identity.

---

## 3. Layer Responsibilities

### Lean — ideal mathematical authority

Lean may establish claims such as:

* signal definitions
* delay composition
* phase relationships
* projection definitions
* sine identities
* selected stable invariants

Lean does NOT establish correctness of:

* JavaScript
* Web Audio
* floating-point execution
* browser DSP
* p5 rendering

Formalization must remain deliberately small.

### Python — numerical reference authority

Python owns:

* sampling experiments
* discretization
* interpolation research
* numerical error measurement
* reference trajectories
* test fixture generation

Python results are numerical evidence, not formal proof.

### TypeScript / Web Audio — runtime authority

The running application owns the concrete signal actually generated.

Runtime behavior outranks an assumed ideal waveform when describing what the user is currently hearing.

### AudioWorklet — observation boundary

The worklet exposes actual indexed runtime samples.

Sample indices belong to the audio timeline.

UI/render frame numbers do not define signal time.

### p5.js — representational authority

p5 receives coordinates and renders them.

**Rendering is observational, not generative.**

p5 must not infer what a signal “should” look like and substitute synthetic geometry.

p5 renders a representation derived from runtime-observed samples after an explicit projection. It does not render or instantiate the ideal mathematical object.

---

## 4. Provenance Vocabulary

Use these terms consistently:

### `ideal`

Defined mathematically without finite-sampling assumptions.

### `numerical_reference`

Finite numerical realization produced under explicit assumptions.

### `runtime_observed`

Samples produced by the executing Web Audio graph.

### `rendered`

A visual representation derived from supplied coordinates.

Do not collapse these statuses.

---

## 5. Bridge Rules

### Ideal → numerical

Requires explicit assumptions such as:

* sample rate
* sampling rule
* finite precision
* integer/fractional delay strategy
* interpolation method

### Numerical → runtime

Agreement requires tests or measurements.

Equivalent formulas alone do not establish identical runtime behavior.

### Runtime → rendered

Rendered coordinates must retain traceable provenance to runtime samples and the projection applied to them.

---

## 6. Transformation Categories

Every signal operation must be classified as one of:

### Pointwise

Depends only on the current sample.

Examples:

* gain
* clipping
* waveshaping

### Temporal / stateful

Depends on signal history or future displacement.

Examples:

* delay
* filters
* integration
* differentiation approximations

### Mixing

Combines multiple signals.

### Projection

Maps one or more signals into coordinates.

### Representational

Changes only presentation.

Examples:

* zoom
* line thickness
* trace persistence
* display rotation

Representational operations must not be described as signal transformations.

---

## 7. Ambiguous Terms

Do not expose overloaded controls without qualification.

In particular, distinguish:

* oscillator initial phase
* X/Y delay phase
* filter phase response
* display rotation

There is no generic authoritative `phase` control.

A derived delay phase is meaningful only relative to a declared reference frequency. It is not a global phase property of a multi-frequency signal.

---

## 8. Formalization Admission Rule

An operation or theorem enters Lean only when:

1. its mathematical meaning is stable;
2. the relationship matters beyond a temporary experiment;
3. protecting that relationship provides real value.

Experimental ideas may exist solely in Python or TypeScript.

**Proof is not a prerequisite for experimentation.**

---

## 9. Failure Policy

When layers disagree:

1. preserve the disagreement;
2. identify the first bridge at which divergence appears;
3. measure before modifying;
4. do not change the ideal model merely to match implementation behavior;
5. do not change runtime behavior merely to match an assumption unless the specification requires it.

Unexpected runtime behavior is evidence to investigate, not something to normalize away.
