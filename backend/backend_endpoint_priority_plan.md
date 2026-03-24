# Backend Endpoint Priority Plan

## 1. Purpose
This file defines the backend API build order for the first ERP vertical slice so backend implementation can move in the same sequence as frontend delivery.

It standardizes:
- which endpoints must be built first
- shared vs module-specific API priorities
- endpoint dependency order
- minimum viable payload strategy
- what frontend can mock temporarily
- what backend must deliver before a sprint can be considered unblocked

This is the backend execution roadmap that matches the frontend sprint plan.

---

## 2. Recommended Folder Location
Keep this file at:

`backend/backend_endpoint_priority_plan.md`

If you do not yet have a `backend/` folder in planning, create one for implementation-level backend delivery documents.

---

## 3. Scope Covered
This plan currently covers the first ERP vertical slice:

- Procurement
  - Purchase Requisition (PR)
  - Purchase Order (PO)
  - Goods Receipt Note (GRN)
- Quality
  - Incoming QC
- Inventory
  - Stock Overview / Inventory Movement
- Shared APIs
  - auth/context
  - comments
  - attachments
  - audit log
  - linked-record support

---

## 4. Endpoint Priority Principles

### Build shared platform APIs early
Frontend cannot integrate smoothly without:
- auth/session context
- response envelope consistency
- common error model
- comments/attachments/audit support pattern

### Build by business flow, not isolated tables
Follow the actual operational chain:
PR → PO → GRN → QC → Stock

### Prefer minimum viable page payloads first
Do not wait for perfect final contracts if a stable minimum page payload can unblock UI integration.

### Separate “must-have to render” from “nice-to-have enrichment”
For each page/API group, define:
- blocking endpoints
- enhancement endpoints

### Keep workflow actions explicit
Submit/approve/reject/cancel/accept/close should be dedicated endpoints, not hidden in generic patch behavior.

---

## 5. Backend Delivery Phases Overview

### Phase 0
Shared backend foundation

### Phase 1
PR endpoints

### Phase 2
PO endpoints

### Phase 3
GRN endpoints

### Phase 4
Incoming QC endpoints

### Phase 5
Stock overview + inventory movement endpoints

### Phase 6
Cross-page polish, linked-record enrichment, performance cleanup

---

# 6. Phase 0 – Shared Backend Foundation

## Objective
Deliver the common backend capabilities required before page integration begins.

## Must-Have Endpoints / Capabilities
### Authentication / Current User Context
- `POST /api/v1/auth/login`
- `POST /api/v1/auth/refresh`
- `POST /api/v1/auth/logout`
- `GET /api/v1/auth/me`

### Shared Response / Error Standards
Not endpoints, but must be implemented consistently:
- success envelope
- list pagination envelope
- validation error shape
- authorization error shape
- invalid transition error shape

### Shared Comments APIs
Pattern options:
- `GET /api/v1/{module}/{id}/comments`
- `POST /api/v1/{module}/{id}/comments`

### Shared Attachments APIs
- `POST /api/v1/attachments`
- `GET /api/v1/{module}/{id}/attachments`
- `GET /api/v1/attachments/{id}`

### Shared Audit APIs
- `GET /api/v1/{module}/{id}/audit-log`

### Suggested Priority
1. `GET /api/v1/auth/me`
2. shared error/response model
3. comments pattern
4. attachments pattern
5. audit log pattern

## Frontend Mock Tolerance
Frontend can mock:
- comments
- attachments
- audit log

Frontend should not have to mock long-term:
- auth/me
- error model
- permission/context basics

## Phase 0 Completion Criteria
- frontend can identify current user
- response/error envelope is stable
- comments/attachments/audit contract pattern is agreed
- route guards and permission-aware UI can begin

---

# 7. Phase 1 – PR Endpoints

## Objective
Unblock PR List and PR Detail implementation.

## Blocking Endpoints
### List
- `GET /api/v1/purchase-requests`
- `GET /api/v1/purchase-requests/summary`

### Detail
- `GET /api/v1/purchase-requests/{id}`

