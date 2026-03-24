# API Page Dependency Matrix

## 1. Purpose
This file defines the exact dependency between frontend pages and backend APIs for the current ERP design slice.

It standardizes:
- which APIs each page depends on
- which APIs are required for initial page load
- which APIs are required for user actions
- which APIs are required for supporting panels such as comments, attachments, audit, linked records, and summary widgets
- what data dependencies drive refresh and invalidation behavior

This file is the main frontend-backend integration map for implementation.

---

## 2. Recommended Folder Location
Keep this file at:

`frontend/api_page_dependency_matrix.md`

---

## 3. Scope Covered
This matrix currently covers:
- PR List
- PR Detail
- PO List
- PO Detail
- GRN Entry
- GRN Detail
- Incoming QC
- Stock Overview

---

## 4. Dependency Mapping Principles
### Page load dependencies
A page should define the minimum API calls required to render usable UI on first load.

### Action dependencies
Each page should define which mutation endpoints are triggered by actions and what must refresh afterward.

### Supporting section dependencies
Comments, attachments, audit, linked records, and summaries should have explicit API dependencies.

### Refresh/invalidation must be intentional
Every mutation should identify:
- which query data becomes stale
- which page sections need refresh
- whether list and summary pages also need refresh

### Backend should shape data for page needs
Do not force frontend to reconstruct page meaning from low-level fragmented APIs unless there is a good reason.

---

# 5. Page-by-Page API Dependency Matrix

## 5.1 PR List

### Route
`/procurement/purchase-requests`

### Initial Load API Dependencies
- `GET /api/v1/purchase-requests`
- `GET /api/v1/purchase-requests/summary`
- optional filter metadata endpoint if dynamic filters are needed
- optional current-user permission/context endpoint

### Main Query Inputs
- page
- per_page
- sort
- search
- status
- requested_by
- department_id
- branch_id
- priority
- date_from
- date_to

### Main Query Outputs Needed
- PR row list
- pagination metadata
- summary counts by status
- row-level action eligibility flags or enough data to compute them
- linked PO indicator/count if supported on list

### Mutation Dependencies
- `POST /api/v1/purchase-requests/{id}/submit`
- `POST /api/v1/purchase-requests/{id}/approve`
- `POST /api/v1/purchase-requests/{id}/reject`
- `POST /api/v1/purchase-requests/{id}/cancel`
- `POST /api/v1/reports/export` or equivalent list export endpoint

### After-Mutation Refresh Rules
After submit/approve/reject/cancel:
- refetch PR list
- refetch PR summary strip
- preserve current filter/sort/page context

### Integration Notes
Backend should ideally include:
- status
- requester
- amount summary
- department/branch context
- linked downstream indicator

---

## 5.2 PR Detail

### Route
`/procurement/purchase-requests/{id}`

### Initial Load API Dependencies
- `GET /api/v1/purchase-requests/{id}`
- `GET /api/v1/purchase-requests/{id}/comments`
- `GET /api/v1/purchase-requests/{id}/attachments`
- `GET /api/v1/purchase-requests/{id}/audit-log`
- `GET /api/v1/purchase-requests/{id}/linked-purchase-orders`
- optional permissions/context endpoint if detail payload does not already include action metadata

### Main Query Outputs Needed
- PR header detail
- line items
- workflow/status data
- approval metadata
- source/linked references
- action eligibility flags or enough data to compute them
- comments
- attachments
- audit entries
- linked PO summary

### Mutation Dependencies
- `PATCH /api/v1/purchase-requests/{id}`
- `POST /api/v1/purchase-requests/{id}/submit`
- `POST /api/v1/purchase-requests/{id}/approve`
- `POST /api/v1/purchase-requests/{id}/reject`
- `POST /api/v1/purchase-requests/{id}/cancel`
- `POST /api/v1/purchase-requests/{id}/comments`
- `POST /api/v1/attachments` or record-scoped upload endpoint

### After-Mutation Refresh Rules
After edit/submit/approve/reject/cancel:
- refetch PR detail
- refetch audit log
- refetch linked PO block if conversion may have happened

