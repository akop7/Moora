# Moora — Project Memory

Last updated: 2026-11-02.

Purpose: a concise handoff record of confirmed context, selected design decisions, actual progress, unresolved questions, and the next action. This file is not a database, an audit trail, or a place for credentials.

## 1. Confirmed context

- Repository: `akop7/Moora`; cloud checkout: `/workspace/Moora`.
- Initial checked-out commit: `7f61035c646881d19e9eed2ff4740bfae547f3af` (`Initial commit`). Earlier onboarding confirmed it matched remote `main` at that time. The cloud checkout uses a local branch named `work`; do not rename/reset it solely to match the remote branch label.
- At the start of documentation work, the repository contained only `README.md` with the heading `Moora` and had no application code, manifests, tests, or agent instructions.
- User-provided source: uploaded **Pasted text.txt**, received 2026-11-02. It describes **SmartLabel AI**, an intelligent multi-agent platform with advanced RAG for medical-device labeling automation.
- Team: **Part Time Humans**.
- Stated context: **Tata Elxsi TELIPORT Season 4**, **Case Study 4: End-to-End Medical Device Labeling Automation Management**. Competition details have not been independently verified.
- The user asked for six documents: `PRD.md`, `Architecture.md`, `Rules.md`, `Phases.md`, `Design.md`, and `Memory.md`.
- The brief explicitly requests architecture, scope, dependencies, risks, and implementation order before application coding, followed by module-by-module development.
- Final regulatory and quality approvals must remain with authorized humans. Sources must be public or properly licensed. Traceability and honest separation of prototype versus production capabilities are mandatory.

## 2. Document map

| Document | Responsibility |
| --- | --- |
| [PRD.md](PRD.md) | Users, problem, scope, requirements, acceptance targets, and limitations |
| [Architecture.md](Architecture.md) | Stack, app flow, RAG, graph execution, data model, APIs, and planned folder structure |
| [Rules.md](Rules.md) | Coding boundaries, approved libraries, evidence handling, agent behavior, security, errors, and approval invariants |
| [Phases.md](Phases.md) | Implementation sequence, dependencies, exit gates, and future extensions |
| [Design.md](Design.md) | Colors, type, pages, interactions, accessibility, and review workspace behavior |
| [Memory.md](Memory.md) | Current facts, decisions, progress, unknowns, and next action |

Use the document responsible for a decision as its detailed source. Resolve contradictions explicitly instead of treating the newest sentence as automatic authorization to weaken a requirement.

## 3. Selected design decisions

These are recommendations made while preparing the documents. The brief supplies the intent; it did not independently specify every implementation choice below.

| ID | Decision | Reason |
| --- | --- | --- |
| D-01 | Use Moora for the app/repository and SmartLabel AI as the solution descriptor | Reconcile the two names without discarding either. Branding remains open. |
| D-02 | Four MVP capabilities: impact analysis, regulatory RAG, grounded drafting, artwork validation | Deliver one meaningful end-to-end workflow with a student-team scope. |
| D-03 | Implement eight brief roles as bounded graph nodes/shared services, not eight microservices | Reduce operational complexity and keep authoritative rules deterministic. |
| D-04 | React/TypeScript/Vite/Tailwind frontend; FastAPI/Pydantic/SQLAlchemy backend | Fit the proposed stack with explicit typed contracts. |
| D-05 | PostgreSQL/pgvector + genuine BM25 + reranking | Satisfy the advanced retrieval requirement and retain relational lineage. |
| D-06 | `rank-bm25` over authorized partitions capped at 5,000 chunks for the MVP | Keep infrastructure small while making capacity limits explicit. |
| D-07 | LangGraph checkpoints; Celery/Redis delivery; database outbox and idempotency | Survive retries, worker restarts, and long human review pauses. |
| D-08 | Private S3-compatible artifacts and versioned fixed-template PDFs | Keep reviewed/exported content reproducible and access controlled. |
| D-09 | Two distinct human regulatory/quality reviewers, neither the submitter | Establish a clear prototype separation-of-duties policy. |
| D-10 | Bind decisions to exact review manifests; invalidate on applicable changes | Prevent stale approvals from authorizing a new artifact. |
| D-11 | English-source demo with one synthetic family, two variants, two evidence contexts, two packaging levels, eight labels | Bound fixtures; this does not establish market language compliance. |
| D-12 | Defer multilingual qualification and live ERP/PLM integration | Protect the core workflow's implementation and validation effort. |
| D-13 | Watermarked demo packages only; no production-release endpoint | Follow the brief's human-control and prototype constraints. |
| D-14 | Local session authentication initially; enterprise OIDC later | Demonstrate actual authorization without requiring an external identity provider first. |
| D-15 | Light workspace, navy navigation, teal actions, Inter/system typography | Support readable document comparison and evidence review. |

## 4. Current implementation state