### Workflow Actions
- `POST /api/v1/purchase-requests/{id}/submit`
- `POST /api/v1/purchase-requests/{id}/approve`
- `POST /api/v1/purchase-requests/{id}/reject`
- `POST /api/v1/purchase-requests/{id}/cancel`

### Draft Management
- `POST /api/v1/purchase-requests`
- `PATCH /api/v1/purchase-requests/{id}`

### Supporting Endpoints
- `GET /api/v1/purchase-requests/{id}/comments`
- `POST /api/v1/purchase-requests/{id}/comments`
- `GET /api/v1/purchase-requests/{id}/attachments`
- `GET /api/v1/purchase-requests/{id}/audit-log`
- `GET /api/v1/purchase-requests/{id}/linked-purchase-orders`

## Minimum Payload Requirements
### PR List must return
- id
- request_no
- request_date
- requester
- department
- status
- total_estimated_amount
- required_by_date
- linked_po_count or indicator
- last_updated
- enough status/action context for frontend rendering

### PR Detail must return
- header
- line items
- workflow status
- requester/department/branch
- purpose/justification
- action eligibility or enough role/status context to compute it

## Frontend Mock Tolerance
Can temporarily mock:
- linked-purchase-orders
- comments/attachments/audit
- some summary enrichments

Cannot practically mock for long:
- list
- detail
- submit/approve/reject workflow endpoints

## Phase 1 Completion Criteria
- PR list and detail can run on real data
- maker/checker workflow works end-to-end
- status transitions enforce backend rules

---

# 8. Phase 2 – PO Endpoints

## Objective
Unblock PO List and PO Detail implementation, including PR → PO continuity.

## Blocking Endpoints
### List
- `GET /api/v1/purchase-orders`
- `GET /api/v1/purchase-orders/summary`

### Detail
- `GET /api/v1/purchase-orders/{id}`

### Draft Management
- `POST /api/v1/purchase-orders`
- `PATCH /api/v1/purchase-orders/{id}`

### Workflow Actions
- `POST /api/v1/purchase-orders/{id}/submit`
- `POST /api/v1/purchase-orders/{id}/approve`
- `POST /api/v1/purchase-orders/{id}/reject`
- `POST /api/v1/purchase-orders/{id}/cancel`
- `POST /api/v1/purchase-orders/{id}/close`

### PR Context / Downstream Readiness
- `GET /api/v1/purchase-requests/{id}/linked-purchase-orders`
- optional `GET /api/v1/purchase-orders/{id}/grn-eligibility`

### Supporting Endpoints
- `GET /api/v1/purchase-orders/{id}/comments`
- `POST /api/v1/purchase-orders/{id}/comments`
- `GET /api/v1/purchase-orders/{id}/attachments`
- `GET /api/v1/purchase-orders/{id}/audit-log`
- `GET /api/v1/purchase-orders/{id}/linked-grns`
- `GET /api/v1/purchase-orders/{id}/receipt-progress`

## Minimum Payload Requirements
### PO List must return
- id
- po_no
- po_date
- vendor
- source_pr
- status
- grand_total
- expected_delivery_date
- linked_grn_count or indicator
- receipt progress summary

### PO Detail must return
- header
- source PR summary
- vendor/commercial fields
- totals
- line items
- workflow status
- grn creation eligibility or enough context to compute it

## Frontend Mock Tolerance
Can temporarily mock:
- receipt progress
- linked GRNs
- comments/attachments/audit

Cannot practically mock for long:
- PO detail
- PO submit/approve/reject
- PR → PO creation flow

## Phase 2 Completion Criteria
- PO pages use real backend data
- PR → PO traceability works
- PO approval workflow works
- PO indicates downstream GRN readiness

---

# 9. Phase 3 – GRN Endpoints

## Objective
Unblock GRN Entry and GRN Detail, including PO → GRN continuity and receipt lifecycle.

## Blocking Endpoints
### Prefill / Entry Context
Recommended:
- `GET /api/v1/purchase-orders/{id}/grn-prefill`
or
- `GET /api/v1/goods-receipts/prefill?source_po={id}`

