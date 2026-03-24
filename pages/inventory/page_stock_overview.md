# Page: Stock Overview

## 1. Page Purpose
The Stock Overview page is the main visibility screen for inventory after procurement receipt and quality disposition. It allows authorized users to:
- view current stock by item, warehouse, location, batch, or serial context
- understand available, reserved, QC-hold, and rejected stock positions
- trace stock back to source receipt documents such as GRN
- identify shortages, overstock, aging, and blocked stock
- navigate to stock movement details and related source documents

This page is the operational bridge between GRN/QC completion and day-to-day inventory visibility.

---

## 2. Source Alignment
This page must remain aligned with:
- Step 2 – Inventory Module
- Step 4 – Inventory Movement Flow
- Step 5 – Screen Map
- Step 8 – Backend API
- Step 9 – Frontend Development
- txn_goods_receipt_note.md
- page_grn_detail.md
- page_incoming_qc.md
- role_store_user.md
- role_qa_inspector.md

---

## 3. Module
- Inventory

---

## 4. Recommended Folder Location
This file should be kept at:

`pages/inventory/page_stock_overview.md`

### Why this location
Even though stock is influenced by procurement and QC, this page belongs to the Inventory module because it is the current-state stock visibility screen, not a document-entry screen.

---

## 5. Page Type
- Standard ERP List / Analysis Page

This page follows a hybrid pattern:
- page header + action bar
- KPI/summary strip
- filters/search
- stock grid/table
- optional right-side detail drawer or drill-down section
- linked movement/source navigation

---

## 6. Primary Roles
### Main Users
- Store User / Warehouse User
- Store Manager
- Production Planner
- Procurement Team (view)
- QA Team (view QC stock positions)

### Secondary Users
- Management
- Finance (view for stock valuation context if permitted)
- Admin (support only)

---

## 7. Business Goal of This Page
This page supports the following business outcomes:
- real-time stock visibility
- warehouse/location-level control
- clear differentiation between usable stock and blocked/QC stock
- traceable stock source visibility
- operational planning for procurement, production, and dispatch

---

## 8. Entry Points
Users may reach this page from:
- sidebar → Inventory → Stock Overview
- dashboard stock widget
- GRN Detail → View Stock Impact
- Incoming QC / accepted receipt result
- global search
- notification links for stock alerts

---

## 9. Route / Navigation Position
### Suggested Route
- `/inventory/stock-overview`

### Related Routes
- `/inventory/stock-movements`
- `/inventory/stock-overview/{item_id}` (optional detail drill-down)
- `/procurement/grn/{id}`

---

## 10. Page Header
Recommended header content:
- breadcrumb
- page title: Stock Overview
- subtitle: current inventory by item and location
- optional actions: Export, Refresh, Saved Views

### Optional Primary Actions
Depending on permission and future scope:
- Stock Adjustment
- Stock Transfer
- View Ledger

---

## 11. KPI / Summary Strip
Recommended summary cards:
- Total On-Hand Stock
- Available Stock
- Reserved Stock
- QC Hold Stock
- Rejected / Blocked Stock
- Low Stock Items
- Overstock Items

### Purpose
This helps users understand stock health before drilling into rows.

---

## 12. Filter and Search Section
### Search
Search by:
- item code
- item name
- batch number
- serial number
- warehouse
- GRN / source document reference (if supported)

### Standard Filters
- Warehouse
- Location / Bin
- Item Category
- Item
- Batch / Serial controlled items only
- Stock Status (Available / Reserved / QC Hold / Rejected)
- Low Stock flag
- Zero Stock hide/show
- Date / As-of date (future)
- Source Type (GRN / Adjustment / Transfer, future)

### Advanced Filters
- QC Hold only
- Recently updated stock
- Negative stock exceptions (if ever allowed)
- Aging bucket
- Expiry range for expiry-controlled items

---

## 13. Table Structure
This page should use a wide operational table.

### Recommended Columns
- Item Code
- Item Name
- Category
- UOM
- Warehouse
- Bin / Location
- Batch No
- Serial Context
- On Hand Qty
- Available Qty
- Reserved Qty
- QC Hold Qty
- Rejected / Blocked Qty
- Reorder Level
- Last Movement Date
- Source / Drilldown Action

### Optional Columns
- Expiry Date
- Valuation Rate
- Stock Value
- Preferred Vendor
- Last GRN Reference
- Last Updated By

