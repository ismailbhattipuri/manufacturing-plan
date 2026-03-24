# Page: GRN List

## 1. Page Purpose
The GRN List page is the main operational listing screen for Goods Receipt Notes (GRN). It allows authorized users to:
- view all goods receipts
- track receipt status and QC status
- identify pending, received, QC-pending, accepted, rejected, and cancelled GRNs
- navigate to GRN detail records
- monitor inbound activity by vendor, warehouse, date, and source PO
- quickly identify receipts requiring action or follow-up

This page is the primary operational visibility page for inbound procurement receipts.

---

## 2. Source Alignment
This page must remain aligned with:
- Step 2 – Procurement Module
- Step 4 – Procurement Flow
- Step 5 – Screen Map
- Step 6 – Wireframe Design
- Step 8 – Backend API
- Step 9 – Frontend Development
- txn_goods_receipt_note.md
- page_grn_entry.md
- page_grn_detail.md
- page_po_detail.md
- role_store_user.md
- role_qa_inspector.md

---

## 3. Module
- Procurement / Inventory Inbound

---

## 4. Recommended Folder Location
This file should be kept at:

`pages/procurement/page_grn_list.md`

### Why this location
Keep GRN list beside:
- `page_grn_entry.md`
- `page_grn_detail.md`
- `page_po_detail.md`

because it belongs to the same procurement receipt chain:
PO → GRN → QC → Stock

---

## 5. Page Type
- Standard ERP List Page

This page follows the enterprise list-page pattern:
- breadcrumb + title
- status/summary strip
- filters/search
- operational table
- row actions
- bulk/filter workflow support
- empty/loading/error/no-access states

---

## 6. Primary Roles
### Main Users
- Store User / Warehouse User
- Store Manager
- QA Inspector / QA Team

### Secondary Users
- Procurement Executive
- Procurement Manager
- Management (view only if permitted)
- Finance (view only if needed for traceability)

---

## 7. Business Goal of This Page
This page supports the following business outcomes:
- visibility into all inbound receipts
- fast identification of pending receipt and QC workload
- source PO traceability
- warehouse activity monitoring
- receipt discrepancy follow-up
- navigation into detail-level stock/QC decisions

---

## 8. Entry Points
Users may reach this page from:
- sidebar → Procurement → GRN List
- dashboard inbound receipt widget
- dashboard QC pending widget
- PO Detail linked GRN section
- notification links
- global search result return path
- breadcrumb navigation from GRN Detail

---

## 9. Route / Navigation Position
### Suggested Route
- `/procurement/grn`

### Related Routes
- `/procurement/grn/new?source_po={id}`
- `/procurement/grn/{id}`
- `/procurement/purchase-orders/{id}`
- `/procurement/incoming-qc`

---

## 10. Page Header
Recommended header content:
- breadcrumb
- page title: Goods Receipt Notes
- optional subtitle: manage and review inbound receipts
- primary action: Create GRN (optional visible only if standalone GRN creation is allowed)
- secondary actions: Export, Refresh

### Important Note
If business flow requires GRN creation only from PO Detail, the Create GRN button may be hidden or restricted.

---

## 11. KPI / Status Summary Strip
Recommended summary cards:
- Total GRNs
- Pending
- Received
- QC Pending
- Accepted
- Rejected
- Cancelled

### Optional Summary Cards
- Today’s Receipts
- Partial Receipts
- Pending QC Count

### Click Behavior
Each card may act as a quick filter.

Examples:
- click “QC Pending” → show only QC-pending receipts
- click “Accepted” → show accepted receipts
- click “Pending” → show incomplete receipt entries

---

## 12. Filter and Search Section
### Search
Search by:
- GRN Number
- PO Number
- Vendor
- Delivery Note Number
- Item name / item code (if supported)
- Warehouse

### Standard Filters
- Status
- QC Status
- Date Range
- Vendor
- Source PO
- Warehouse
- Received By
- Branch
- Has Discrepancy
- QC Required
- Receipt Type

### Advanced Filters
- Batch/Serial-controlled items
- partial receipt only
- accepted only
- rejected only
- stock posted yes/no
- source PR/PO context (future advanced traceability)

### Filter Actions
- Apply
- Reset
- Clear All
- Save View (future)

---

## 13. Table Structure
This page should use a high-density operational table.

### Recommended Columns
- GRN Number
- GRN Date
- Source PO
- Vendor
- Warehouse
- Received By
- Receipt Qty Summary
- QC Status
- Status
- Discrepancy Flag
- Stock Impact Status
- Last Updated
- Actions

### Optional Columns
- Delivery Note Number
- Branch
- Item Count
- Accepted Qty
- Rejected Qty
- Created By
- Linked QC Indicator

### Column Behavior
- GRN Number should be clickable to detail page
- Source PO should be clickable
- status and QC status should use distinct badges
- discrepancy rows should be visually identifiable