### Draft / Create
- `POST /api/v1/goods-receipts`
- `PATCH /api/v1/goods-receipts/{id}`

### Detail
- `GET /api/v1/goods-receipts/{id}`

### Workflow / Lifecycle
- `POST /api/v1/goods-receipts/{id}/mark-received`
- `POST /api/v1/goods-receipts/{id}/send-to-qc`
- `POST /api/v1/goods-receipts/{id}/accept`
- `POST /api/v1/goods-receipts/{id}/reject`
- `POST /api/v1/goods-receipts/{id}/cancel`

### Supporting Endpoints
- `GET /api/v1/goods-receipts/{id}/comments`
- `POST /api/v1/goods-receipts/{id}/comments`
- `GET /api/v1/goods-receipts/{id}/attachments`
- `GET /api/v1/goods-receipts/{id}/audit-log`
- `GET /api/v1/goods-receipts/{id}/linked-inventory-movements`
- `GET /api/v1/goods-receipts/{id}/linked-qc-records`
- `GET /api/v1/goods-receipts/{id}/stock-impact`

## Minimum Payload Requirements
### GRN Prefill must return
- source PO summary
- receivable lines
- remaining quantities
- receiving defaults
- warehouse/location defaults if available
- batch/serial/QC requirement flags

### GRN Detail must return
- header
- source PO summary
- receipt line items
- warehouse/location details
- status / QC state
- receipt quantities
- stock impact summary or flag

## Frontend Mock Tolerance
Can temporarily mock:
- linked inventory movement
- stock impact display
- attachments/comments/audit

Cannot practically mock for long:
- prefill endpoint
- create/update/mark-received flow
- quantity validation logic

## Phase 3 Completion Criteria
- GRN entry works from approved PO
- quantity validation enforced by backend
- GRN detail uses real data
- PO receipt progress can refresh from GRN actions

---

# 10. Phase 4 – Incoming QC Endpoints

## Objective
Unblock QA operational processing for QC-required receipts.

## Preferred Endpoint Strategy
Use a dedicated QC queue instead of forcing the frontend to derive QC action rows from generic GRN list endpoints.

## Blocking Endpoints
### Queue
- `GET /api/v1/quality/incoming-qc/pending`
or fallback:
- `GET /api/v1/goods-receipts?qc_status=pending`

### Result Actions
Preferred:
- `POST /api/v1/quality/incoming-qc/{id}/accept`
- `POST /api/v1/quality/incoming-qc/{id}/reject`
or fallback:
- `POST /api/v1/goods-receipts/{id}/accept`
- `POST /api/v1/goods-receipts/{id}/reject`

### Supporting Endpoints
- `POST /api/v1/attachments` for QC evidence
- `POST /api/v1/goods-receipts/{id}/comments` or dedicated QC comment endpoint

## Minimum Payload Requirements
### QC Queue must return
- QC-actionable records only
- source GRN id/no
- source PO/vendor/item context
- received qty
- accepted/rejected qty constraints
- warehouse/date context
- actionability flag

## Frontend Mock Tolerance
Can temporarily mock:
- QC evidence upload
- some queue enrichment fields

Cannot practically mock for long:
- QC pending queue
- accept/reject result endpoints
- quantity reconciliation logic

## Phase 4 Completion Criteria
- QA can process incoming QC from real backend data
- QC decisions affect linked GRN/stock state
- non-QA roles remain blocked from QC actions

---

# 11. Phase 5 – Stock Overview + Inventory Movement Endpoints

## Objective
Unblock inventory visibility after GRN/QC actions.

## Blocking Endpoints
### Stock Overview
- `GET /api/v1/stock-balances`
- `GET /api/v1/stock-balances/summary`

### Drilldown / Movement
- `GET /api/v1/stock-balances/{id}`
or
- `GET /api/v1/stock-balances/{id}/movements`
or generic:
- `GET /api/v1/inventory-movements`

### Optional Alerts / Enrichment
- `GET /api/v1/stock-balances/low-stock`

