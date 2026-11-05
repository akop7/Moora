# Moora — Product and Visual Design

Status: design specification for the proposed MVP; no UI has been implemented.

Product context: [PRD.md](PRD.md). Contracts and state: [Architecture.md](Architecture.md).

## 1. Design direction

Design Moora as a calm, precise workspace for people reviewing consequential document changes. The primary visual task is comparing versions and inspecting evidence. Prioritize readable text, clear hierarchy, generous workspace, and explicit status over decorative AI imagery.

Use **Moora** in the navigation and **SmartLabel AI** as a secondary descriptor on the login screen, overview, and presentation. Credit **Part Time Humans** in the project/about area. This follows the provisional naming decision in the PRD.

The MVP uses a light workspace with a dark navy navigation rail and restrained teal actions. Dark mode is deferred until document viewing, finding overlays, and status contrast can be tested in both themes.

## 2. Design principles

1. **Evidence is one action away.** Clicking a citation opens the actual source version and location, not a generic search page.
2. **Show what changed.** Place the approved baseline, proposed revision, and field-level reasons together.
3. **Make uncertainty actionable.** Explain missing evidence, stale inputs, failed checks, and the next permitted action.
4. **Distinguish progress from approval.** A completed workflow is not a reviewed label. Never use one green badge for both.
5. **Keep consequential actions deliberate.** Approval confirms a visible manifest/version and records the reviewer's decision and rationale.
6. **Preserve context.** Returning from a source or finding should restore the selected page, artifact, filter, and comparison mode.
7. **Use honest data.** Empty states and synthetic fixtures are preferable to invented compliance scores, savings, or success percentages.

## 3. Color tokens

Use semantic tokens so statuses and interactions remain consistent. Color is never the only indicator.

| Token | Value | Use |
| --- | --- | --- |
| `canvas` | `#F8FAFC` | Main application background |
| `surface` | `#FFFFFF` | Cards, tables, dialogs, document panels |
| `surface-muted` | `#F1F5F9` | Secondary regions and alternating context |
| `text-primary` | `#0F172A` | Headings and primary content |
| `text-secondary` | `#475569` | Supporting text, metadata, form help |
| `border` | `#CBD5E1` | Decorative table/card separators |
| `control-border` | `#64748B` | Input boundaries where a visible boundary is necessary |
| `nav-background` | `#0F172A` | Desktop navigation rail |
| `nav-text` | `#E2E8F0` | Navigation text on navy |
| `primary` | `#0F766E` | Main actions and selected controls; white button text |
| `primary-hover` | `#115E59` | Primary action hover/pressed state |
| `accent` | `#2DD4BF` | Small accents on dark navigation, not white-text button backgrounds |
| `focus` | `#2563EB` | Keyboard focus ring with an offset from the component |
| `info-text` / `info-bg` | `#1D4ED8` / `#EFF6FF` | Running/information notices |
| `warning-text` / `warning-bg` | `#92400E` / `#FFFBEB` | Review needed, uncertain input, minor findings |
| `danger-text` / `danger-bg` | `#B91C1C` / `#FEF2F2` | Blocking findings, rejected actions, errors |
| `success-text` / `success-bg` | `#166534` / `#F0FDF4` | Successful checks and explicitly approved demo states |

Target at least 4.5:1 contrast for normal text and 3:1 for large text and essential non-text UI boundaries. Validate actual rendered combinations, including disabled, focus, hover, and overlay states. The lighter decorative border token is not sufficient by itself for every interactive control boundary.

Specification check on 2026-11-02: eleven base text, button, status, focus, and control-border color pairings were calculated and met their respective thresholds. This does not validate rendered components, document overlays, or all interaction states.

Do not reuse danger red for ordinary deleted text without a legend; document differences and validation severity are separate concepts.

## 4. Typography, spacing, and shape

### Typography

Use locally hosted **Inter** if the selected asset's license is retained; otherwise use `system-ui, -apple-system, BlinkMacSystemFont, "Segoe UI", sans-serif`. Use the system monospace stack for hashes, IDs, and machine-readable values. The interface must remain usable if custom fonts fail to load.

| Style | Size / line height | Weight | Use |
| --- | --- | --- | --- |
| Page title | 28 / 36 px | 600 | One main page heading |
| Section title | 20 / 28 px | 600 | Major panels and dialogs |
| Card/subsection title | 16 / 24 px | 600 | Table sections and panel titles |
| Body | 14 / 22 px | 400 | Default application content |
| Long-form evidence | 16 / 26 px | 400 | Source passages and detailed rationale |
| Label/button | 14 / 20 px | 500–600 | Fields, actions, navigation |
| Metadata | 12 / 18 px | 400–500 | Secondary dates, versions, hashes |

Do not shrink warnings or safety-critical differences into metadata text. Regulatory source formatting can be preserved inside the viewer without imposing its typography on the entire application.

### Layout tokens

