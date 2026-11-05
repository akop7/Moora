# Moora — Product Requirements Document

Status: proposed implementation baseline, not an implemented or validated product.

Prepared: 2026-11-02.

Team: **Part Time Humans**.

Context: **Tata Elxsi TELIPORT Season 4, Case Study 4: End-to-End Medical Device Labeling Automation Management**, as supplied in the project brief.

## 1. Product identity and purpose

**Moora** is the application and repository name. **SmartLabel AI** is the proposed solution name; use **Moora — SmartLabel AI** in the project presentation until the team decides otherwise. This naming convention reconciles the two names in the brief and is a planning decision, not a confirmed rebrand.

Moora helps a medical-device labeling team turn an approved product-data change into traceable proposed label updates, evidence-backed findings, and a package for human review. It connects product revisions, affected labels, regulatory evidence, generated drafts, artwork checks, and reviewer decisions in one workflow.

The prototype produces **demonstration artifacts only**. It must never autonomously approve a label, certify compliance, validate a sterilization process, or release content into a production labeling system.

Related documents: [Architecture](Architecture.md), [Engineering and AI rules](Rules.md), [Delivery phases](Phases.md), [Visual design](Design.md), and [Project memory](Memory.md).

## 2. Problem and opportunity

The supplied brief describes a manufacturer managing product variants, packaging levels, markets, and languages through spreadsheets, disconnected systems, manual artwork inspection, and repeated approval cycles. A single modification can propagate across many label versions. The brief's examples of 30+ labels and 20+ languages describe the motivating scenario; they are not measured results or MVP capacity claims.

The most valuable intervention is a complete, inspectable change workflow. A reviewer should be able to follow a changed field to every affected label, open the evidence behind a proposed edit, inspect the actual rendered artwork, and identify exactly which version was approved.

### Critical evaluation

| Strength or risk | Product response |
| --- | --- |
| Labeling has concrete inputs, outputs, and human review stages | Build around versioned change requests and review bundles rather than a general chat interface. |
| Changes propagate through a known product/label structure | Use explicit dependency mappings; use AI to explain uncertain impact, not silently invent relationships. |
| Regulatory text is conditional, time-sensitive, and sometimes licensed | Require applicability metadata, source rights, effective-date checks, and cited evidence. |
| Eight independently autonomous agents would be difficult for a student team to validate | Implement four integrated capabilities in one bounded LangGraph workflow, supported by deterministic services. |
| OCR and visual similarity can miss important defects | Combine text, geometry, barcode, and image checks; expose unsupported checks and uncertainty to reviewers. |
| A fluent draft can contain unsupported safety advice | Restrict drafting to approved facts, controlled terminology, and sufficient evidence; block unsupported changes. |
| Approval can become stale after a later edit | Bind decisions to immutable artifact and evidence manifests and require revalidation after changes. |
| An impressive demo can be mistaken for production readiness | Label synthetic data, simulated integrations, and unmeasured targets explicitly. |

## 3. Target users

| Persona | Main job | Required capability |
| --- | --- | --- |
| Labeling engineer / change owner | Submit an approved product-data change and prepare revised labels | Impact map, structured draft editor, artwork comparison, run status |
| Regulatory affairs reviewer | Evaluate evidence, applicability, and regulatory findings | Source viewer, evidence traceability, findings, regulatory decision |
| Quality reviewer | Check revision integrity and the complete review record | Version diff, validation report, quality decision, audit export |
| Localization reviewer, later phase | Review terminology and safety-critical translated content | Glossary, translation memory, source/target comparison |
| Administrator / knowledge steward | Manage access, templates, source versions, and rule sets | Role management, source ingestion, publication/revocation controls |
| Auditor / read-only observer | Reconstruct a change and its decisions | Read-only lineage, version history, downloadable permitted evidence |

The MVP supports one demonstration organization. Records and authorization still carry `organization_id` so the application does not depend on globally visible data. Full enterprise multitenancy and qualification workflows require later validation.

## 4. Product goals and boundaries

### Goals

1. Identify affected label revisions from an approved product revision and explicit dependencies.
2. Retrieve applicable evidence with document/version/page traceability and visible uncertainty.
3. Draft controlled label-field changes without introducing unsupported product or safety claims.
4. Compare baseline and proposed PDF artwork and present actionable findings.
5. Enforce regulatory and quality review against the exact artifacts being considered.
6. Export a reproducible demonstration package and a complete change history.

### Outside the MVP

- Live ERP/PLM, enterprise artwork, printing, or production-release integrations.
- Autonomous regulatory decisions or claims of FDA, EU, ISO, or other certification.
- Full global regulatory coverage, all medical-device classes, or all packaging formats.
- General-purpose graphic design, arbitrary desktop-publishing file editing, or unrestricted label layout generation.
- Qualified multilingual release, translation into 20+ languages, or machine translation accepted without review.
- Clinical records, patient data, medical advice, or sterilization-process qualification.
- Validated electronic signatures, enterprise disaster recovery, high availability, or a certified quality-management system.

## 5. Focused MVP

