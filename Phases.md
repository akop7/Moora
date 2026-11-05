# Moora — Delivery Phases

Status: roadmap, not a record of completed application work.

As of 2026-11-02, the repository contains the README and six planning documents; no application features are implemented.

## 1. Delivery strategy

Build one traceable vertical workflow before broadening coverage. The core MVP contains four integrated capabilities: change impact, regulatory RAG, grounded drafting, and artwork validation. Authentication, deterministic rules, audit records, and human approval gates support all four.

Use the existing `/workspace/Moora` checkout for cloud development. Do not create another worktree unless requested. Future directory names below are planned outputs, not files already present.

Use [PRD.md](PRD.md) for scope, [Architecture.md](Architecture.md) for contracts and state, [Rules.md](Rules.md) for boundaries, and [Design.md](Design.md) for UI behavior. Update [Memory.md](Memory.md) with actual outcomes at each exit gate.

| Phase | Outcome | Depends on | Current status |
| --- | --- | --- | --- |
| 0 | Scope, documents, source/fixture decisions | Project brief | Documents drafted; remaining decisions pending |
| 1 | Runnable foundation, authentication, permissions, CI | 0 | Not started |
| 2 | Versioned products, changes, and impact map | 1 | Not started |
| 3 | Controlled knowledge and evaluated advanced RAG | 2 and source/provider access | Not started |
| 4 | Grounded drafts and controlled PDF rendering | 3 | Not started |
| 5 | Artwork comparison and deterministic checks | 4 | Not started |
| 6 | Durable human review, audit, and demo packaging | 2–5 | Not started |
| 7 | Repeatable measured end-to-end demonstration | 6 | Not started |
| 8 | Optional localization/integration expansion | 7 and explicit feature selection | Deferred |

No dates or team capacity were supplied. Relative sizes below are planning guidance, not a delivery commitment. Phases 3, 5, and 6 contain the largest technical or domain uncertainties. Cut optional breadth before removing evidence, recovery, or approval gates.

## 2. Phase 0 — Scope and trusted inputs

Relative size: small engineering effort, potentially variable source/reviewer lead time.

Deliverables:

- The six requested planning documents and a recorded Moora/SmartLabel AI naming convention.
- A source-rights register for the proposed corpus, including what may be stored, embedded, sent to providers, and exported.
- A synthetic demo fixture definition: one device family, two variants, US/EU evidence contexts, pouch/carton packaging, English source content, and eight labels.
- Approved synthetic product revisions, controlled text/template choices, expected impact mappings, and a clear explanation of which data is simulated.
- Model/provider, embedding profile/dimension, reranker, document-library license, and cost-limit decisions at the points where they become necessary.
- Named demonstration roles and a policy for separate regulatory and quality decisions. Seeded role/qualification metadata must be labeled synthetic.

Exit gate:

- Requirements, planned architecture, and MVP boundaries agree.
- Every proposed external source has a documented permitted acquisition/usage path before ingestion.
- The team can explain the demonstration's expected outputs without relying on an invented regulation or a model's unverified safety advice.

Current evidence: planning documents have been created. Corpus, provider, fixture content, reviewer availability, and delivery schedule are still unconfirmed. Those choices need not delay the generic Phase 1 skeleton.

## 3. Phase 1 — Foundation, login, and permissions

Relative size: medium.

Implement:

1. Create the npm and uv workspaces, compatible dependency pins, and lockfiles.
2. Add the React/Vite app shell and FastAPI app, shared typed settings, error envelope, logging, and OpenAPI generation.
3. Add Docker Compose dependencies for PostgreSQL/pgvector, Redis, and private S3-compatible storage; verify container proxy/CA configuration in the cloud environment.
4. Create the first Alembic migrations for organizations, users, memberships, reviewer qualifications, sessions, audit events, jobs, outbox, and idempotency records.
5. Implement login/logout, session revocation, CSRF/origin protection, role checks, and isolated demo seed accounts. Credentials come from local/environment configuration, never committed defaults.
6. Build the desktop application shell, login screen, empty dashboard, and permission-aware routes using the design tokens.
7. Add CI for frontend type/lint checks, backend checks, and relevant tests. Add reproducible local install/start instructions only after executing them.

Exit gate:

- A fresh setup from the documented commands starts the services and an authenticated functional API/UI flow works.
- Repeating seed/setup operations is safe and does not duplicate data or reset unrelated state.
- Unauthorized, cross-organization, and CSRF-invalid actions are rejected through direct API calls.
- Queued test work can persist through a worker restart and appears with a real terminal outcome.
- Health/readiness checks report actual dependencies; failures produce safe errors.

Required validation: authentication, authorization, session expiry, migrations on an empty database, idempotent seeding, and one real background-task integration check. No AI model is required yet.

## 4. Phase 2 — Product revisions and impact analysis

Relative size: medium.

Implement:

1. Product, product-revision, template, label, label-revision, dependency, and change-request schemas.
2. Validated import of synthetic product data and approved baseline metadata.
3. A change form that selects approved before/after revisions and derives the actual changed fields server-side.
4. A deterministic dependency traversal that produces affected label IDs and explanatory paths.
5. A constrained AI explanation adapter if useful; the dependency engine remains authoritative and can function without it.
6. Product pages, revision history, change detail, and an accessible impact table/graph.
7. Audit events for inputs, scope confirmation, and revisions.

Exit gate:

- The sterilization-field fixture identifies exactly the expected eight affected labels, with no silently inferred mapping.
- Unmapped fields or inconsistent product/label relationships produce visible unresolved input.
- Revision history is preserved and an existing approved baseline cannot be overwritten.
- Unauthorized users cannot read or alter another organization's product data.

Required validation: fixture precision/recall, negative mappings, revision constraints, request validation, and permission tests. Live ERP/PLM connectivity is deferred.

## 5. Phase 3 — Authorized ingestion and advanced RAG

Relative size: large; external source rights and provider/model access are dependencies.

Implement:

1. The source/version/rights registry and safe file upload/quarantine path.
2. Native text extraction, OCR fallback, layout/section anchors, extraction diagnostics, and knowledge-steward publication.
3. Parent/child semantic chunks, a pinned embedding profile, pgvector persistence, and immutable knowledge snapshots.
4. Authorized metadata filtering, protected query rewriting, real BM25, vector retrieval, reciprocal rank fusion, and cross-encoder reranking.
5. Evidence contracts with exact document-version/page/chunk references, claim links, and support/conflict status.
6. The evidence library, ingestion status view, source viewer, and evidence drawer.
7. A frozen evaluation set with at least 20 answerable and 10 insufficient/conflicting/outdated/unauthorized cases.
8. Source-version updates and staleness events for downstream consumers.

Exit gate:

- The planned corpus is ingested with permitted rights and inspectable extraction provenance.
- Both BM25 and vector branches execute, reranking executes, and all selected citations resolve correctly.
- Evaluation reports actual recall@10, citation support, abstention, latency, and costs against PRD targets; failures are explained and addressed before dependent drafting is accepted.
- Wrong-organization, wrong-jurisdiction, unpublished, and unpermitted sources do not leak into candidates or model context.
- Insufficient evidence blocks dependent generation. A replay fixture is never represented as a live model result.

Required validation: ingestion integration, extraction samples, access filtering, version handling, retrieval evaluation, evidence conflicts, and abstention. An LLM-generated evaluation grade alone is insufficient.

## 6. Phase 4 — Grounded drafts and fixed-template rendering

Relative size: medium.

Implement:

1. Versioned prompt templates and typed `DraftEdit` outputs restricted to allowed label fields.
2. Field-to-product/evidence/approved-text provenance and protected identifiers/units.
3. Unsupported-claim checks, bounded repair attempts, and explicit input-required outcomes.
4. A structured draft editor that always creates successor revisions.
5. ReportLab rendering from controlled templates, embedding fonts/assets only with permitted rights.
6. Immutable candidate PDFs, hashes, previews, and a text-change view.
7. Durable graph execution through validation, impact, retrieval, authoring, and rendering, including node idempotency.

Exit gate:

- A complete sample change produces inspectable structured edits and a candidate PDF from the same revision.
- Every accepted changed field has valid provenance; unsupported safety content is blocked.
- Leading zeros, product identifiers, units, and mandatory template regions are preserved.
- Manual edits create a new revision and invalidate downstream checks.
- Provider failure, malformed output, and worker restart have explicit recoverable outcomes.

Required validation: schema validation, grounding negatives, fixed-template rendering, hash consistency, revision invalidation, and graph replay. Translation and arbitrary label layout authoring are deferred.

## 7. Phase 5 — Artwork and deterministic verification

Relative size: large.

Implement:

1. Baseline/candidate PDF extraction and page rasterization with recorded scale and transforms.
2. Text, bounding-box, layout, image-region, and permitted symbol-reference comparisons.
3. Barcode decoding and expected-payload checks without claiming print-quality certification.
4. Versioned deterministic rules for fixture-defined mandatory fields, identifiers, allowed values, and required evidence/check completion.
5. Finding categories, severity, regions, provenance, and explicit `NOT_EVALUATED`/`ERROR` outcomes.
6. Side-by-side comparison, overlay controls, keyboard-accessible finding lists, and evidence links.
7. An annotated set of at least 12 comparisons including changed/missing text, symbol changes, geometry defects, and barcode discrepancies.

Exit gate:

- All seeded critical defects in the frozen fixture set are found; misses and false positives are reported separately.
- Poor OCR, unsupported formats, or failed required checkers block review instead of showing a green result.
- Findings refer to the exact baseline/candidate artifact hashes used in the report.
- A rule's applicability and source/version can be inspected; no regulation is invented as a fixture shortcut.