- Spacing scale: 4, 8, 12, 16, 24, 32, 48 px.
- Standard control height: 40 px; primary touch targets should be at least 44 × 44 px where practical.
- Card radius: 12 px. Inputs/buttons: 8 px. Status pills: fully rounded.
- Use a subtle shadow only for overlays, menus, and raised panels; ordinary tables can rely on borders.
- Default page padding: 24 px desktop, 16 px tablet/mobile.
- Desktop navigation: 232 px; top application bar: 64 px.
- Reading forms: approximately 960 px maximum width. Comparison workspaces can use the available width.

## 5. Navigation and page structure

| Route | Screen | Main content and primary action |
| --- | --- | --- |
| `/login` | Sign in | Credentials, session errors, product identity, demonstration context |
| `/app` | Overview | Actual change/review counts, recent work, blockers; **New change request** |
| `/app/products` | Product library | Products, variants, approved revisions, scope filters |
| `/app/products/:productId` | Product detail | Revision data, label relationships, controlled template links |
| `/app/changes` | Change requests | Status, owner, affected labels, last event, and review readiness |
| `/app/changes/:changeId` | Change workspace | Scope, evidence, draft, validation, review, and history |
| `/app/knowledge` | Knowledge library | Sources, versions, rights/access status, ingestion and publication state |
| `/app/reviews` | Review queue | Assigned role, manifest version, blockers, and decision status |
| `/app/reviews/:bundleId` | Review workspace | Artifact comparison, findings, evidence, and decision panel |
| `/app/audit` | Audit history | Actor/event filters, lineage, permitted report/package downloads |
| `/app/settings` | Administration | Membership, reviewer scopes, templates, and permitted configuration |

Hide irrelevant actions for convenience, but enforce permissions on the server. Unauthorized deep links show an appropriate access state. Do not expose restricted document titles or object metadata through navigation suggestions.

### Main application shell

```text
┌────────────────┬─────────────────────────────────────────────────┐
│ Moora          │ Breadcrumbs          Organization    Current user│
│ SmartLabel AI  ├─────────────────────────────────────────────────┤
│                │ Page title          Status          Primary action│
│ Overview       │ Context / filters / current revision             │
│ Products       ├─────────────────────────────────────────────────┤
│ Changes        │                                                 │
│ Knowledge      │ Main workspace                                  │
│ Reviews        │                                                 │
│ Audit          │                                                 │
│ Settings       │                                                 │
└────────────────┴─────────────────────────────────────────────────┘
```

Show a restrained persistent banner in demo mode: **Synthetic demonstration data · Human review required**. Generated packages carry their own **Demonstration — not for production use** marking. Do not imply the demo banner substitutes for approval controls.

## 6. Core screen specifications

### Overview

Use four factual summaries: active change requests, affected labels under review, blocking findings, and assigned review tasks. Each opens a filtered list. Use real counts from the API; show an empty state when no data exists.

The main area contains recent changes, their current stage, and the next action. Do not display an overall "compliance percentage" or estimated money saved without a defined, measured method.

### Change request creation

Use a short form: product, approved baseline revision, approved target revision, and change reason. Show the derived field diff before submission. Mark product data as synthetic where applicable.

The impact result lists variant, evidence context, packaging level, language, current label revision, and the dependency path. A graph may supplement the table, but the table provides the full keyboard-accessible meaning.

### Change workspace

Use the stages **Scope → Evidence → Draft → Validate → Review → Demo package**. Every stage shows a text status and any blocking reason. Keep a summary header with change ID, product, revision pair, owner, and last meaningful event.

The draft view pairs structured field edits with provenance. An edit's previous value, proposed value, source links, and explanation remain visible. Saving an edit states that a new revision will invalidate downstream checks where applicable.

### Evidence library and drawer

The library distinguishes uploaded, extracting, needs review, published, superseded, and failed source versions. Show rights restrictions and extraction quality without displaying confidential source text to unauthorized users.

An evidence drawer contains document title, issuer, exact version, PDF page and optional printed page label, passage, source region, applicability rationale, assessment date, and linked label fields/findings. Show **Insufficient evidence**, **Conflicting sources**, or **Input needed** as first-class states.

Do not replace evidence with a generated summary only. Users must be able to inspect the permitted original passage.

### Artwork and review workspace

At wide widths, use a comparison area and a findings/decision panel. The comparison area shows baseline and candidate PDFs with synchronized page selection and optional synchronized zoom. Each side displays its revision and hash abbreviation.

Controls: side-by-side/overlay mode, page selection, zoom, finding filters, and a textual difference list. Selecting a finding focuses its page/region and opens its explanation and evidence. Coordinate transforms must keep overlays aligned across zoom levels.

Classify findings by type (text, symbol, barcode, geometry, evidence, rule) and severity separately. Overlays use labeled outlines and patterns as well as color. Preserve enough original artwork to make the underlying difference visible.

The decision panel shows required regulatory/quality decisions, reviewer eligibility, unresolved blockers, current manifest hash, and already-recorded decisions. Approval is disabled with a concrete explanation when the policy is not met. API policy remains authoritative if the page becomes stale.

### Approval interaction