## Minimum Payload Requirements
### Stock Overview must return
- item identity
- warehouse/location
- on_hand_qty
- available_qty
- reserved_qty
- qc_hold_qty
- rejected_or_blocked_qty
- reorder level / low-stock indicator
- last movement date
- source/drilldown hint

### Movement Drilldown should return
- movement type
- quantity
- source document reference
- stock state effect
- timestamp
- warehouse/location context

## Frontend Mock Tolerance
Can temporarily mock:
- low-stock alert enrichment
- advanced drilldown

Cannot practically mock for long:
- overview rows
- stock state buckets
- source-linked movement visibility

## Phase 5 Completion Criteria
- stock overview renders real data
- accepted/rejected/QC-hold stock buckets are differentiated
- drilldown/movement source traceability works

---

# 12. Phase 6 – Integration Enrichment and Hardening

## Objective
Complete the “works in practice” layer after core endpoints exist.

## Priority Enhancements
- linked-record enrichment payloads
- better summary endpoints
- more complete audit entries
- performance tuning for list/detail screens
- batch/serial receiving details if not done earlier
- export endpoints
- dashboard summary endpoints for this vertical slice

## Important Cleanup Areas
- consistent validation messages
- consistent permission errors
- consistent invalid-transition errors
- consistent pagination and sort behavior
- response shape normalization across modules

## Phase 6 Completion Criteria
- no major page is blocked by payload gaps
- linked-record views feel complete
- workflow actions are consistent
- performance is acceptable for internal pilot/UAT

---

# 13. Backend Priority by Frontend Sprint

## Supports Sprint 0–1
Must be ready:
- auth/me
- response/error model
- comments/attachments/audit patterns

## Supports Sprint 2
Must be ready:
- PR list/detail/workflow endpoints

## Supports Sprint 3
Must be ready:
- PO list/detail/workflow endpoints
- PR → PO context linkage

## Supports Sprint 4
Must be ready:
- GRN prefill/create/detail/workflow endpoints

## Supports Sprint 5
Must be ready:
- incoming QC queue and result endpoints

## Supports Sprint 6
Must be ready:
- stock overview and movement endpoints

## Supports Sprint 7
Must be ready:
- enrichments, hardening, cleanup, performance

---

# 14. Suggested Backend Team Build Order

## Order 1 – Shared Platform
- auth/me
- API client consistency patterns
- comments/attachments/audit patterns

## Order 2 – Procurement Core
- PR endpoints
- PO endpoints

## Order 3 – Receiving Core
- GRN prefill
- GRN create/update/detail/workflow

## Order 4 – Quality
- QC queue/results

## Order 5 – Inventory Visibility
- stock overview
- movement/drilldown

## Order 6 – Hardening
- enrichment
- performance
- edge-case consistency

---

# 15. What Frontend Can Use Mocks For Temporarily

## Safe Temporary Mocks
- comments panel
- attachments panel
- audit panel
- low-stock enrichments
- some linked-record badge counts
- some drilldown enrichment

## Unsafe Long-Term Mocks
- workflow transitions
- approval actions
- quantity validation
- receipt prefill context
- stock buckets
- QC accept/reject result logic
- maker-checker enforcement

These must be backend-real early.

---

# 16. Payload Design Recommendations

## Prefer page-shaped detail responses
For major detail pages:
- include the main page body data in one strong endpoint
- keep support sections separated only when it adds clarity or performance

## Include actionability metadata where useful
Examples:
- `can_submit`
- `can_approve`
- `can_create_grn`
- `qc_required`
- `remaining_receivable_qty`

This reduces duplicated business reconstruction in frontend.

## Use linked-record summary blocks
Examples:
- linked PO summary on PR detail
- linked GRN summary on PO detail
- linked stock impact on GRN detail

---

# 17. Completion Criteria
This file is complete when:
- backend build order is defined
- sprint dependencies are mapped
- blocking vs enhancement endpoints are separated
- frontend mock boundaries are clear
- minimum payload expectations are documented

---

# 18. Next Related Files
Recommended next files:
1. frontend_module_bootstrap_checklist.md
2. coding_standards_frontend.md
3. first_vertical_slice_delivery_checklist.md
4. backend_validation_error_contract.md