Required validation: annotated artwork comparisons, checker failure cases, geometry/coordinate correctness, and actual rendered artifact checks. Broad CV coverage across arbitrary customer PDFs is deferred.

## 8. Phase 6 — Human review, audit, and demo package

Relative size: large because of concurrency and approval invariants.

Implement:

1. Immutable review manifests containing all label/artifact/evidence/rule/check versions in scope.
2. Review tasks and human decisions with active role/qualification checks, separation of duties, rationale, and manifest/version tokens.
3. Durable LangGraph review interrupts and authorized resume events; release workers while waiting for people.
4. Explicit changes-requested, rejected, stale, and approved-for-demo states.
5. Nonblocking finding dispositions; no general bypass for blocking findings.
6. Audit timeline, evidence lineage, JSON/PDF reports, checksums, and a watermarked demonstration package.
7. Transactional package authorization and final availability checks, including invalidation while a package is being assembled.
8. Review queue, review workspace, audit screen, and permitted download actions.

Exit gate:

- Two distinct authorized reviewers, excluding the submitter, can approve the exact bundle.
- Self-approval, wrong-role decisions, missing qualifications, stale decisions, direct unauthorized API calls, duplicate decisions, and bypassed UI actions are rejected.
- Product/evidence/template/rule/artifact changes make affected approvals ineffective until revalidation.
- A worker restart during a human wait retains the same review history and does not create a fabricated decision.
- The generated package reconstructs the full chain from change request to approved artifacts and verifies its hashes.
- No production-release route or printer/ERP publishing connector exists.

Required validation: end-to-end approval and rejection journeys, concurrency/version conflict tests, duplicate delivery, unauthorized export, stale-package prevention, and audit reconstruction.

## 9. Phase 7 — Measured demonstration and handoff

Relative size: medium.

Implement and rehearse:

1. A reproducible seed/reset command limited to explicitly identified synthetic demonstration data, with protection against accidental non-demo deletion.
2. A live path from product change through impact, evidence, draft, artwork findings, separate human decisions, and demo package.
3. A negative path with missing evidence or a changed reviewed artifact; show the blocker and permitted recovery.
4. A source/provenance explanation and a clear statement of simulated integrations and production limitations.
5. A baseline manual task comparison using the same inputs and outputs. Record measured elapsed time, reviewer time, node duration, cost, false positives, misses, and evidence scores.
6. A labeled fixture-replay option for presentations when live providers are unavailable. Record replay mode in every resulting report; do not present replay metrics as live measurements.
7. A complete README/setup guide, environment installation/startup configuration, evaluation results, and known limitations.

Exit gate:

- The saved install/start workflow is exercised in its documented directories, including service restart and a representative authenticated functional request.
- All required test suites run with nonzero applicable tests; failures, skips, and external blockers are reported distinctly.
- The team can demonstrate provenance and enforced human gates without manual database edits.
- The demo has actual measurements, or explicitly states that a metric has not yet been measured.
- The six planning documents and project memory match the implemented scope.

Core MVP completion means Phases 1–7 pass their relevant gates. It does not establish enterprise production readiness or regulatory certification.

## 10. Phase 8 — Optional expansion after the core demo

Choose one extension at a time:

| Extension | Prerequisites and acceptance |
| --- | --- |
| One additional language | Approved glossary and translation memory, locale-specific rules, qualified human reviewer, preserved identifiers/units, and separate translation evaluation |
| Simulated-to-live ERP/PLM connector | Named system, permitted API access, mapping/version contract, reconciliation, and fixture-backed connector tests |
| Additional market or device family | Authorized sources, applicable rule/template review, labeled evaluation cases, and no assumptions inherited from another device |
| Regulatory update monitoring | Authorized acquisition, immutable version comparison, applicability review, and tested staleness propagation |
| More document/layout formats | Parser/license review, bounded extraction, labeled comparison truth set, and explicit unsupported conditions |
| Enterprise authentication | OIDC provider, organization mapping, revocation policy, qualified reviewer integration, and isolation tests |
| Production deployment | Domain-led validation, secure operations, backup/restore, retention, threat modeling, change control, and a separately approved production release design |

Do not compress these into the initial demo by removing approval gates or claiming that a model has replaced a qualified reviewer.

## 11. Definition of done for each task

- The behavior meets a named PRD requirement and the current phase's scope.
- Contracts, migrations, permissions, failure states, and UI behavior agree.
- Relevant tests and functional checks have executed and their outcomes are recorded.
- No credentials, restricted corpus, or accidental generated outputs are committed.
- Documentation and memory identify implemented behavior, limitations, and the next concrete step.
- Remaining blocked work identifies the missing dependency precisely; untested behavior is not described as complete.

The next implementation task is **Phase 1: scaffold the selected workspace, start local infrastructure, and implement a minimal authenticated API/UI flow**. Select the source corpus and provider in parallel with this foundation work when those decisions become available.