1. The reviewer inspects the complete bundle and relevant findings/evidence.
2. Selecting **Approve for demo**, **Request changes**, or **Reject** opens a concise decision dialog.
3. The dialog identifies the bundle revision, number of included artifacts, required role, and exact decision. Record a rationale for every decision.
4. The submission includes the current manifest/version token. A `409` response displays **This bundle changed. Reload and review the current version.**
5. On success, show the actor, role, decision time, and approved manifest; refresh effective bundle status from the server.

Do not auto-submit an approval after generation or preselect approval in a way that could be mistaken for an existing decision. The MVP's recorded decision is not presented as a validated electronic signature.

### Audit and export

Use a chronological event list with actor type (human/agent/service), entity revision, action, time, correlation ID, and permitted source links. Provide filters without hiding the relationship between related events.

An export panel shows package eligibility, manifest version, included artifacts, source-rights restrictions, and required decisions. Use **Generate demo package** and **Download demo package**, never **Publish to production**.

## 7. Status and finding vocabulary

| State | Text and treatment |
| --- | --- |
| Draft | Neutral; work has not completed its checks |
| Queued / Running | Blue information style; current step and elapsed time |
| Waiting for input | Amber; name the missing fact or mapping |
| Blocked | Red; specific prerequisite and permitted recovery |
| Waiting for review | Amber; outstanding roles and current manifest |
| Changes requested | Amber; reviewer rationale and successor revision action |
| Rejected | Red; recorded decision, no implied retry approval |
| Stale / Revalidation required | Amber with a distinct refresh icon; name the changed dependency |
| Approved for demo | Green; exact version and effective human decisions visible |
| Packaged for demo | Green/neutral; immutable package and approval manifest |
| Failed | Red; safe error, correlation ID, and retry only when permitted |
| Not evaluated | Neutral/amber with explicit text; never rendered as passed |

Keep finding severity (critical, major, minor, advisory) separate from execution status and approval state. Green **Check passed** refers only to the named check. No badge should say **Certified compliant**.

## 8. Loading, empty, error, and concurrency states

- For ordinary data loads, use skeletons matching the final layout; keep accessible loading announcements concise.
- For background work, show completed/current steps and elapsed time. Do not fabricate a percentage if work units are unknown.
- An empty product/source/review page explains the required next action for the user's role.
- Retain unsaved form input after validation or network errors. Map field errors beside their fields and provide a summary for keyboard users.
- A source or provider failure shows a readable reason and safe correlation ID. Never display raw stack traces or credentials.
- Stale content is clearly marked. Refreshing should not silently discard a user's edits or reinterpret an earlier approval.
- Distinguish missing source access from an empty evidence result. Do not disclose restricted source details in an error.
- Preserve the user's selected document page and finding when status polling updates surrounding metadata.

## 9. Responsive and accessible behavior

- Optimize full comparison work for 1280 px and wider. At 1024–1279 px, use collapsible navigation and a resizable details panel.
- Below 1024 px, show one document at a time with an explicit baseline/candidate switch and a synchronized textual diff. Keep findings and decisions reachable.
- On narrow screens, convert dense tables to labeled rows/cards or provide deliberate horizontal scrolling with persistent identifying fields.
- Use semantic headings, landmarks, table headers, field labels, and buttons. Give icon-only buttons accessible names.
- Support keyboard navigation, a skip link, visible focus, focus trapping/restoration in dialogs, and Escape behavior where appropriate.
- Announce meaningful workflow status changes with restrained live regions; do not repeatedly announce every poll.
- Provide text descriptions for image findings and access to extracted text where permitted. Never make canvas overlays the only review mechanism.
- Respect reduced-motion preferences; use brief 120–180 ms transitions and avoid decorative looping animation.
- Test at 200% zoom and with keyboard-only navigation; include a screen-reader review of the primary approval flow.

## 10. Reusable component inventory

Use a small shared set: `AppShell`, `PageHeader`, `StatusBadge`, `FindingSeverity`, `DataTable`, `RevisionSelector`, `FieldDiff`, `ImpactTable`, `EvidenceDrawer`, `DocumentViewer`, `ComparisonToolbar`, `FindingList`, `RunTimeline`, `ReviewDecisionDialog`, `AuditTimeline`, `EmptyState`, and `ErrorState`.

Keep business policy in API/domain services. Components render the server's permitted actions and current state; they do not create independent approval logic.

## 11. Design acceptance checks

- A reviewer can move from a proposed edit to its source and back without losing place.
- Baseline and candidate versions cannot be confused, even when screenshots or exports are viewed separately.
- Blocking states explain a concrete next action and cannot be mistaken for a successful check.
- All core flows work by keyboard, status is understandable without color, and actual contrast is measured.
- No screen invents performance figures, certifies compliance, or disguises fixture replay as live AI.
- A stale review is rejected visibly and cannot be submitted against a newly changed artifact.
- Mobile/tablet views preserve access to evidence and decisions even when comparison layout changes.

These checks are planned UI acceptance criteria. Visual polish alone does not satisfy the workflow's security, evidence, or approval requirements.
