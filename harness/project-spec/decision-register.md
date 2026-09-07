# Audio Visualizer — Decision Register

Status values:

* **LOCKED** — project harness authority
* **DEFERRED** — explicitly outside current scope
* **OPEN** — requires evidence or implementation experience

| Decision                       | Choice                                    | Status         |
| ------------------------------ | ----------------------------------------- | -------------- |
| Product identity               | Mathematical relation instrument          | LOCKED           |
| DAW functionality              | Out of scope                              | LOCKED           |
| Runtime                        | Browser-first                             | LOCKED           |
| Main language                  | TypeScript                                | LOCKED           |
| Formal layer                   | Lean + mathlib                            | LOCKED           |
| Formal scope                   | Small stable kernel only                  | LOCKED           |
| Numerical layer                | Python reference + fixtures               | LOCKED           |
| Audio engine                   | Web Audio API                             | LOCKED           |
| Sample observation             | AudioWorklet                              | LOCKED           |
| Visual source                  | Actual runtime samples                    | LOCKED           |
| Renderer                       | p5.js P2D                                 | LOCKED           |
| p5 responsibility              | Visualization only                        | LOCKED           |
| Node.js responsibility         | Tooling/build/tests only                  | LOCKED           |
| Build system                   | Vite                                      | LOCKED           |
| Sample rate                    | Runtime `AudioContext.sampleRate`         | LOCKED           |
| Delay primitive                | Integer sample count                      | LOCKED           |
| Delay ms/phase                 | Derived values                            | LOCKED           |
| Fractional delay               | Separate future transform                 | DEFERRED          |
| Worklet quantum                | Variable-length handling                  | LOCKED           |
| Audio/render relationship      | Audio timeline authoritative              | LOCKED           |
| Main-thread visual backlog     | Drop stale visual data                    | LOCKED           |
| SharedArrayBuffer              | Excluded from MVP                         | DEFERRED          |
| Audio startup                  | Explicit user gesture                     | LOCKED           |
| Built-in waveform status       | Runtime-observed, not ideal-by-definition | LOCKED           |
| Effects                        | Biquad + waveshaper                       | LOCKED           |
| Signal-vs-delayed-self         | MVP                                       | LOCKED           |
| A-vs-B projection              | MVP                                       | LOCKED           |
| Dry-vs-processed projection    | MVP                                       | LOCKED           |
| OfflineAudioContext testing    | Required                                  | LOCKED           |
| Production worklet smoke test  | Required                                  | LOCKED           |
| p5 dependency                  | Exact tested version pin                  | LOCKED           |
| MIDI                           | Post-MVP                                  | DEFERRED          |
| WebGL/WebGPU                   | Performance-triggered only                | DEFERRED          |
| Arbitrary signal graph         | Post-MVP exploration                      | DEFERRED          |
| Desktop packaging              | Demand-triggered                          | DEFERRED          |
| Presets/session persistence    | Post-MVP                                  | DEFERRED          |
| Lean phase representation      | Raw radians vs `Real.Angle`               | OPEN           |
| Browser support contract       | Current stable desktop Chromium   | LOCKED           |
| Extra browser support contracts   | Firefox/Safari support   | DEFERRED         |
| Fractional-delay interpolation | Method and guarantees                     | OPEN after MVP |
| GPU renderer migration         | Only after measured P2D bottleneck        | OPEN after MVP |
| Worklet transport       | Batched MessagePort + transferable buffers | LOCKED |
| Observation identity    | Named stream + audio-timeline sample index  | LOCKED |
| Cross-stream pairing    | Pair by audio sample identity, never arrival/render time | LOCKED |

## Decision Rule

A deferred feature may be promoted only when there is evidence that the current design blocks a project goal.

Do not add infrastructure based only on anticipated future complexity.