After add comment:
- refetch comments

After upload attachment:
- refetch attachments

### Integration Notes
Prefer one rich detail payload for the main page body, rather than separate calls for each core section.

---

## 5.3 PO List

### Route
`/procurement/purchase-orders`

### Initial Load API Dependencies
- `GET /api/v1/purchase-orders`
- `GET /api/v1/purchase-orders/summary`
- optional filter metadata endpoint
- optional current-user permission/context endpoint

### Main Query Inputs
- page
- per_page
- sort
- search
- status
- vendor_id
- buyer_id
- branch_id
- delivery_status
- date_from
- date_to

### Main Query Outputs Needed
- PO row list
- pagination metadata
- summary counts
- receipt progress indicator
- linked GRN indicator/count
- row-level workflow/action eligibility context

### Mutation Dependencies
- `POST /api/v1/purchase-orders/{id}/submit`
- `POST /api/v1/purchase-orders/{id}/approve`
- `POST /api/v1/purchase-orders/{id}/reject`
- `POST /api/v1/purchase-orders/{id}/cancel`
- `POST /api/v1/reports/export` or list export equivalent

### After-Mutation Refresh Rules
After workflow action:
- refetch PO list
- refetch PO summary strip
- preserve current filter state

---

## 5.4 PO Detail

### Route
`/procurement/purchase-orders/{id}`

### Initial Load API Dependencies
- `GET /api/v1/purchase-orders/{id}`
- `GET /api/v1/purchase-orders/{id}/comments`
- `GET /api/v1/purchase-orders/{id}/attachments`
- `GET /api/v1/purchase-orders/{id}/audit-log`
- `GET /api/v1/purchase-orders/{id}/linked-grns`
- `GET /api/v1/purchase-orders/{id}/receipt-progress`
- optional permission/context endpoint if needed

### Main Query Outputs Needed
- PO header detail
- source PR summary
- vendor/commercial details
- totals summary
- line items
- workflow state
- linked GRNs
- receipt progress
- comments
- attachments
- audit entries

### Mutation Dependencies
- `PATCH /api/v1/purchase-orders/{id}`
- `POST /api/v1/purchase-orders/{id}/submit`
- `POST /api/v1/purchase-orders/{id}/approve`
- `POST /api/v1/purchase-orders/{id}/reject`
- `POST /api/v1/purchase-orders/{id}/cancel`
- `POST /api/v1/purchase-orders/{id}/close`
- `POST /api/v1/purchase-orders/{id}/comments`
- `POST /api/v1/attachments` or record-scoped upload endpoint

### Downstream Navigation Dependency
- `GET /api/v1/purchase-orders/{id}` or dedicated lightweight endpoint must expose whether GRN creation is allowed
- route to GRN Entry should use `source_po` prefill context

### After-Mutation Refresh Rules
After edit/submit/approve/reject/cancel/close:
- refetch PO detail
- refetch audit log
- refetch receipt progress if lifecycle changed

After GRN creation in downstream flow:
- refetch linked GRNs
- refetch receipt progress

After comment/attachment changes:
- refetch those panels only

---

## 5.5 GRN Entry

### Route
`/procurement/grn/new?source_po={id}`

### Initial Load API Dependencies
- `GET /api/v1/purchase-orders/{id}` or dedicated GRN prefill context endpoint
- optional `GET /api/v1/warehouses`
- optional `GET /api/v1/locations`
- optional item-level rule endpoint for batch/serial/QC requirements
- optional permission/context endpoint

### Recommended Better Pattern
Instead of overusing raw PO detail:
- provide a dedicated endpoint such as:
  - `GET /api/v1/purchase-orders/{id}/grn-prefill`
or
  - `GET /api/v1/goods-receipts/prefill?source_po={id}`

This endpoint should return:
- source PO summary
- receivable line items
- remaining quantities
- receiving defaults
- item-level receiving rules

### Main Query Outputs Needed
- source PO summary
- receivable lines
- warehouse/location defaults
- qty constraints
- batch/serial requirement flags
- QC requirement flags

