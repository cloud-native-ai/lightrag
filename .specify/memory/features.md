# Feature Index

This index tracks all functional and non-functional features managed within the project. It serves as the central directory for specifications, plans, and implementation status.

<!--
  ACTION REQUIRED for any command that mutates this table (`/speckit.feature`,
  `/speckit.plan`, `/speckit.implement`):

  The `Total Features` value below MUST be auto-derived from the number of data
  rows in the table (count rows that begin with `| ` and a feature ID — exclude
  the header and separator rows). Do NOT maintain it by hand and do NOT bump it
  separately when adding a feature row; recompute on every write.

  Reference shell expression a human can paste to verify:
      awk -F'|' '/^\| [0-9]{3} \|/ {n++} END{print n}' .specify/memory/features.md
-->

**Total Features**: 19 _(auto-derived; recompute on every edit — see comment above)_

## Functional Features

| ID | Name | Description | Status | Feature Details | Last Updated |
|----|------|-------------|--------|-----------------|---------------|
| 001 | Document Ingestion & Parsing | Multi-format document parsing pipeline (PDF, DOCX, PPTX, XLSX, Markdown, images) with external parser support | Implemented | [features/001.md](features/001.md) | 2026-06-23 |
| 002 | Knowledge Graph Construction & Storage | Entity/relation extraction, incremental KG construction, multi-backend persistent storage | Implemented | [features/002.md](features/002.md) | 2026-06-23 |
| 003 | RAG Query & Retrieval | Multiple query modes (local, global, hybrid, naive) with reranking, citation, and context precision | Implemented | [features/003.md](features/003.md) | 2026-06-23 |
| 004 | Multimodal Document Processing | RAG-Anything integration for images, tables, equations via MinerU/Docling services | Implemented | [features/004.md](features/004.md) | 2026-06-23 |
| 005 | Text Chunking Strategies | Four chunking strategies: Fixed, Recursive, Vector Semantic, Paragraph Semantic | Implemented | [features/005.md](features/005.md) | 2026-06-23 |
| 006 | REST API Server | FastAPI server with document/query/graph endpoints, JWT auth, WebUI dashboard | Implemented | [features/006.md](features/006.md) | 2026-06-23 |
| 007 | LLM Provider Integration | 14+ LLM providers with role-specific configuration (EXTRACT, QUERY, KEYWORDS, VLM) | Implemented | [features/007.md](features/007.md) | 2026-06-23 |
| 008 | Interactive Setup Wizard | Guided setup for LLM, embedding, storage, and server configuration via interactive prompts | Implemented | [features/008.md](features/008.md) | 2026-06-23 |
| 009 | Containerization & Deployment | Multi-variant Docker images, Docker Compose, CI/CD pipelines for Docker Hub and PyPI | Implemented | [features/009.md](features/009.md) | 2026-06-23 |
| 010 | Evaluation & Tracing | RAGAS quality evaluation, Langfuse observability tracing, offline retrieval checks | Implemented | [features/010.md](features/010.md) | 2026-06-23 |
| 012 | CLI Administration Tools | CLI tools for password hashing, cache management, VDB rebuild, and KG visualization | Implemented | [features/012.md](features/012.md) | 2026-06-23 |

## Non-Functional Features

| ID | Name | Description | Status | Feature Details | Last Updated |
|----|------|-------------|--------|-----------------|---------------|
| 011 | Code Quality & Linting | Ruff linting/formatting, pre-commit hooks, pytest async test framework, CI quality gates | Implemented | [features/011.md](features/011.md) | 2026-06-23 |
| 013 | Test Coverage Reporting | Automated coverage measurement with thresholds enforced in CI | Draft | [features/013.md](features/013.md) | 2026-06-23 |
| 014 | Dependency Security Scanning | Vulnerability scanning, SBOM generation, and automated dependency updates | Draft | [features/014.md](features/014.md) | 2026-06-23 |
| 015 | Performance Benchmarking | Benchmark suite for ingestion throughput, query latency, and backend comparison | Draft | [features/015.md](features/015.md) | 2026-06-23 |
| 016 | Contributor Guide & Changelog | CONTRIBUTING.md, CHANGELOG.md, PR/issue templates for maintainability | Draft | [features/016.md](features/016.md) | 2026-06-23 |
| 017 | API Versioning & Deprecation Policy | Formal versioning strategy for library and REST API with deprecation timelines | Draft | [features/017.md](features/017.md) | 2026-06-23 |
| 018 | Structured Logging & Metrics | JSON structured logging, Prometheus metrics, health check endpoint | Draft | [features/018.md](features/018.md) | 2026-06-23 |
| 019 | Kubernetes Deployment Support | Helm chart for K8s deployment with health probes, scaling, and external backends | Draft | [features/019.md](features/019.md) | 2026-06-23 |