---

## 14. Row Click / Drilldown Behavior
Default row action should open a drill-down or stock movement view showing:
- movement history
- source GRN reference
- linked QC result
- stock status breakdown by source

### Suggested Navigation
- Stock row → Stock Movement / Ledger detail
- Source reference → GRN Detail
- Item → Item Master / Item stock detail

---

## 15. Row Actions
Possible row actions:
- View Stock Movement
- View Source GRN
- View Item Master
- Transfer Stock (future, permission-based)
- Adjust Stock (future, permission-based)

### Important Rule
This page is mostly analytical/operational, not a high-mutation page. Most direct stock changes should happen through dedicated transactions.

---

## 16. Role-Based UI Rules
### Store User
Should see:
- warehouse/location stock
- available/QC/rejected quantities
- source references
- movement drill-down

### QA Inspector
Should see:
- QC Hold stock
- accepted/rejected stock outcomes
- source GRN / QC linkage
- no direct warehouse movement actions unless policy allows

### Procurement Team
Should see:
- stock availability relevant to purchasing
- GRN-linked stock effects
- low stock visibility
- no direct stock mutation actions

### Management / Viewer
Should see:
- summary cards
- filters
- export where permitted
- no operational mutation actions

---

## 17. Status / Stock-Type Visibility Rules
This page should distinguish stock buckets clearly:
- Available
- Reserved
- QC Hold
- Rejected / Blocked
- In Transit (future)
- Damaged (future)

### Important Rule
Do not present all stock as one number. Operational decisions depend on stock state.

---

## 18. Empty / Loading / Error / No-Access States
### Empty State
- No stock records found
- No stock for current filters

### Loading State
- summary strip skeleton
- table skeleton rows

### Error State
- stock fetch failed
- filter fetch failed
- movement drilldown failed

### No-Access State
- show restricted-access message
- hide stock data

---

## 19. Data Requirements
This page needs:
- current stock balances
- quantity split by stock state
- item and warehouse metadata
- low-stock / reorder indicators
- last movement metadata
- source reference summary for drilldown

---

## 20. API Dependencies
### Core APIs
- Get stock overview list
- Get stock summary totals
- Get stock detail / movement drilldown
- Get low-stock alerts
- Export stock overview

### Example Endpoint Needs
- `GET /api/v1/stock-balances`
- `GET /api/v1/stock-balances/summary`
- `GET /api/v1/stock-balances/{id}`
- `GET /api/v1/inventory-movements`
- `GET /api/v1/reports/inventory-aging`
- `POST /api/v1/reports/export`

---

## 21. Validation / Display Rules
### Stock Visibility Rules
- Available stock should exclude rejected stock
- QC Hold stock should remain separate from unrestricted stock
- Accepted GRN quantity should flow here correctly
- Rejected GRN quantity should not inflate usable stock

### Display Rules
- low-stock rows should be visually highlighted
- QC-hold rows should be clearly marked
- zero-stock rows optional based on filter
- source document links should be easy to access

---

## 22. Linked Navigation Rules
This page should connect to:
- GRN Detail (source receipt)
- Incoming QC (if stock is in QC hold and action is pending)
- stock movement / ledger page
- item detail page
- future stock transfer / adjustment pages

---

## 23. Frontend Build Notes
Recommended reusable components:
- PageHeader
- SummaryCardStrip
- FilterBar
- DataTable
- StockStatusBadge
- DrilldownDrawer or DetailPanel
- EmptyState
- ErrorState

### State Handling Needed
- filter state
- pagination state
- summary refresh state
- drilldown state
- export state

---

## 24. Backend Build Notes
Backend should ensure:
- stock is aggregated correctly by item/location/state
- QC hold and rejected stock are not mixed into available stock
- linked source references are queryable
- summary counts are efficient
- drilldown can retrieve source document references without N+1 problems

---

## 25. Completion Criteria
Stock Overview page design is complete when:
- stock summary and table structure are fixed
- quantity-state distinctions are explicit
- drilldown/source references are defined
- role visibility rules are defined
- API dependencies are listed
- GRN/QC → stock transition is reflected clearly

---

## 26. Next Related Files
Recommended next files:
1. transactions/txn_inventory_movement.md
2. pages/inventory/page_stock_movement.md
3. pages/inventory/page_stock_adjustment.md
4. pages/inventory/page_stock_transfer.md
