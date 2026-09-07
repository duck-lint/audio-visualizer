# AGENTS.md

## Project

Audio Visualizer is a browser-first mathematical relation explorer for audible signals.

It is not a DAW.

Primary product relation:

`generate signal → hear signal → observe runtime samples → apply explicit relation → render geometry`

## Project Authority

This repository contains **project-specific specification and authority only**. Workflow, orchestration, escalation, and general engineering operating instructions are external to this harness.

Authoritative project documents:

* `harness/project-spec/project-spec.md` — required product behavior, MVP boundary, stack, and runtime constraints.
* `harness/project-spec/authority.md` — canonical mathematical/runtime authority chain, provenance classes, bridge rules, and semantic boundaries.
* `harness/project-spec/decision-register.md` — locked, deferred, and explicitly open project decisions.
* `harness/project-spec/mvp-implementation-plan.md` — project-specific MVP dependency order and acceptance gates.

These documents have distinct scopes.

`mvp-implementation-plan.md` may not redefine the project specification, authority model, or locked decisions.

A contradiction among the specification, authority model, and locked decisions is a harness defect; do not resolve it by treating implementation, tests, or inference as higher project authority.

Existing implementation does not override the harness.