| Area | Actual status |
| --- | --- |
| Repository/cloud onboarding | Earlier onboarding verified checkout integrity and read-only GitHub access; no setup script was needed for the empty repository. |
| Planning documents | All six requested documents created in the repository; documentation checks are recorded in the session log below. |
| Application/frontend/API | Not implemented. |
| Dependency manifests/lockfiles | Not created; planned versions and libraries are not installed as project dependencies. |
| Database/migrations/object storage | Not configured for the application. |
| Agents/RAG/model providers | Not implemented or exercised. |
| Source corpus/templates/synthetic fixtures | Specified at a planning level; actual content, rights, and expected results still need selection. |
| Authentication/review/approval | Policy documented; no enforcement code exists yet. |
| UI/accessibility | Design specified and eleven base palette contrast pairings checked; no rendered interface or accessibility audit yet. |
| Product tests/benchmarks | None exist or have run. PRD numerical targets are proposed, not measured. |
| Deployment/publication | No application deployment or environment publication was performed as part of documentation work. |

Do not infer completed functionality from folder diagrams, API tables, example states, or this decision list.

## 5. Current phase and next action

Current position: **Phase 0 documentation is drafted; application implementation has not started.** Source, fixture, provider, and reviewer details remain open at their respective phase gates.

Next coding task: scaffold Phase 1 in the existing checkout. Create the selected frontend/Python workspaces, local PostgreSQL/pgvector + Redis + object storage, migrations, authentication/permissions, and an authenticated functional smoke flow. Establish repeatable install/start commands and CI using the actual tool versions chosen during implementation.

Do not begin with all eight agents or a complete dashboard populated by invented results. The first AI-dependent vertical slice comes after versioned product/change data and authorized source handling exist.

## 6. Open decisions and dependencies

| Item | Needed by | Current handling |
| --- | --- | --- |
| Final public-facing name | Presentation polish | Use Moora — SmartLabel AI provisionally. |
| Deadline and team capacity | Scheduling | No calendar estimates or delivery promises recorded. |
| Synthetic device and approved wording/templates | Phase 2/4 | Do not invent real clinical or regulatory requirements. |
| Authorized corpus and rights register | Phase 3 | No actual FDA/EU/ISO/IMDRF content supplied in the brief. |
| LLM/embedding provider, budget, and data policy | Phase 3/4 | Use adapters; obtain credentials through secure environment settings when needed. |
| Embedding dimension and reranker model/license | Phase 3 | Choose and pin with evaluation/resource checks. |
| PyMuPDF and other redistribution licenses | Before installing/distributing affected components | Verify fit; a PDFium adapter is the planned extraction alternative if necessary. |
| Qualified domain reviewer / real reviewer policy | Before any non-demo use | Demo qualification metadata does not establish real professional competence. |
| Cloud provider, region, identity provider | Later deployment | Portable interfaces; no provider is presumed. |
| Translation locale, glossary, and reviewer | Optional Phase 8 | No qualified multilingual release in the MVP. |

These unknowns do not prevent generic foundation work. They may block live AI, source ingestion, domain acceptance, or production use. Record the exact boundary instead of describing the entire project as ready.

## 7. Non-negotiable carry-forward rules

- Never invent evidence, compliance status, measured results, approvals, or source permissions.
- Keep critical/major findings and missing mandatory evidence/checks blocking.
- Approval is a human, authenticated, version-bound decision; generation completion is separate.
- Preserve immutable history and invalidate affected approval effectiveness after changes.
- Use an explicitly labeled replay mode only; never hide a failed live integration behind fixtures.
- Keep credentials and restricted source documents out of Git, logs, prompts, and this memory file.
- Use the existing isolated cloud checkout; no additional Git worktree without a user request.
- Update actual status only after the relevant work and checks have executed.

## 8. Session log

### 2026-11-02 — Project documentation

- Read the complete user-supplied project brief and confirmed the repository was still an empty application skeleton with only a README.
- Created the six requested planning documents and linked their responsibilities.
- Recorded critical risks, four-capability MVP scope, advanced RAG, durable orchestration, schema/API contracts, human gates, delivery phases, and visual design.
- No application logic, dependency installation, external source ingestion, or deployment was performed.
- Documentation validation passed: six requested files, 29 resolving relative links, seven closed code/diagram blocks, and clean patch whitespace. Existing tracked files were unchanged. Reviewed scope, phase status, reviewer policy, and demo-only packaging terminology for consistency.
- Eleven specified palette pairings met their text or non-text contrast thresholds by calculation. No application tests, live AI evaluation, rendered UI checks, or regulatory validation ran.

## 9. How to maintain this file

After a meaningful task, update the relevant status row, add a short dated log entry with changed files and actual checks, record unresolved blockers, and identify the next concrete action. Preserve important decisions and their rationale. Do not turn this file into a transcript or copy full test logs, prompts, regulatory text, or secrets into it.
