# Frontend Build Sequence Sprint Plan

## 1. Purpose
This file converts the frontend planning package into an execution-ready build order.

It defines:
- what to build first
- dependency order between frontend layers
- what can be parallelized
- what backend support is needed at each stage
- handoff checkpoints between design, frontend, and backend
- sprint-style implementation sequencing for the first ERP vertical slice

This is the bridge between planning and actual delivery.

---

## 2. Recommended Folder Location
Keep this file at:

`frontend/frontend_build_sequence_sprint_plan.md`

---

## 3. Scope Covered
This sprint plan currently covers the first completed ERP vertical slice:

- Procurement
  - PR List
  - PR Detail
  - PO List
  - PO Detail
  - GRN Entry
  - GRN Detail
  - Incoming QC
- Inventory
  - Stock Overview

---

## 4. Execution Principles
### Build the platform before pages
Do not start with business pages before route shell, API client, permissions, and shared components exist.

### Build by vertical flow, not random screen order
Follow the business chain:
PR → PO → GRN → QC → Stock

### Prioritize stable shared pieces early
Headers, tables, dialogs, status badges, and route guards should come before advanced page-specific components.

### Avoid backend/frontend disconnect
Each sprint should identify required backend endpoints before the frontend page is considered truly ready.

### Deliver usable slices
Every sprint should create something testable, not only technical scaffolding.

---

## 5. Recommended Sprint Structure Overview

### Sprint 0
Foundation and scaffolding

### Sprint 1
Shared UI + routing + permission infrastructure

### Sprint 2
PR flow implementation

### Sprint 3
PO flow implementation

### Sprint 4
GRN flow implementation

### Sprint 5
Incoming QC implementation

### Sprint 6
Stock Overview implementation

### Sprint 7
Integration hardening + cross-page workflow polish

---

# 6. Sprint 0 – Foundation Setup

## Objective
Prepare the codebase so feature work can begin cleanly.

## Frontend Tasks
- set up app shell structure
- set up router base
- set up providers
- set up API client
- set up auth/session handling placeholder
- set up shared folder/module folder scaffolding
- set up query library integration
- set up base styling/theme/layout system
- define route metadata pattern
- define basic permission helper pattern

## Shared Output
- running application shell
- sidebar/topbar placeholders
- route composition base
- module folder structure committed
- API layer base committed

## Backend Dependency
Minimal:
- auth/session placeholder or mock
- API contract agreement for envelope/error structure

## Completion Checkpoint
Sprint 0 is complete when:
- app boots in browser
- route shell works
- module folders are ready
- API client is usable
- query provider is active

---

# 7. Sprint 1 – Shared UI and Core Infrastructure

## Objective
Create the reusable UI and behavior foundation needed by all pages.

## Frontend Tasks
Build shared components:
- PageHeader
- FilterBar
- DataTable
- StatusBadge
- ConfirmDialog
- EmptyState
- ErrorState
- LoadingSkeleton
- Breadcrumbs
- RecordStatusHeader
- ActionBar
- LinkedRecordsPanel
- RejectReasonModal
- CommentThreadPanel
- AttachmentPanel
- AuditTrailPanel

Build shared behavior:
- permission helpers
- route guards
- query key conventions
- generic error handling pattern
- common table pagination/filter behavior

## Parallel Work
Can run in parallel with backend endpoint stabilization for PR/PO.

## Backend Dependency
- stable response envelope
- comments/attachments/audit API pattern agreement
- permission payload strategy agreement

## Completion Checkpoint
Sprint 1 is complete when:
- shared page skeletons can be assembled quickly
- action dialogs and panels exist
- route guarding works
- comments/attachments/audit components can render mock/live data

---

# 8. Sprint 2 – Purchase Requisition (PR) Flow

## Objective
Implement the first full workflow slice:
PR List + PR Detail

## Frontend Tasks
### PR List
- build list page
- integrate summary strip
- integrate search/filter/sort/pagination
- row actions by role/status
- submit/approve/reject/cancel actions
- preserve list filter state

### PR Detail
- build detail header and summary
- line item view/edit (draft only)
- approval timeline
- comments
- attachments
- audit panel
- linked PO section
- submit/approve/reject/cancel flow