---

## 14. Suggested Default Sort
Recommended default:
- latest GRN first
or
- most actionable records first for operational users

### Default Recommendation
- `created_at DESC`

### Operational Alternative
- QC Pending first, then newest pending receipts

---

## 15. Pagination
Use server-driven pagination.

### Standard Controls
- page number
- page size
- total count
- next/previous

### Recommended Default Page Size
- 20 rows

---

## 16. Row Click Behavior
Default row click:
- open GRN Detail

### Route Pattern
- `/procurement/grn/{id}`

---

## 17. Row Actions
Row actions should depend on role and current GRN status.

### Common Row Actions
- View
- Edit
- Mark Received
- Send to QC
- Accept
- Reject
- Cancel
- Print / Export
- View Source PO
- View Stock Impact

### Role + Status Logic
#### Store User
- Pending → View, Edit, Mark Received, Cancel
- Received → View, Send to QC, Accept (if policy allows no-QC path)
- QC Pending → View only, Comment if detail page used
- Accepted / Rejected / Cancelled → View

#### QA Inspector
- Pending → usually no action
- Received + QC Required → View / open for QC context
- QC Pending → View, Accept, Reject
- Accepted / Rejected → View only

#### Procurement Team
- all statuses → View only for traceability
- no warehouse mutation actions

---

## 18. Bulk Actions
Bulk actions should be conservative.

### Possible Bulk Actions
- Bulk Export
- Bulk Print
- Bulk Send to QC (only if business explicitly allows)
- Bulk Accept / Reject should usually be avoided unless tightly controlled

### Guardrails
- block mixed-status invalid actions
- only show actions to authorized roles
- prefer detail page for higher-risk operations

---

## 19. Primary CTA Rules
### Create GRN Button Visibility
Visible only if:
- user has receiving/create permission
- standalone GRN entry is allowed by business
- or system supports GRN creation without always entering from PO detail

Otherwise:
- rely on PO Detail → Create GRN path

---

## 20. Status-Based UI Rules
### Pending
- editable by receiving role
- mark received action visible
- draft/pending indicator visible

### Received
- operational receipt completed
- QC path or accept/reject path visible depending on policy

### QC Pending
- emphasize QA action requirement
- show queue/action marker

### Accepted
- show stock impact as complete
- view-only for most users

### Rejected
- show rejection indicator clearly
- source discrepancy / issue follow-up visible

### Cancelled
- archived/read-only appearance

---

## 21. Role-Based UI Rules
### Store User
Should see:
- operational receipt workload
- pending and received GRNs
- warehouse and qty context
- source PO link
- stock-impact status

Should not see:
- procurement approval controls

### QA Inspector
Should see:
- QC Pending receipts prominently
- source receipt context
- QC-related row actions where allowed

Should not see:
- receipt-entry edit actions not relevant to QA

### Procurement Manager / Executive
Should see:
- source PO/GRN traceability
- vendor and discrepancy visibility
- no receiving mutation actions

---

## 22. Visual Priority Rules
The UI should prioritize:
1. QC Pending GRNs
2. Pending / incomplete GRNs
3. Rejected/discrepant GRNs
4. Recent accepted receipts
5. Cancelled/archive cases lower priority

---

## 23. Empty State
### No GRNs at All
Show:
- informative empty state
- explanation of GRN purpose
- Create GRN CTA if user is allowed

### No Match After Filters
Show:
- “No goods receipts match the current filters”
- clear filters action

---

## 24. Loading State
Use:
- table skeleton rows
- summary strip skeleton
- filter/loading indicators where needed

---

## 25. Error State
Possible errors:
- list fetch failed
- summary fetch failed
- mutation failed
- permission/context mismatch

Display:
- clear message
- retry action
- preserve filters if possible

---

## 26. No-Access State
If the user lacks permission:
- show restricted-access state
- hide GRN list data
- prevent action visibility

---

## 27. Data Requirements
This page needs:
- paginated GRN list
- status summary counts
- QC summary counts
- row-level status and action context
- source PO/vendor/warehouse references
- discrepancy and stock-impact indicators

---

## 28. API Dependencies
### Core APIs
- Get GRN list
- Get GRN summary counts
- Mark GRN received
- Send GRN to QC
- Accept GRN
- Reject GRN
- Cancel GRN
- Export GRN list

### Example Endpoint Needs
- `GET /api/v1/goods-receipts`
- `GET /api/v1/goods-receipts/summary`
- `POST /api/v1/goods-receipts/{id}/mark-received`
- `POST /api/v1/goods-receipts/{id}/send-to-qc`
- `POST /api/v1/goods-receipts/{id}/accept`
- `POST /api/v1/goods-receipts/{id}/reject`
- `POST /api/v1/goods-receipts/{id}/cancel`

