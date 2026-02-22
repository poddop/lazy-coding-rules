# Coding Rules

## Engineering Governance
- `GOV1` Behavior preservation: prioritize preserving behavior and preventing regressions over structural refactoring.
- `GOV2` YAGNI: do not introduce abstractions for hypothetical future use.
- `GOV3` Conflict priority: 1) SSOT / Single Writer / One-way Data Flow / FSM / Immutability 2) Clean Architecture / CQRS / Single Gateway / Normalized State / Declarative Style 3) DRY (only within boundaries that do not break higher-priority principles).
- `GOV4` N/A handling and DRY guardrail: exclude N/A principles from evaluation, and apply DRY with Rule of Three.
- `GOV5` Root-cause evaluation: in each refactor cycle, evaluate at least one root-cause option; if deferred, explicitly record the trigger or condition for follow-up.
- `GOV6` Guard coverage: for refactors/feature changes, for any touched enforceable principles, add/update the lowest-cost tests/static checks whenever practical (see `FB13`); violations must fail; grow coverage in priority order (1->2->3), starting with low-cost checks (dependency direction, forbidden imports, fixed construction sites, etc.), and expand only when recurring regressions justify additional coverage.

## Architecture & State Principles
1. `ARCH3` SSOT: One source of truth per feature.
2. `ARCH4` Single Writer: Each state slice/aggregate has exactly one write authority. All other modules request changes via explicit commands/events; direct writes are prohibited.
3. `ARCH6` One-way Data Flow: Unidirectional flow.
4. `ARCH9` FSM (N/A if no async lifecycle/guards): Explicit states/transitions.
5. `ARCH8` Immutability: Immutable updates.
6. `ARCH1` Clean Architecture: Inward dependencies, separation, testability.
7. `ARCH2` CQRS: Separate read/write paths; optimize reads if needed. Read-model updates (projections/caches) are derived from the single write authority and must not introduce an additional writer.
8. `ARCH5` Single Gateway (N/A if no external I/O): One gateway per I/O boundary.
9. `ARCH7` Normalized State (N/A if no entity collections): IDs + references.
10. `ARCH10` Declarative Style (N/A if not state-driven UI): Declarative composition.
11. `ARCH11` DRY: No duplicated responsibility logic (Rule of Three).

## File Boundary And Size Policy
- `FB1` Responsibility bundle rule: divide modules/files by responsibility bundles (cohesive responsibilities that change together), not by file size alone.
- `FB2` Boundary-first: responsibility bundles must respect architectural boundaries (writer/gateway/flow/dependency direction).
- `FB3` Allowed scope per file: one primary responsibility bundle plus up to two tightly coupled sub-responsibilities.
- `FB4` Do not enforce strict `1 responsibility = 1 file`; avoid over-splitting.

## Split Triggers
- `FB5` Mandatory split when different I/O boundaries coexist in one file. (Trigger class: `mandatory`)
- `FB6` Mandatory split when orchestration logic and low-level gateway details are mixed in one file. (Trigger class: `mandatory`)
- `FB7` Strong split recommendation at `>= 25 KB`, unless a boundary reason justifies keeping it together. (Trigger class: `recommended`)
- `FB8` Review threshold at `>= 20 KB` (not mandatory by itself). (Trigger class: `none`)
- `FB9` Recommend split when at least two of the following are true. (Trigger class: `recommended`)
  - file size `> 20 KB`
  - file length `> 500` lines
  - `>= 4` exported/public command entry points
  - repeated multi-owner churn in the same file area

## Over-splitting Guardrails
- `FB10` Avoid very thin files (`< 2 KB`) unless the file is `mod.rs`, type-only, or a required boundary adapter.
- `FB11` Do not split in ways that add write authorities or bypass existing gateways.

## Refactor Safety
- `FB12` Preserve external command names and behavior when splitting.
- `FB13` Add or update guard tests/static checks for touched principles whenever practical.