Use one synthetic device family, two device variants, two evidence contexts (US and EU), two packaging levels (pouch and carton), and English source content. This gives **eight demonstration labels** if every combination is represented. It does not establish that English satisfies any particular EU member state's language requirements.

Seed a small authorized corpus, approximately 5–10 selected documents or permitted excerpts. Include a curated dependency map, approved synthetic product revisions, approved text blocks, controlled templates, a baseline PDF per label, and an annotated expected-result set. Never fabricate a regulation to fill a fixture gap; synthetic policy examples must be visibly identified as company-demo rules.

### Four integrated capabilities

| Capability | MVP implementation | Visible outcome |
| --- | --- | --- |
| Change impact analysis | Deterministic dependency traversal with a constrained explanation node | Affected variants, labels, packaging levels, and evidence contexts with reasons |
| Regulatory intelligence and advanced RAG | Authorized document ingestion, hybrid BM25/vector retrieval, filtering, reranking, citations, and abstention | An evidence set with applicability and missing/conflicting-source flags |
| Grounded label drafting | Structured LLM output constrained by approved product facts, approved text, evidence, and a fixed template | Field-level proposed edits with provenance and a rendered PDF |
| Artwork validation | Text extraction/OCR, aligned image comparison, geometry checks, and barcode decoding | Annotated differences and classified findings against approved baseline artwork |

Compliance rules, versioning, review routing, audit recording, and package generation are shared application services. They do not require a separate LLM to perform authoritative decisions. The eight roles in the original brief remain the long-term responsibility model; [Architecture.md](Architecture.md) maps each role to its implementation.

## 6. Primary user journey

1. A labeling engineer selects an existing product and two approved product-data revisions.
2. The engineer creates a change request, for example a change to `sterilization.method`, and reviews the structured field diff. The system does not validate the underlying manufacturing change.
3. Moora identifies affected labels and explains the dependency path. Unknown mappings require clarification before the affected scope is finalized.
4. Moora retrieves and freezes relevant evidence for the selected product context, jurisdiction, and assessment date. Missing or conflicting mandatory evidence blocks drafting of the affected content.
5. Moora proposes field-level updates with source references. A user can edit a draft, creating a new immutable draft revision.
6. A deterministic renderer creates candidate PDFs from approved templates. Moora compares each candidate with its approved baseline and runs applicable deterministic checks.
7. Reviewers see the exact bundle of label revisions, PDFs, evidence versions, rule versions, and findings. They can approve, reject, or request changes.
8. Two distinct authorized people provide regulatory and quality decisions. Neither is the change submitter. Changes after a decision invalidate its effectiveness for the new bundle.
9. Once all gates pass, a user may generate an immutable **demo package**. There is no production-release endpoint in the MVP.

## 7. Functional requirements

| ID | Requirement | Acceptance condition |
| --- | --- | --- |
| FR-01 | Authenticate users and enforce organization and role boundaries | Unauthenticated, cross-organization, and unauthorized actions are rejected by the API, including direct requests. |
| FR-02 | Store product, template, and label revisions | Every run references immutable input revisions; historical revisions remain inspectable. |
| FR-03 | Accept typed change requests | Before/after revisions and changed fields are validated; ambiguous or unapproved inputs cannot proceed. |
| FR-04 | Determine and confirm impact | Every fixture label affected by the supplied dependency rules appears with its dependency path; scope changes are recorded. |
| FR-05 | Ingest controlled knowledge | Each usable source has authorization/provenance metadata, a content hash, version, extraction status, and an applicability review. |
| FR-06 | Perform advanced retrieval | Retrieval records BM25/vector candidates, metadata filters, reranking, selected chunks, and their immutable source locations. |
| FR-07 | Abstain when evidence is insufficient | Unanswerable, conflicting, obsolete, or unauthorized-evidence cases return a blocking status and reason, not an invented answer. |
| FR-08 | Produce grounded structured drafts | Each changed field references approved product facts and/or evidence; unsupported safety wording is rejected or escalated. |
| FR-09 | Render and validate artwork | Each candidate is rendered from a versioned template; OCR, text, geometry, and barcode findings link to the relevant artifact and region. |
| FR-10 | Apply versioned compliance checks | Findings distinguish deterministic violations, evidence concerns, advisory AI observations, and checks not evaluated. |
| FR-11 | Coordinate human decisions | Regulatory and quality decisions bind to the exact review manifest; unauthorized, duplicate, stale, and self-approval attempts fail. |
| FR-12 | Export a controlled demo package | Export requires the current approved manifest and passing gates; package includes artifacts, evidence references, checks, decisions, and hashes. |
| FR-13 | Preserve audit history | Significant user, agent, and service events are append-only through the application and correlated to actors, inputs, outputs, and run IDs. |
| FR-14 | Recover long-running work | Retries, worker restarts, duplicate deliveries, and human pauses do not create duplicate versions, approvals, or packages. |
| FR-15 | Expose operational status | Users can distinguish queued, running, waiting, blocked, failed, stale, and completed work and take the permitted recovery action. |

## 8. Review and packaging policy

