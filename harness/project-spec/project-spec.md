# Audio Visualizer — Project Specification

## 1. Thesis Attractor

Build a **browser-first mathematical relation explorer whose primary objects are audible signals**.

The application exists to let a user:

1. generate a signal,
2. hear the signal,
3. transform or relate it mathematically,
4. see the resulting geometry,
5. inspect what operation produced that geometry.

This is **not a DAW**.

---

## 2. Locked Stack

| Layer               | Technology     | Responsibility                                                                   |
| ------------------- | -------------- | -------------------------------------------------------------------------------- |
| Formal math         | Lean + mathlib | Small stable mathematical kernel; selected definitions, invariants, and theorems |
| Numerical reference | Python         | Discretization experiments, error analysis, reference models, fixture generation |
| Application         | TypeScript     | Main application state and behavior                                              |
| Audio               | Web Audio API  | Runtime signal generation, mixing, DSP, scheduling                               |
| Sample access       | AudioWorklet   | Sample-level observation of the runtime audio signal                             |
| Visualization       | p5.js P2D      | Interactive rendering of supplied coordinates                                    |
| Tooling             | Node.js + Vite | Build, tests, scripts, development server                                        |

Node.js is not part of the signal path.

p5.js does not generate authoritative signal data.

---

## 3. MVP Signal Path

```text
Oscillators
    ↓
Mixer
    ↓
Effects
    ↓
Web Audio runtime
    ├────────────→ Audio output
    │
    └→ AudioWorklet sample tap
              ↓
       indexed sample buffer
              ↓
      mathematical projection
              ↓
            p5.js
```

The visualizer MUST operate on observed runtime samples rather than independently redrawing an ideal waveform formula.

---

## 4. MVP Features

### Oscillators

Three independently controllable oscillators.

Each supports:

* sine
* triangle
* square
* saw
* frequency
* amplitude
* initial phase
* mute

Initial phase is the source waveform's phase offset relative to its activation point on the audio timeline. It is distinct from projection delay phase, filter phase response, and display rotation.

### Signal processing

MVP includes:

* mixer
* master gain
* one biquad filter
* one waveshaper / clipping transform

### Visualization modes

MVP includes:

1. signal vs delayed self

   `x[n] = s[n]`

   `y[n] = D_k(s)[n] = s[n-k]`, for `k ≥ 0`

2. oscillator A vs oscillator B

3. dry signal vs processed signal

### Visualization controls

* delay in samples
* derived delay in milliseconds
* derived phase relative to a selected frequency
* trace persistence
* zoom
* freeze

### Explanatory readout

Where applicable, surface:

* runtime sample rate
* delay samples
* delay milliseconds
* phase relation
* mathematical operation
* ideal-model status
* runtime/numerical status

### Development witness

A small interactive Python numerical witness is a required development artifact.

It must:

* consume the Python numerical reference rather than duplicate signal mathematics;
* expose sine amplitude, frequency, source phase, and nonnegative lag;
* show source and delayed waveforms plus the XY projection;
* provide canonical zero-, quarter-, and half-cycle cases;
* show numerical error against an ideal relation when that relation is defined.

The witness is not product UI. It provides human-inspectable numerical evidence and does not establish formal or runtime truth.

---

## 5. Delay Semantics

Delay uses the causal lag convention.

Ideal continuous delay:

`D_τ(s)(t) = s(t - τ)`, for `τ ≥ 0`.

MVP discrete delay:

`D_k(s)[n] = s[n-k]`, for nonnegative integer `k`.

The delayed-self projection is:

`x[n] = s[n]`

`y[n] = D_k(s)[n]`

with:

`delaySeconds = k / sampleRate`

and lag magnitude at reference frequency `f`:

`phaseLagRadians = 2π × f × delaySeconds`

For a sine, Y's phase relative to X is `-phaseLagRadians`.

Fractional delay is NOT silently approximated.

If later introduced, fractional delay is a distinct interpolating transformation.

---

## 6. Formal Kernel Contract

Before runtime feature implementation, the Lean/mathlib foundation must define only the stable ideal objects required by the MVP:

* continuous signal: `Signal := ℝ → ℝ`;
* sinusoid with amplitude, frequency or angular frequency, and source phase;
* gain;
* signal mixing;
* causal delay `D_τ(s)(t) = s(t - τ)`;
* delayed-self projection `(s(t), D_τ(s)(t))`.

It must establish at minimum:

* zero-delay identity;
* delay composition;
* zero-lag sine projection: `y = x`;
* half-cycle sine projection: `y = -x`;
* quarter-cycle sine projection: `x² + y² = A²`.

Where practical, canonical sine theorems should state phase displacement as a condition such as `ωτ = π/2` rather than requiring division by frequency.

The foundation does NOT formalize sampling, floating-point execution, Python, Web Audio, browser behavior, p5 rendering, filters, or non-sinusoidal runtime oscillator realization.

---

## 7. Research-Grounded Runtime Constraints

The implementation MUST account for the following:

* `AudioContext.sampleRate` is runtime data; do not hardcode 48 kHz.
* `AudioWorkletProcessor.process()` buffer length must be inspected; do not assume a permanent 128-frame quantum.
* p5/render frames are not the audio clock.
* visualization may drop stale data; audio must never wait for rendering.
* browser autoplay restrictions require an explicit user action to start audio.
* worklet asset loading must be tested against the production Vite build, not only the dev server.
* SharedArrayBuffer is excluded from MVP because it introduces cross-origin-isolation deployment requirements.
* built-in Web Audio oscillator output is runtime behavior and must not be treated as identical to the ideal Lean waveform definition.
* audible speaker-time synchronization is not an MVP guarantee; the visualization represents the rendered audio signal.

---

## 8. Testing

Use distinct evidence at each layer:

### Lean

Verify selected ideal mathematical relations.

### Python

Generate numerical reference cases and quantify discretization error.

### Development witness

Provide a human-inspectable view of the Python numerical reference. Visual inspection is a sanity/comprehension check, not formal proof or runtime evidence.

### TypeScript

Unit-test application and projection logic.

### OfflineAudioContext

Render deterministic Web Audio graphs and compare actual browser output against numerical expectations.

### Browser smoke tests

Verify:

* audio starts
* worklet loads
* signal reaches speakers
* samples reach renderer
* production build behaves correctly

---

## 9. MVP Acceptance Criteria

MVP is achieved when:

1. a user can select sine, triangle, square, or saw;
2. frequency and amplitude changes are audible;
3. the displayed XY trajectory derives from the actual runtime signal;
4. changing sample delay visibly changes the trajectory;
5. sine + quarter-period-equivalent delay produces the expected near-circular numerical realization;
6. multiple oscillators can be mixed;
7. filter and waveshaper transformations can be observed;
8. ideal, numerical, and runtime claims remain distinguishable;
9. the application works from a production browser build;
10. no DAW/session/timeline architecture is required;
11. the required Lean kernel builds and its canonical sine/delay theorems are proved;
12. the development witness runs the canonical zero-, quarter-, and half-cycle numerical cases.

---

## 10. Explicit Non-Goals for MVP

Do not add:

* DAW timeline or tracks
* sequencer
* recording
* plugin hosting
* arbitrary node graph
* MIDI
* presets/accounts/cloud storage
* desktop shell
* SharedArrayBuffer
* WebGL/WebGPU rendering
* WASM DSP
* audio-file import
* mobile optimization
* exhaustive formal verification
