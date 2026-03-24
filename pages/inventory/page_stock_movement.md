# Page: Stock Movement

## 1. Page Purpose
The Stock Movement page is the operational and analytical page used to view inventory movement history across the ERP system. It allows authorized users to:
- view every stock increase, decrease, transfer, and stock-state change
- trace movement back to source documents such as GRN, QC, stock adjustment, stock transfer, production, and returns
- understand where quantity moved from and to
- audit stock behavior over time
- investigate discrepancies, stock history, and movement impact

This page is the primary inventory ledger-style visibility screen for stock movement.

---

## 2. Source Alignment
This page must remain aligned with:
- Step 2 – Inventory Module
- Step 4 – Inventory / Process Flow Design
- Step 5 – Screen Map
- Step 8 – Backend API
- Step 9 – Frontend Development
- txn_inventory_movement.md
- txn_goods_receipt_note.md
- txn_qc_inspection.md
- flow_purchase_return.md
- flow_sales_return.md
- flow_rework.md
- flow_scrap_handling.md
- flow_subcontracting.md
- page_stock_overview.md
- page_grn_detail.md
- role_store_user.md
- role_qa_inspector.md

---

## 3. Module
- Inventory

---

## 4. Recommended Folder Location
This file should be kept at:

`pages/inventory/page_stock_movement.md`

### Why this location
This page belongs to Inventory because it is the system-wide stock ledger view, even though movements may originate from procurement, quality, production, returns, or stock operations.

---

## 5. Page Type
- Standard ERP List / Ledger Page

This page follows a ledger-style list pattern:
- breadcrumb + title
- filter/search area
- summary/quick stats (optional)
- movement table
- drilldown/source document linking
- export and audit-style traceability support
- empty/loading/error/no-access states

---

## 6. Primary Roles
### Main Users
- Store User / Warehouse User
- Store Manager
- Procurement Team (view)
- QA Team (view)
- Production Team (view)
- Management (view)
- Finance / audit users (view if permitted)

### Secondary Users
- Admin (support only)

---

## 7. Business Goal of This Page
This page supports the following business outcomes:
- traceable inventory history
- source-document auditability
- stock discrepancy investigation
- movement-level analysis by item, warehouse, and movement type
- validation of stock impact from GRN, QC, returns, transfers, scrap, and other operations
- operational confidence in inventory integrity

---

## 8. Entry Points
Users may reach this page from:
- sidebar → Inventory → Stock Movement
- Stock Overview row drilldown
- GRN Detail → View Stock Impact / Inventory Movement
- future stock transfer/adjustment detail pages
- audit/reconciliation workflows
- dashboard stock exception widgets
- global search result links

---

## 9. Route / Navigation Position
### Suggested Route
- `/inventory/stock-movements`

### Related Routes
- `/inventory/stock-overview`
- `/procurement/grn/{id}`
- `/inventory/stock-adjustment/{id}` (future)
- `/inventory/stock-transfer/{id}` (future)

---

## 10. Page Header
Recommended header content:
- breadcrumb
- page title: Stock Movements
- optional subtitle: trace inventory changes across the system
- secondary actions: Export, Refresh

### Optional Future Primary Actions
This page should usually be ledger-like and not the main place for direct stock mutations.
Future mutation actions belong more naturally to:
- Stock Adjustment page
- Stock Transfer page

---

## 11. Optional KPI / Summary Strip
Recommended summary cards (optional but useful):
- Total Movements
- Stock In
- Stock Out
- Internal Transfers
- QC State Movements
- Rejected / Scrap Movements

### Purpose
Gives quick operational visibility without replacing the main table.

---

## 12. Filter and Search Section
### Search
Search by:
- movement number
- source document number
- item code
- item name
- batch/serial
- warehouse
- location/bin

### Standard Filters
- Movement Type
- Source Module
- Source Document
- Item
- Warehouse
- From Location
- To Location
- Stock State
- Date Range
- Batch / Lot
- Serial-controlled only
- Created By / Performed By

### Advanced Filters
- GRN-linked only
- QC-linked only
- Return-linked only
- Scrap-linked only
- Rework-linked only
- Subcontracting-linked only
- Negative/exception movements (if relevant)
- stock impact direction (IN / OUT / MOVE)

### Filter Actions
- Apply
- Reset
- Clear All
- Save View (future)

---

## 13. Table Structure
This page should use a ledger-style operational table.

### Recommended Columns
- Movement Number
- Movement Date / Timestamp
- Movement Type
- Source Module
- Source Document Number
- Item Code
- Item Name
- Warehouse
- From Location
- To Location
- Quantity In
- Quantity Out
- Stock State From
- Stock State To
- Batch / Serial
- Performed By
- Actions

### Optional Columns
- UOM
- Branch / Plant
- Reason Code
- QC Reference
- Return / Scrap / Rework Reference
- Last Updated

### Column Behavior
- Movement Number should be clickable to a movement detail/drilldown if supported
- Source Document Number should be clickable to the original source page
- state changes should be visually clear (example: QC Hold → Available)
- quantity direction should be obvious

---

## 14. Suggested Default Sort
Recommended default:
- latest movement first

### Default Recommendation
- `movement_date DESC`

This is typically the most useful ledger behavior.

---

## 15. Pagination
Use server-driven pagination.

### Standard Controls
- page number
- page size
- total count
- next/previous

### Recommended Default Page Size
- 25 rows

---

## 16. Row Click / Drilldown Behavior
Default row click may open:
- movement detail drawer
or
- source document directly, depending on UX choice