- Approval authority belongs to server-validated human identities with the required role and active reviewer qualification record. A label on an AI-generated report is not an approval.
- A review bundle contains an exact artifact manifest. There must be no implicit approval of a mutable "latest" label.
- One regulatory reviewer and one quality reviewer must approve; they must be different people and must not be the submitter.
- Critical and major findings block approval. Missing required evidence, incomplete required checks, unsupported required formats, stale inputs, and insufficient OCR quality also block approval regardless of severity display.
- Minor/advisory findings require explicit reviewer disposition. They may be acknowledged with a rationale; this cannot waive a blocking condition.
- A revision to product data, label content, artwork, templates, applicable evidence, rules, or review membership requires a new or revalidated bundle. Prior decisions remain in history.
- Packaging rechecks these predicates transactionally. A successful earlier UI view is not sufficient authorization.
- The MVP package is watermarked **Demonstration — not for production use**. Production publishing is absent.

## 9. Quality attributes and constraints

| Area | Requirement |
| --- | --- |
| Traceability | Every proposed edit, finding, decision, and exported artifact must be traceable to immutable inputs. |
| Security | Least privilege, private object storage, scoped signed downloads, safe uploads, redacted logs, and server-side access checks. |
| Reliability | Durable workflow checkpoints, idempotent side effects, bounded retries, and visible failures. |
| Usability | Evidence, changes, and blocking reasons must be accessible from the review screen without reconstructing the workflow manually. |
| Accessibility | Target WCAG 2.2 AA; keyboard access, visible focus, sufficient contrast, and text alternatives to visual diffs. This is a target, not a certification. |
| Resource use | Start with a modular backend and low worker concurrency suitable for a student project; record actual runtime and API costs. |
| Data governance | Use public or properly licensed sources. Source text must not be sent to external model providers unless its rights and organization policy permit it. |
| Explainability | Show source references, rule outcomes, and known limitations. Do not present model confidence as proof of correctness. |

## 10. Validation and success metrics

All numbers below are **proposed acceptance targets**. No results, time savings, or quality improvements have been measured yet. Freeze the fixture set before evaluating and report denominators and exceptions.

| Measure | Method | Initial target |
| --- | --- | --- |
| Impact precision and recall | Compare affected label IDs with the curated fixture truth set | 100% on the eight-label demo fixtures; no claim about unseen product graphs |
| Retrieval relevance | At least 20 answerable queries with independently annotated relevant passages | Recall@10 ≥ 0.90 on the frozen evaluation set |
| Citation resolution | Resolve every emitted citation to its stored document version and location | 100% valid references |
| Citation support | Human reviewers assess whether each cited passage supports its linked claim | Precision ≥ 0.95 on the evaluation set; identified unsupported safety claims always block review |
| Abstention | At least 10 unanswerable, conflicting, outdated, or unauthorized-source cases | All mandatory-evidence failure fixtures block dependent content |
| Draft grounding | Compare changed fields against approved facts, text blocks, and evidence | No unsupported safety-critical statement accepted in the demo fixtures |
| Artwork detection | At least 12 annotated comparisons covering text, symbol, layout, and barcode defects | All seeded critical defects detected; report misses and false positives separately |
| Human gate enforcement | Attempt self-approval, wrong-role approval, stale approval, and direct export | All prohibited paths rejected |
| Audit completeness | Reconstruct each packaged change from its manifest | All required lineage links present and hashes verifiable |
| Recovery | Interrupt a run and redeliver a job | One effective output per idempotency key; no duplicate decisions or packages |
| Cycle time | Compare manual and assisted completion of the same task, recording human and compute time separately | Establish a baseline before claiming improvement |
| Cost and latency | Record per-node duration, retries, tokens, and external API cost | Report actual values and hardware/provider conditions; set budgets after the first baseline |

Passing a small synthetic evaluation set does not establish clinical safety, market compliance, production validation, or generalization to all devices.

## 11. Demonstration and differentiation

The primary demo follows one approved sterilization-field change from impact analysis to an explicitly approved demo package. Include a negative-path demonstration: remove required evidence or edit a reviewed artifact, then show that the workflow blocks and explains the next action.

The differentiating features are a **change-to-label dependency map**, **evidence-to-edit lineage**, **visual findings tied to actual rendered artifacts**, and **approvals that become ineffective when their inputs change**. These are feasible and inspectable. A presentation should show the evidence behind each result and disclose simulated data sources.

## 12. Dependencies and unresolved decisions

- Confirm the preferred public-facing name, final demo device fixture, and team delivery dates.
- Choose an authorized source corpus and record its licensing, versions, and applicability decisions.
- Choose a model/embedding provider, account budget, data-sharing policy, and reranker model; keep integrations configurable.
- Confirm template, symbol, and font usage rights. Do not copy ISO content or symbol artwork without permission.
- Confirm reviewer policy with a qualified domain reviewer before extending beyond a prototype.
- Cloud vendor and deployment region remain open; the architecture uses portable interfaces.

These decisions do not block creating the project documents or scaffolding. Missing provider access or source rights may block a live-AI or regulatory demonstration and must be reported explicitly.
