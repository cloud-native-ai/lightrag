<!--
Sync Impact Report
==================
Version change: (none) → 1.0.0.1 (MAJOR — initial constitution from template)
Modified principles: N/A (initial creation)
Added sections:
  - Core Principles (I–VII)
  - Security & Data Privacy
  - Development Workflow
  - Governance
Removed sections: N/A
Templates requiring updates:
  - .specify/templates/plan-template.md → ✅ updated (Constitution Check table maps 1:1 to all 7 principles)
  - .specify/templates/requirements-template.md → ✅ updated (Feature binding section references Principle VII)
  - .specify/templates/tasks-template.md → ✅ updated (Tests Mode references Principle III)
  - README.md → ⚠ pending (no principle refs to update yet; defer to future sync)
  - docs/quickstart.md → N/A (file does not exist)
Follow-up TODOs: None (all placeholders resolved)
-->

# LightRAG Constitution

## Core Principles

### I. Library-First Design

Every significant feature MUST begin as a cohesive, reusable module within the
`lightrag` Python package. Modules MUST:

- Be self-contained and independently testable.
- Have a single, clearly documented responsibility.
- Expose a stable public API with explicit `__all__` or documented exports.
- Avoid circular dependencies; prefer dependency injection where feasible.

Rationale: LightRAG is distributed as a Python library (`lightrag-hku` on PyPI).
Library-first design ensures that core functionality remains usable both
programmatically and via the API server, enables reuse across embedding, querying,
and storage workflows, and simplifies testing.

### II. API & Interface Consistency

LightRAG exposes functionality through both a Python library API and a FastAPI
REST server. Both interfaces MUST:

- Maintain consistent parameter naming, semantics, and defaults across the library
  and REST API.
- Return structured, well-typed responses (Pydantic models for the REST API;
  typed dataclasses or Pydantic models for the library).
- Follow REST conventions: proper HTTP status codes, meaningful error messages
  in a consistent JSON envelope, and versioned endpoints where breaking changes
  are unavoidable.
- Provide CLI entry points (`lightrag-*` scripts) for administrative operations
  that accept plain text or JSON I/O.

Rationale: Users interact with LightRAG through multiple channels (library, REST,
CLI). Consistency across these surfaces reduces cognitive load, prevents
integration bugs, and ensures documentation remains accurate for all usage modes.

### III. Test-First Development

Implementation MUST follow a Test-Driven Development style for core logic:

- Write or update tests BEFORE implementing new behavior.
- Ensure tests FAIL first (Red), then implement to make them PASS (Green).
- Refactor only with all tests passing (Refactor).

At minimum:

- Pure functions and utility modules MUST have unit tests.
- API endpoints MUST have contract tests validating request/response schemas.
- Critical flows (document ingestion, query, deletion) MUST have automated
  regression coverage.

Rationale: LightRAG's pipeline involves multiple stages (parsing, chunking,
embedding, graph construction, storage, retrieval). TDD reduces regressions
and clarifies intent at each stage.

### IV. Storage Backend Portability

LightRAG supports multiple storage backends (PostgreSQL+pgvector, Neo4j, MongoDB,
Milvus, Qdrant, Redis, OpenSearch, Faiss). All storage operations MUST:

- Be abstracted behind a common interface (base class or protocol) that each
  backend implements.
- Support the same core operations (insert, query, delete, index) with consistent
  semantics across backends.
- Include backend-specific integration tests that validate correctness against
  a real or emulated instance of that backend.
- Document any backend-specific limitations, performance characteristics, or
  configuration requirements in the relevant docs.

Rationale: Users choose storage backends based on their infrastructure. A
consistent abstraction layer ensures that switching backends or adding new ones
does not require changes to higher-level LightRAG logic, and that all backends
meet the same quality bar.

### V. Observability, Versioning & Simplicity

All components MUST be observable and versioned:

- Use structured logging for important events and errors (document ingestion,
  query execution, storage operations).
- Follow semantic versioning (MAJOR.MINOR.PATCH) for the `lightrag-hku` package.
- Document breaking changes and migration notes in release notes.
- Keep designs as simple as possible; avoid speculative features (YAGNI).
- Expose tracing hooks (Langfuse integration) for critical paths to support
  production debugging.

Rationale: LightRAG is deployed in diverse environments from local laptops to
production clusters. Observability and clear versioning make the system
debuggable, upgradable, and maintainable.