### Mutation Dependencies
- `POST /api/v1/goods-receipts`
- `PATCH /api/v1/goods-receipts/{id}` if draft/pending can be saved incrementally
- `POST /api/v1/goods-receipts/{id}/mark-received`
- `POST /api/v1/goods-receipts/{id}/send-to-qc`
- `POST /api/v1/attachments` if attachments supported during entry

### After-Mutation Refresh Rules
After successful create/mark received:
- navigate to GRN Detail
- downstream pages should later refresh linked PO receipt progress

### Integration Notes
Backend should enforce quantity limits and return validation errors mapped cleanly to line-item UI.

---

## 5.6 GRN Detail

### Route
`/procurement/grn/{id}`

### Initial Load API Dependencies
- `GET /api/v1/goods-receipts/{id}`
- `GET /api/v1/goods-receipts/{id}/comments`
- `GET /api/v1/goods-receipts/{id}/attachments`
- `GET /api/v1/goods-receipts/{id}/audit-log`
- `GET /api/v1/goods-receipts/{id}/linked-inventory-movements`
- `GET /api/v1/goods-receipts/{id}/linked-qc-records`
- `GET /api/v1/goods-receipts/{id}/stock-impact`
- optional permission/context endpoint if needed

### Main Query Outputs Needed
- GRN header detail
- source PO summary
- receipt line items
- status and QC state
- comments
- attachments
- audit entries
- linked inventory movement summary
- linked QC summary
- stock impact summary

### Mutation Dependencies
- `PATCH /api/v1/goods-receipts/{id}`
- `POST /api/v1/goods-receipts/{id}/mark-received`
- `POST /api/v1/goods-receipts/{id}/send-to-qc`
- `POST /api/v1/goods-receipts/{id}/accept`
- `POST /api/v1/goods-receipts/{id}/reject`
- `POST /api/v1/goods-receipts/{id}/cancel`
- `POST /api/v1/goods-receipts/{id}/comments`
- `POST /api/v1/attachments` or record-scoped upload endpoint

### After-Mutation Refresh Rules
After mark-received/send-to-qc/accept/reject/cancel:
- refetch GRN detail
- refetch audit log
- refetch stock impact
- refetch linked QC/inventory references

After comment/attachment changes:
- refetch those sections only

### Integration Notes
If stock impact is core to this page, backend should provide a dedicated clean payload, not force frontend to derive stock state from raw movement rows only.

---

## 5.7 Incoming QC

### Route
`/procurement/incoming-qc`

### Initial Load API Dependencies
- `GET /api/v1/goods-receipts` with QC-pending filters, or
- dedicated queue endpoint such as:
  - `GET /api/v1/quality/incoming-qc/pending`
- optional filter metadata endpoint
- optional permission/context endpoint

### Recommended Better Pattern
A dedicated QC queue endpoint is preferred because frontend needs:
- only QC-actionable records
- normalized line/action context
- cleaner operational performance

### Main Query Inputs
- status
- warehouse_id
- vendor_id
- date_from
- date_to
- source_grn
- page
- per_page
- sort

### Main Query Outputs Needed
- QC queue rows
- GRN/item context
- qty split context
- whether row is actionable
- source links
- pagination metadata

### Mutation Dependencies
- `POST /api/v1/goods-receipts/{id}/accept`
- `POST /api/v1/goods-receipts/{id}/reject`
or, if QC becomes a dedicated domain endpoint:
- `POST /api/v1/quality/incoming-qc/{id}/result`
- `POST /api/v1/quality/incoming-qc/{id}/accept`
- `POST /api/v1/quality/incoming-qc/{id}/reject`
- `POST /api/v1/attachments` for QC evidence
- `POST /api/v1/goods-receipts/{id}/comments` or QC comment endpoint

### After-Mutation Refresh Rules
After QC result:
- refetch QC queue
- if opened from deep-linked GRN context, refetch GRN detail status in linked view/context
- downstream Stock Overview should eventually reflect accepted/rejected stock changes

### Integration Notes
QC queue payload should expose accepted/rejected quantity constraints clearly to reduce frontend business reconstruction.

---