### Recommended Pattern
Use a row action or quick drilldown drawer for movement detail, while keeping source document as a clickable linked reference.

---

## 17. Row Actions
Common row actions:
- View Movement Detail
- View Source Document
- View Item Stock
- View Batch/Serial Detail (if applicable)

### Important Rule
This page is primarily read-only / analytical.
Do not place operational stock mutation actions here unless explicitly designed later.

---

## 18. Role-Based UI Rules
### Store User
Should see:
- warehouse movement history
- source document references
- item/location details
- quantity direction and stock states

### QA Inspector
Should see:
- QC-linked movements
- stock-state changes from QC hold/rejected to accepted/scrap/rework
- no operational transfer/adjustment mutation buttons

### Procurement Team
Should see:
- GRN, return, and vendor-related movement effects
- no direct inventory mutation actions

### Management / Finance / Audit Viewer
Should see:
- read-only traceability
- export if permitted
- source link visibility

---

## 19. Movement Type Visibility Rules
The page should distinguish movement types clearly.

### Recommended Movement Types
- GRN Receipt
- QC Accepted
- QC Rejected / Blocked
- Stock Transfer
- Stock Adjustment
- Scrap
- Rework
- Purchase Return
- Sales Return
- Production Receipt
- Material Issue
- Subcontracting Issue
- Subcontracting Receipt

### Important Rule
Movement type labels must be standardized; avoid ambiguous generic labels like only “update”.

---

## 20. Stock State Visibility Rules
The page should support state-based stock transitions such as:
- Available
- Reserved
- QC Hold
- Rejected
- Rework
- Scrap Hold
- Blocked
- Vendor Location (subcontracting context)

This is crucial for auditability.

---

## 21. Empty State
### No Movements at All
Show:
- informative empty state
- explanation of stock movement tracking

### No Match After Filters
Show:
- “No stock movements match the current filters”
- clear filters action

---

## 22. Loading State
Use:
- table skeleton rows
- optional summary strip skeleton
- filter-loading indicators if needed

---

## 23. Error State
Possible errors:
- list fetch failed
- drilldown fetch failed
- export failed
- source document link unavailable

Display:
- clear message
- retry action
- preserve active filters where possible

---

## 24. No-Access State
If user lacks permission:
- show restricted-access state
- hide movement data
- disable export and source document drilldown as required

---

## 25. Data Requirements
This page needs:
- paginated movement rows
- item and warehouse/location references
- source document references
- movement type and stock-state transitions
- quantity direction data
- batch/serial context where applicable

---

## 26. API Dependencies
### Core APIs
- Get inventory movement list
- Get movement detail / drilldown
- Export movement list
- Get related source reference if not embedded

### Example Endpoint Needs
- `GET /api/v1/inventory-movements`
- `GET /api/v1/inventory-movements/{id}`
- `POST /api/v1/reports/export` or movement export endpoint

### Query Support Needed
- page
- per_page
- sort
- search
- movement_type
- source_module
- source_document
- item_id
- warehouse_id
- from_location_id
- to_location_id
- stock_state
- date_from
- date_to
- batch_no
- serial_no
- performed_by

---

## 27. Validation / Display Rules
### Display Rules
- quantity in and quantity out must be visually separated
- state transitions should be easy to read
- source references should always be visible if movement is source-driven
- exception movements should be visually distinguishable

### Consistency Rules
- movement totals should match source transaction outcomes
- drilldown should never contradict Stock Overview quantities
- links to source docs must remain stable

---

## 28. Navigation / Linked Behavior
This page should connect to:
- Stock Overview
- GRN Detail
- future QC Detail
- future Stock Adjustment Detail
- future Stock Transfer Detail
- purchase return / sales return detail (future)
- scrap / rework records (future)

### Example Navigation
- movement row → view GRN detail
- movement row → view stock overview for item/location
- stock overview drilldown → prefiltered stock movement list

---

## 29. Export / Print Behavior
### Export
Allowed for permitted roles.
Export should respect current filters.

### Print
Generally less important than export for this page.

---

## 30. Audit / Traceability Value
This page acts as a movement ledger and should strongly support:
- who moved stock
- when stock moved
- why stock moved
- what source document caused it
- how stock state changed

This is one of the key audit pages in the ERP.

---

## 31. Wireframe Notes
For wireframe execution:
- prioritize a clear ledger table
- make quantity direction and state transitions highly visible
- source document links should be easy to scan
- filter area may need advanced filter collapsible section
- movement detail drawer is recommended for quick inspection without leaving list context

---

## 32. Frontend Build Notes
### Reusable Components Suggested
- PageHeader
- FilterBar
- DataTable
- StatusBadge / StockStateBadge
- DrilldownDrawer
- EmptyState
- ErrorState

### State Handling Needed
- filter state
- search state
- pagination state
- sort state
- selected movement row
- drilldown open/close state
- export state

---

## 33. Backend Build Notes
Backend should ensure:
- movement rows are immutable and traceable
- source references are queryable and stable
- movement_type and stock_state values are normalized
- batch/serial context is supported where relevant
- list performance is acceptable for ledger usage
- exports can handle large filtered result sets efficiently

---

## 34. Completion Criteria
Stock Movement page design is complete when:
- header, filters, and table columns are fixed
- movement type and stock-state transitions are defined
- source document linking is clear
- role visibility is defined
- API dependencies are listed
- ledger/drilldown behavior is clear

---

## 35. Next Related Files
Recommended next files:
1. page_stock_adjustment.md
2. page_stock_transfer.md
3. txn_stock_adjustment.md
4. txn_stock_transfer.md