## Shared/Reusable Components Extended
- PR-specific line item presentation
- PR linked records block
- form validation summary if edit mode exists

## Backend Dependency
Required endpoints:
- PR list
- PR summary
- PR detail
- PR submit/approve/reject/cancel
- comments
- attachments
- audit log
- linked POs

## Testing Focus
- maker/checker visibility
- status transition rendering
- list/detail consistency
- invalid action hiding/disabling

## Completion Checkpoint
Sprint 2 is complete when:
- PR flow is testable end-to-end
- Procurement Executive and Procurement Manager roles behave correctly
- approval actions refresh UI correctly

---

# 9. Sprint 3 – Purchase Order (PO) Flow

## Objective
Implement:
PO List + PO Detail

## Frontend Tasks
### PO List
- build list page
- filters/search/sort
- summary strip
- approval action handling
- linked GRN indicators

### PO Detail
- source PR reference
- vendor/commercial summary
- totals summary
- line items
- approval controls
- comments/attachments/audit
- linked GRN section
- receipt progress widget
- downstream Create GRN action visibility

## Shared/Reusable Components Extended
- SourceReferenceCard
- VendorCommercialSummary
- TotalsSummaryCard
- ReceiptProgressWidget

## Backend Dependency
Required endpoints:
- PO list
- PO summary
- PO detail
- PO submit/approve/reject/cancel/close
- comments
- attachments
- audit log
- linked GRNs
- receipt progress

## Testing Focus
- PR → PO traceability
- role-based approval controls
- downstream GRN creation visibility
- linked record refresh after GRN creation

## Completion Checkpoint
Sprint 3 is complete when:
- PO workflow is fully usable
- source PR linkage is visible
- receipt progress/linked GRNs refresh correctly

---

# 10. Sprint 4 – GRN Flow

## Objective
Implement:
GRN Entry + GRN Detail

## Frontend Tasks
### GRN Entry
- source PO prefill
- receivable line item grid
- warehouse/bin selection
- qty validation
- batch/serial input handling if needed
- Save Pending / Mark Received
- Send to QC handling
- attachment support if required during entry

### GRN Detail
- receipt summary
- source PO reference
- line item qty breakdown
- QC/disposition view
- comments/attachments/audit
- stock impact panel
- linked inventory/QC references

## Shared/Reusable Components Extended
- ReceiptSummaryCard
- QCDispositionPanel
- StockImpactPanel
- GRN-specific line item grid mode

## Backend Dependency
Required endpoints:
- GRN prefill context
- create GRN
- update pending GRN
- mark received
- send to QC
- accept/reject if combined in GRN detail
- comments
- attachments
- audit
- stock impact
- linked inventory/QC references

## Testing Focus
- approved PO → GRN entry flow
- quantity validation
- pending vs received vs QC pending behavior
- stock impact visibility after acceptance

## Completion Checkpoint
Sprint 4 is complete when:
- receipt flow from PO to GRN is usable
- quantity rules are enforced
- linked PO and stock-impact traceability works

---

# 11. Sprint 5 – Incoming QC

## Objective
Implement:
Incoming QC queue / action page

## Frontend Tasks
- build QC queue/list
- build inspection action panel
- accepted/rejected quantity inputs
- QC notes/evidence
- accept/reject actions
- refresh linked GRN state after QC result

## Shared/Reusable Components Extended
- InspectionActionPanel
- evidence upload variant
- QC queue table config

## Backend Dependency
Required endpoints:
- QC pending list / dedicated queue
- QC result submit
- GRN/QC linked context
- evidence upload
- optional QC comments

## Testing Focus
- QA-only action visibility
- accepted + rejected quantity reconciliation
- GRN → QC → stock refresh chain
- rejection reason enforcement

## Completion Checkpoint
Sprint 5 is complete when:
- QA can process pending receipts
- QC results update GRN/stock state correctly
- non-QA roles cannot perform QC actions

---

# 12. Sprint 6 – Stock Overview

## Objective
Implement:
Stock Overview page

## Frontend Tasks
- summary strip
- stock table
- filters/search
- stock-state badges
- drilldown drawer or linked movement view
- source GRN linkage
- QC hold / rejected / available state visibility

