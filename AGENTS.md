# AGENTS.md

## Objective
Minimize unnecessary refactoring while enforcing architecture and UI/state-management soundness in all code generation; when risk is low to medium, prefer root-cause fixes over containment-only changes.

## Required Rules (Always Apply)
- Behavior preservation: prioritize preserving behavior and preventing regressions over structural refactoring.
- YAGNI: do not introduce abstractions for hypothetical future use.
- Conflict priority: 1) SSOT / Single Writer / One-way Data Flow / FSM / Immutability 2) Clean Architecture / CQRS / Single Gateway / Normalized State / Declarative Style 3) DRY (only within boundaries that do not break higher-priority principles).
- Module/file boundaries: split by responsibility bundles (cohesive responsibilities that change together), not by size alone; follow `rules/coding-rules.md` `FB*` rules.
- N/A handling and DRY guardrail: exclude N/A principles from evaluation, and apply DRY with Rule of Three.
- In each refactor cycle, evaluate at least one root-cause option; if deferred, explicitly record the trigger or condition for follow-up.
- For principles touched by the scoped change that can be mechanically enforced (SSOT/Single Writer/Flow/FSM/Immutability/Clean/CQRS/Gateway/Normalized/Declarative/DRY), codify low-cost tests or static checks whenever practical, and fail when violated.
- For refactors or feature changes, add/update guard tests that correspond to the principle(s) you changed (never change behavior; strictly follow YAGNI).
- Grow guard coverage in priority order (1->2->3), starting with low-cost checks (dependency direction, forbidden imports, fixed construction sites, etc.), and expand only when recurring regressions justify additional coverage.

## Index (Detailed Rules)
- `rules/coding-rules.md`: Architecture & State Principles + File Boundary Rules (`ARCH*`, `FB*`).
