# AI Coding Rules Starter

A practical AI coding ruleset for “lazy vibe coders”

## What is this?

This is a starter ruleset designed to help an AI produce code with **sound architecture and consistent design decisions**.

It is primarily intended for **Codex** usage, where rules are loaded at runtime from a project directory.

## What’s included

- `AGENTS.md`  
  Concrete design rules (e.g., `ARCH*`, `FB*`)

## Rules (Principles)

1. **Clean Architecture**  
   Keep dependencies pointing inward.

2. **CQRS**  
   Separate reads and writes.

3. **SSOT (Single Source of Truth)**  
   Keep authoritative data in one place to prevent inconsistencies.

4. **Single Writer**  
   Allow data mutation in only one place to prevent conflicts.

5. **Single Gateway**  
   Centralize the entry point to data access.

6. **One-way Data Flow**  
   Ensure data flows in a single direction.

7. **Normalized State**  
   Do not store the same data in multiple places; reference by ID.

8. **Immutability**  
   Do not overwrite existing data; create new data for changes when needed.

9. **FSM (Finite State Machine)**  
   Define allowed state transitions upfront to avoid unexpected state changes.

10. **Declarative Style**  
   Describe *what the system should be*, not *how to do it*.

11. **DRY (Don’t Repeat Yourself)**  
   Avoid duplication in code and logic.

## How to use

Place the following two files in your project’s `.codex` directory:

- `AGENTS.md`

Once present, Codex will load these rules at runtime.

Example structure:

```txt
your-project/
  .codex/
    AGENTS.md
