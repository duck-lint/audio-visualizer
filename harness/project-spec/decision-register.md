# Audio Visualizer — Decision Register

Status values:

* **LOCK** — project harness authority
* **DEFER** — explicitly outside current scope
* **OPEN** — requires evidence or implementation experience

| Decision                       | Choice                                    | Status         |
| ------------------------------ | ----------------------------------------- | -------------- |
| Product identity               | Mathematical relation instrument          | LOCK           |
| DAW functionality              | Out of scope                              | LOCK           |
| Runtime                        | Browser-first                             | LOCK           |
| Main language                  | TypeScript                                | LOCK           |
| Formal layer                   | Lean + mathlib                            | LOCK           |
| Formal scope                   | Small stable kernel only                  | LOCK           |
| Numerical layer                | Python reference + fixtures               | LOCK           |
| Audio engine                   | Web Audio API                             | LOCK           |
| Sample observation             | AudioWorklet                              | LOCK           |
| Visual source                  | Actual runtime samples                    | LOCK           |
| Renderer                       | p5.js P2D                                 | LOCK           |
| p5 responsibility              | Visualization only                        | LOCK           |
| Node.js responsibility         | Tooling/build/tests only                  | LOCK           |
| Build system                   | Vite                                      | LOCK           |
| Sample rate                    | Runtime `AudioContext.sampleRate`         | LOCK           |
| Delay primitive                | Integer sample count                      | LOCK           |
| Delay ms/phase                 | Derived values                            | LOCK           |
| Fractional delay               | Separate future transform                 | DEFER          |
| Worklet quantum                | Variable-length handling                  | LOCK           |
| Audio/render relationship      | Audio timeline authoritative              | LOCK           |
| Main-thread visual backlog     | Drop stale visual data                    | LOCK           |
| SharedArrayBuffer              | Excluded from MVP                         | DEFER          |
| Audio startup                  | Explicit user gesture                     | LOCK           |
| Built-in waveform status       | Runtime-observed, not ideal-by-definition | LOCK           |
| Effects                        | Biquad + waveshaper                       | LOCK           |
| Signal-vs-delayed-self         | MVP                                       | LOCK           |
| A-vs-B projection              | MVP                                       | LOCK           |
| Dry-vs-processed projection    | MVP                                       | LOCK           |
| OfflineAudioContext testing    | Required                                  | LOCK           |
| Production worklet smoke test  | Required                                  | LOCK           |
| p5 dependency                  | Exact tested version pin                  | LOCK           |
| MIDI                           | Post-MVP                                  | DEFER          |
| WebGL/WebGPU                   | Performance-triggered only                | DEFER          |
| Arbitrary signal graph         | Post-MVP exploration                      | DEFER          |
| Desktop packaging              | Demand-triggered                          | DEFER          |
| Presets/session persistence    | Post-MVP                                  | DEFER          |
| Lean phase representation      | Raw radians vs `Real.Angle`               | OPEN           |
| Browser support contract       | Exact evergreen-browser acceptance set    | OPEN           |
| Fractional-delay interpolation | Method and guarantees                     | OPEN after MVP |
| GPU renderer migration         | Only after measured P2D bottleneck        | OPEN after MVP |

## Decision Rule

A deferred feature may be promoted only when there is evidence that the current design blocks a project goal.

Do not add infrastructure based only on anticipated future complexity.