### VI. Continuous Integration & Quality Gates

Changes MUST be safe to merge:

- Linting (ruff), formatting, and basic tests MUST pass in CI.
- A minimal smoke test or example run SHOULD be provided for new features.
- New behavior MUST be reflected in specs/plan/tasks/docs where applicable.
- Pre-commit hooks MUST be configured and enforced for all contributors.

Rationale: LightRAG is an open-source project with multiple contributors.
Consistent quality gates prevent regressions and ensure predictable releases.

### VII. Feature-Centric Development

Feature is the long-term core framework of the project:

- The Feature list under `.specify/memory/features.md` MUST remain the "single
  source of truth" for the project's capabilities.
- Every phase of spec → plan → tasks → implement MUST review Feature
  additions, merges, splits, or deletions.
- Feature changes MUST be traceable to corresponding spec/plan evidence and
  recorded in the per-Feature detail file under `.specify/memory/features/`.
- Each requirements specification MUST declare its binding to a Feature ID
  and Feature Name.

Rationale: LightRAG evolves rapidly with new capabilities (multimodal processing,
new storage backends, chunking strategies, evaluation frameworks). Keeping the
Feature registry as the backbone ensures long-term consistency and makes the
project's evolution auditable.

## Security & Data Privacy

LightRAG processes user documents that may contain sensitive information. All
development MUST:

- Never log raw document content at INFO level or above; use DEBUG level only
  and with opt-in flags.
- Sanitize or redact sensitive fields (API keys, tokens, passwords) from logs
  and error messages.
- Support authentication for the API server (JWT-based) and enforce it by
  default in production deployments.
- Validate and sanitize all file uploads (MIME type checks, size limits,
  content inspection) before processing.
- Document any data that leaves the deployment boundary (e.g., calls to
  external LLM APIs) so users can make informed privacy decisions.

Rationale: RAG systems inherently handle user-provided content. Security and
privacy must be designed in from the start, not bolted on later.

## Development Workflow

All substantial changes MUST follow this workflow:

1. **Feature/Spec Alignment**: Identify or create the relevant Feature entry
   under `.specify/memory/features.md`.
2. **Requirements**: Produce or update the requirements specification under
   `.specify/specs/<REQUIREMENTS_KEY>/requirements.md`.
3. **Plan**: Generate an implementation plan under the same spec directory.
4. **Tasks**: Derive actionable, independently testable tasks from the plan.
5. **Implement**: Execute tasks with TDD. Commit after each logical group.
6. **Review**: Validate completion against the plan and constitution.
7. **Document**: Update relevant docs in `docs/` and the README for
   user-facing changes.

Code review is mandatory for all PRs. Reviewers MUST verify:

- Compliance with this constitution's principles.
- Test coverage for new and modified paths.
- Documentation updates for user-facing changes.
- No regression in existing functionality.

## Governance

This constitution supersedes all other project guidelines and conventions.
It defines the non-negotiable principles that govern LightRAG's architecture,
development, and maintenance.

**Amendment Procedure**:

1. Propose amendments via a dedicated spec under `.specify/specs/`.
2. Amendments MUST be reviewed and approved by project maintainers.
3. Every amendment MUST bump the constitution version according to the
   versioning policy below.
4. After amendment, run `/speckit.feature` to refresh the feature index and
   `/speckit.requirements` on any in-progress specs to ensure alignment.

**Versioning Policy**:

Constitution versions follow `x.y.z.ddd` format:

- **MAJOR (x)**: Rewrite-level changes — complete restructuring of principles,
  backward incompatible governance removals or fundamental redefinitions.
- **MINOR (y)**: Core section modifications — adding/removing/renaming
  principles, materially expanding or contracting principle scope.
- **PATCH (z)**: Descriptive refinements — clarifications, wording improvements,
  typo fixes, non-semantic adjustments.
- **DAILY (ddd)**: Increments on EVERY constitution update regardless of
  magnitude. Resets to 1 when x, y, or z increments.

**Compliance Review**:

- All PRs MUST include a brief constitution compliance statement in the
  description, noting any principles that are relevant to the change.
- The `plan-template.md` Constitution Check table MUST be validated against
  the current constitution for every new spec.
- Periodic reviews (at least quarterly) SHOULD assess whether the constitution
  still reflects the project's actual practices and needs.

**Version**: 1.0.0.1 | **Ratified**: 2026-06-23 | **Last Amended**: 2026-06-23