## 5.8 Stock Overview

### Route
`/inventory/stock-overview`

### Initial Load API Dependencies
- `GET /api/v1/stock-balances`
- `GET /api/v1/stock-balances/summary`
- optional filter metadata endpoint
- optional low-stock endpoint:
  - `GET /api/v1/stock-balances/low-stock`
or embed low-stock flags directly in the overview/summary payload

### Main Query Inputs
- search
- warehouse_id
- location_id
- item_id
- category_id
- stock_status
- low_stock
- hide_zero
- page
- per_page
- sort

### Main Query Outputs Needed
- current stock rows
- split quantities by state (available, reserved, QC hold, rejected/blocked)
- summary totals
- low-stock indicators
- source/drilldown reference hints
- pagination metadata

### Drilldown Dependencies
- `GET /api/v1/inventory-movements`
- or dedicated stock-row detail endpoint such as:
  - `GET /api/v1/stock-balances/{id}`
  - `GET /api/v1/stock-balances/{id}/movements`

### Mutation Dependencies
Current page is mostly read/analysis oriented.
Mutations are future-oriented:
- export stock overview
- future stock transfer
- future stock adjustment

### After-Mutation Refresh Rules
After any upstream stock-impacting event (GRN accepted/QC result/adjustment/transfer):
- refetch stock overview
- refetch stock summary
- refetch active drilldown panel if open

### Integration Notes
Backend should expose stock buckets clearly instead of forcing frontend to infer usable stock from raw ledger movements.

---

# 6. Cross-Page Invalidation / Refresh Matrix

## PR approved
Refresh:
- PR Detail
- PR List summary
- any dashboard widget for pending PR approvals
- downstream Create PO eligibility context

## PO approved
Refresh:
- PO Detail
- PO List summary
- dashboard widgets for approved/open POs
- GRN creation eligibility context

## GRN marked received
Refresh:
- GRN Detail
- PO Detail receipt progress
- PO linked GRNs
- QC queue if QC required

## GRN accepted / QC accepted
Refresh:
- GRN Detail
- stock impact section
- Stock Overview
- PO receipt progress
- linked inventory movement panel

## GRN rejected / QC rejected
Refresh:
- GRN Detail
- QC queue
- Stock Overview if rejected stock bucket is represented there
- procurement traceability views

---

# 7. Recommended API Shaping Strategy

## Use detail endpoints that are page-aware enough
Example:
- PO Detail should not require 8 unrelated low-level endpoints if a richer detail contract can provide the page body cleanly.

## Use queue-specific endpoints for operational workspaces
Example:
- Incoming QC queue should have a dedicated filtered API rather than only generic GRN listing.

## Use linked-record endpoints intentionally
Example:
- linked GRNs
- linked POs
- linked inventory movements
- linked QC records

## Keep comments/attachments/audit generic but consistent
Prefer shared cross-module API patterns for:
- comments
- attachments
- audit logs

---

# 8. Suggested Query Key Grouping (Frontend)
Frontend query keys can be grouped like:

- `pr.list`
- `pr.summary`
- `pr.detail`
- `pr.comments`
- `pr.attachments`
- `pr.audit`
- `pr.linkedPOs`

- `po.list`
- `po.summary`
- `po.detail`
- `po.receiptProgress`
- `po.linkedGRNs`

- `grn.detail`
- `grn.comments`
- `grn.attachments`
- `grn.audit`
- `grn.stockImpact`
- `grn.linkedInventory`
- `grn.linkedQC`

- `qc.queue`

- `stock.overview`
- `stock.summary`
- `stock.drilldown`

This makes invalidation easier and more consistent.

---

# 9. Completion Criteria
This file is complete when:
- each page has initial load API dependencies
- each page has mutation dependencies
- refresh/invalidation rules are documented
- major supporting panels have explicit API dependencies
- backend shaping recommendations are clear

---

# 10. Next Related Files
Recommended next files:
1. frontend_state_management_plan.md
2. frontend_folder_structure_code_plan.md
3. frontend_build_sequence_sprint_plan.md
4. backend_endpoint_priority_plan.md
