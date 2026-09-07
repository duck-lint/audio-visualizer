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

   `y[n] = s[n+k]`

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

---

## 5. Delay Semantics

The MVP primitive is:

`delaySamples : integer`

with:

`delaySeconds = delaySamples / sampleRate`

and:

`phaseRadians = 2π × frequency × delaySeconds`

Fractional delay is NOT silently approximated.

If later introduced, fractional delay is a distinct interpolating transformation.

---

## 6. Research-Grounded Runtime Constraints

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

## 7. Testing

Use distinct evidence at each layer:

### Lean

Verify selected ideal mathematical relations.

### Python

Generate numerical reference cases and quantify discretization error.

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

## 8. MVP Acceptance Criteria

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
10. no DAW/session/timeline architecture is required.

---

## 9. Explicit Non-Goals for MVP

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