### Query Support Needed
- page
- per_page
- sort
- search
- status
- qc_status
- warehouse_id
- vendor_id
- source_po
- date_from
- date_to
- received_by
- branch_id

---

## 29. Validation / Action Rules on This Page
### Before Mark Received
- GRN must be Pending
- quantities must be valid
- user must have receiving permission

### Before Send to QC
- GRN must be Received
- QC-required condition must exist if policy requires

### Before Accept
- role and status must allow it
- qty reconciliation must pass
- QC path rules must pass

### Before Reject
- role and status must allow it
- rejection reason must be captured

### Before Cancel
- GRN must be cancellable under policy
- stock-impact stage must allow cancellation

---

## 30. Suggested Modals / Drawers
### Useful Interaction Elements
- Mark Received Confirmation
- Reject Reason Modal
- Cancel Confirmation Modal
- Quick Preview Drawer (optional future)
- QC Redirect / Action Prompt for QC-pending records

---

## 31. Export / Print Behavior
### Export
Allowed for permitted roles.
Export should respect filters.

### Print
Optional and less critical than detail-page print, but may be useful for warehouse logs.

---

## 32. Audit / Traceability Signals on List
List rows should expose enough metadata for operational traceability:
- source PO
- vendor
- receipt date
- warehouse
- status / QC status
- stock impact flag
- linked issues / discrepancy flag

Detailed audit remains on GRN Detail.

---

## 33. Notification / Deep Link Behavior
This page should support deep links from:
- dashboard inbound widget
- QC pending dashboard widget
- receipt-created notification
- rejected receipt follow-up notification

Query-param filtered entries should be supported where useful.

Examples:
- `/procurement/grn?status=pending`
- `/procurement/grn?qc_status=pending`

---

## 34. Wireframe Notes
For wireframe execution:
- use a wide table
- emphasize dual-status visibility (receipt status + QC status)
- discrepancy indicators should stand out
- source PO column should be easy to scan
- summary strip should support quick operational triage

---

## 35. Frontend Build Notes
### Reusable Components Suggested
- PageHeader
- SummaryCardStrip
- FilterBar
- DataTable
- StatusBadge
- RowActionMenu
- ConfirmDialog
- RejectReasonModal
- EmptyState
- ErrorState

### State Handling Needed
- filter state
- search state
- pagination state
- sort state
- action loading state
- active modal state
- role-aware row action rendering

---

## 36. Backend Build Notes
Backend should ensure:
- summary counts are efficient
- QC status and receipt status are both queryable
- row-level action eligibility can be derived cleanly
- mutation endpoints enforce quantity/status/role rules
- source PO and vendor references are available without extra joins on frontend
- list performance remains acceptable for operational use

---

## 37. Completion Criteria
GRN List page design is complete when:
- header, filters, summary strip, and table columns are fixed
- row actions are defined by role and status
- pagination/search/filter behavior is clear
- empty/loading/error/no-access states are defined
- API dependencies are listed
- PO → GRN → QC operational visibility is reflected

---

## 38. Next Related Files
Recommended next files:
1. txn_qc_inspection.md
2. page_stock_movement.md
3. page_stock_adjustment.md
4. page_stock_transfer.md


## 39. Frontend Query Keys

### Query Keys
- ['grn', 'list', filters]
- ['grn', 'summary']
- ['grn', 'detail', grnId]

### Invalidation Rules
After actions:
- Mark Received → invalidate ['grn','list'], ['grn','summary']
- Send to QC → invalidate ['grn','list'], ['grn','summary']
- Accept/Reject → invalidate ['grn','list'], ['grn','summary'], ['grn','detail', id]

### Notes
- avoid full refetch of all modules
- only invalidate affected GRN-related queries


## 40. API Response Shape (Expected)

### GRN List Response
{
  "data": [
    {
      "id": "GRN-001",
      "grn_number": "GRN-001",
      "grn_date": "2026-01-10",
      "po_number": "PO-123",
      "vendor_name": "ABC Supplier",
      "warehouse_name": "Main WH",
      "received_by": "John",
      "receipt_qty": 120,
      "accepted_qty": 100,
      "rejected_qty": 20,
      "status": "RECEIVED",
      "qc_status": "PENDING",
      "has_discrepancy": true,
      "stock_posted": false
    }
  ],
  "meta": {
    "page": 1,
    "per_page": 20,
    "total": 240
  }
}

## 41. Action Permission Matrix

| Action          | Store User | QA Inspector | Procurement |
|----------------|-----------|-------------|------------|
| View           | ✅        | ✅          | ✅         |
| Edit           | Pending   | ❌          | ❌         |
| Mark Received  | Pending   | ❌          | ❌         |
| Send to QC     | Received  | ❌          | ❌         |
| Accept         | Received* | QC Pending  | ❌         |
| Reject         | ❌        | QC Pending  | ❌         |
| Cancel         | Pending   | ❌          | ❌         |

*depends on business rule (QC optional flow)