## Shared/Reusable Components Extended
- StockStatusBadge
- StockDrilldownDrawer
- stock summary strip composition

## Backend Dependency
Required endpoints:
- stock overview
- stock summary
- stock drilldown or movement list
- low-stock indicators if applicable

## Testing Focus
- accepted GRN shows in stock
- QC hold stock separated clearly
- rejected stock not mixed into usable stock
- source traceability to GRN works

## Completion Checkpoint
Sprint 6 is complete when:
- stock is visible by state
- drilldown works
- procurement/QC/warehouse traceability is visible

---

# 13. Sprint 7 – Integration Hardening and Workflow Polish

## Objective
Stabilize the complete vertical slice.

## Frontend Tasks
- end-to-end workflow testing
- loading/empty/error/no-access polish
- permission edge-case cleanup
- deep link and return/back behavior polish
- comments/attachments/audit consistency checks
- query invalidation cleanup
- responsive/scroll/long-table behavior polish
- cross-page linked record polish
- user guidance / confirmation messaging cleanup

## Backend Dependency
- endpoint consistency fixes
- validation message cleanup
- response shape normalization
- linked-record metadata completeness
- performance tuning for list/detail pages

## Testing Focus
- PR → PO → GRN → QC → Stock full flow
- role switching behavior
- stale data prevention
- invalid workflow transition handling
- list/detail filter persistence
- direct deep-link entry

## Completion Checkpoint
Sprint 7 is complete when:
- the first ERP vertical slice is stable enough for UAT/internal pilot
- workflow and permissions behave consistently
- no critical integration gaps remain

---

# 14. Parallel Work Recommendation

## Can Run in Parallel
### Frontend
- shared component work
- route/guard work
- page implementation after API contracts are stable
- module-specific visual composition

### Backend
- endpoint scaffolding
- comments/attachments/audit shared APIs
- PR and PO endpoints in parallel
- GRN/QC endpoints once PO contracts are stable
- stock APIs once GRN/QC posting model is stable

## Must Be Sequenced Carefully
- PR before PO full completion
- PO before GRN prefill flow
- GRN/QC before stock visibility correctness
- permission matrix before final action rendering
- route map before full navigation polish

---

# 15. Suggested Team Handoff Model

## Design / Product
Hands off:
- page specs
- transaction logic
- role logic
- connection logic

## Backend
Hands off:
- endpoint contracts
- validation/error behavior
- permission payload decisions
- linked-record metadata support

## Frontend
Builds:
- shared components
- pages
- route integration
- state management
- mutation handling
- page-to-page UX consistency

## QA / Internal Review
Validates:
- workflow correctness
- permission correctness
- data refresh behavior
- state transitions
- traceability

---

# 16. Readiness Checklist Before Each Sprint
Before starting any sprint, confirm:
- page spec exists
- role logic for that page exists
- transaction logic exists
- route and permission rules are known
- API contract is available or mocked
- refresh dependencies are known
- acceptance criteria are defined

---

# 17. Recommended Acceptance Definition Per Page
A page is “done” only when:
- it renders correctly
- role visibility is correct
- workflow actions are correct
- loading/empty/error states exist
- comments/attachments/audit work if required
- linked records are visible if required
- refresh behavior is correct after mutations
- route/deep link access works

---

# 18. Suggested Immediate Build Order Inside Engineering
If engineering starts tomorrow, the best micro-order is:

1. app shell
2. route map implementation
3. API client and query provider
4. permission helper layer
5. shared page/table/dialog/status components
6. PR List
7. PR Detail
8. PO List
9. PO Detail
10. GRN Entry
11. GRN Detail
12. Incoming QC
13. Stock Overview
14. integration hardening

---

# 19. Completion Criteria
This file is complete when:
- sprint order is defined
- dependency order is defined
- backend/frontend coordination points are clear
- page completion criteria are clear
- the first build roadmap is executable

---

# 20. Next Related Files
Recommended next files:
1. backend_endpoint_priority_plan.md
2. frontend_module_bootstrap_checklist.md
3. coding_standards_frontend.md
4. first_vertical_slice_delivery_checklist.md
