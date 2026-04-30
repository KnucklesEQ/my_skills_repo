---
name: project-context-briefing
description: Create or update docs/project-context.md as a compact LLM-oriented briefing for brownfield projects
compatibility: opencode
metadata:
  domain: brownfield-analysis
  output: docs/project-context.md
---

## What I do

I create or update `docs/project-context.md` as a compact briefing for future LLM sessions.

Goal:
- reduce the need for full repo initialization
- capture project boundaries, terminology, constraints, and reliable implementation guidance
- optimize for future reasoning by an LLM, not for human-friendly narrative documentation

## Source priority

1. current codebase and active configuration
2. user-provided documentation
3. user answers to clarification questions
4. inference

## Source handling

- At the start, ask whether the user has additional project documentation not present in the repository.
- If the user says no, does not provide any, or asks to proceed, continue from the repository alone.
- Do not block the task waiting for external documentation.
- Examples: wiki pages, product docs, architecture notes, API docs, migration plans, domain glossaries.
- Treat user-provided docs as input, not ground truth.
- Reconcile documentation against the current codebase.
- Do not invent features, rules, flows, architecture, or domain semantics.
- Prefer explicit negatives and boundaries over broad summaries.

## Conflict resolution

- Current codebase and active configuration are the source of truth.
- If README, wiki, or other docs conflict with code, prefer code.
- Always mark the discrepancy explicitly as `Conflict:`.

## Evidence discipline

- Do not infer active architecture from dependencies, folder names, or package manifests alone.
- Prefer actual runtime wiring:
  - entrypoints
  - routes
  - imports
  - active config
  - env usage
  - infra manifests
  - tests
- Distinguish between code that exists and code that is actively wired into runtime.
- Do not equate repository presence with active behavior.
- A module, dependency, directory, or config file is only part of current system behavior if runtime wiring or active usage supports it.
- Do not state inferred behavior as confirmed.
- Use `Inferred:` only when multiple repository signals support the conclusion.

## High-impact claims

For claims about these topics, prefer `Confirmed:` or `Unknown:`:
- auth
- tenancy
- public API
- background jobs
- deployment/runtime model

Avoid `Inferred:` here unless multiple strong signals support the conclusion.

## When to ask questions

### If `docs/project-context.md` does not exist

1. Ask whether the user has additional project documentation.
2. If docs are provided, review them first as supporting input.
3. Then do a fast repo scan.
4. Identify critical unknowns.
5. Ask only the most important clarification questions that remain.
6. If the user provides no extra documentation, continue from the repository alone.
7. If important gaps remain, mark them as `Unknown:`.
8. Then write `docs/project-context.md`.

### If `docs/project-context.md` already exists

- Read the existing file first.
- Preserve correct, still-supported content.
- Update it in place.
- Ask questions only if critical gaps or contradictions remain.

### Question policy

- Prefer asking first for missing external documentation before asking technical clarification questions.
- Only ask clarification questions after reviewing provided docs and the repository.
- Keep questions concise.
- Ask at most 8 questions.
- Always try to confirm, if still unclear:
  - exact technical stack
  - main architectural pattern
  - deployment/runtime model
  - key domain terms
  - whether there is frontend, auth, public API, background jobs, and multitenancy
  - whether there are legacy or transitional areas that should not be copied

## What to inspect first

- README
- package manifests
- lockfiles
- app entrypoints
- routes
- controllers / handlers
- services / use cases
- schemas / contracts
- config files
- env examples
- tests
- deployment / infra files
- existing docs

## Ignore by default

- node_modules
- dist
- build
- coverage
- vendor
- generated code
- compiled assets
- caches
- snapshot outputs

Only use them if they are the only reliable source of active runtime behavior.

## Actively look for

- migration-in-progress areas
- duplicated old/new implementations
- deprecated endpoints still present
- parallel data models
- feature flags hiding active behavior
- temporary adapters or shims
- compatibility layers
- dormant modules present but not wired

## What to optimize for

- explicit scope
- explicit non-scope
- system boundaries
- technical stack clarity
- domain terminology clarity
- major constraints
- useful negatives
- unknowns that matter for reasoning
- which code should be followed as a pattern
- which code should not be copied

## Iteration discipline  
  
- Treat `docs/project-context.md` as an iterative briefing, not a one-shot perfect document.  
- At the end of each update, list unresolved questions, if any.  
- Only ask questions whose answers would materially improve future reasoning.  
- Prefer `Unknown:` for gaps that are not blocking or not worth interrupting the user.  
- Keep unresolved questions concise and prioritized.

## Preferred style of statements

Prefer statements like:
- no frontend
- no authentication yet
- no public API
- no background jobs
- single-tenant
- internal-only tool
- read-only integration
- persistence in PostgreSQL only
- do not copy `legacy/`; migration in progress
- term `Lead` means pre-customer sales entity, not registered user

## Avoid

- long prose
- marketing language
- generic architecture filler
- exhaustive folder listings
- restating trivial facts visible from one filename scan
- setup/build instructions unless they materially affect reasoning
- large glossaries; include only jargon that prevents mistakes

## Evidence labels

Use these labels when useful:
- Confirmed:
- User-stated:
- Inferred:
- Unknown:
- Conflict:

## Output file template

```md
# Project Context

## Purpose
Brief statement of what the project is for.

## Technical Stack
Languages, frameworks, runtime, database, infra, and other major technical choices.
Only include details that materially affect reasoning.

## Critical Jargon
Define only ambiguous business terms, internal labels, or domain entities that could cause reasoning mistakes.

## In Scope
Concrete capabilities the system currently has.

## Out of Scope
Concrete capabilities the system currently does not have.

## Main Actors
Users, systems, or roles that interact with it.

## Main Flows
Core end-to-end flows only.

## System Boundaries
Where this system starts and stops.

## Main Components
Only the components that matter for reasoning.

## Reference Patterns
Preferred implementation styles, modules, or architectural paths worth following.

## Non-Reference Areas
Legacy, deprecated, transitional, experimental, or misleading code that should not be copied as a pattern.

## Integrations
External services, APIs, queues, storage, auth providers, etc.

## Data / Persistence
Main data stores, ownership, and persistence assumptions.

## Auth / Permissions
Current authentication and authorization model, or absence of one.

## Background Processing
Jobs, queues, schedulers, async workers, or absence of them.

## Constraints / Assumptions
Important technical or business constraints.

## Known Unknowns
Important gaps, ambiguities, or unresolved questions.

```

## Writing rules

- extremely concise
- factual
- explicit
- high signal
- bullet-first
- no fluff

## Update discipline

- preserve correct existing content
- revise only sections affected by new evidence
- remove stale claims no longer supported by code
- keep terminology stable unless the codebase clearly changed

## Size constraint

- keep the file compact
- prefer dense bullets over prose
- omit low-value detail
- do not exceed the minimum needed for correct future reasoning
- do not try to make the file complete
- make it sufficient for correct future reasoning

## Quality bar

After reading the file, a future LLM should understand:
- what the system is
- what it is not
- what important terms mean
- what code to follow
- what code to avoid
- what remains unclear

Do not omit the technical stack just because parts are inferable from the repo.
Make the stack explicit when it affects future reasoning.
When unsure, mark uncertainty instead of smoothing it over.
