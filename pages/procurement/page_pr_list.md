# Page: PR List

## 1. Page Purpose
The PR List page is the main operational listing screen for Purchase Requisitions (PR). It allows authorized users to:
- view all relevant PR records
- search and filter PRs
- create new PRs
- open PR detail pages
- monitor PR status and approval progress
- identify PRs that require action, follow-up, or conversion to PO

This page is the primary entry point into the Purchase Requisition workflow.

---

## 2. Source Alignment
This page must remain aligned with:
- Step 2 – Procurement Module
- Step 3 – User Role Design
- Step 4 – Procurement Flow
- Step 5 – Screen Map
- Step 6 – Wireframe Design
- Step 8 – Backend API
- Step 9 – Frontend Development
- role_procurement_executive.md
- role_procurement_manager.md
- txn_purchase_requisition.md

---

## 3. Module
- Procurement

---

## 4. Page Type
- Standard ERP List Page

This page follows the enterprise list-page pattern:
- breadcrumb + page title
- action bar
- status summary strip
- filters/search
- data table
- row actions
- empty/loading/error states

---

## 5. Primary Roles
### Main Users
- Procurement Executive
- Procurement Manager

### Secondary Users
- Management (view/reporting access if permitted)
- Admin (configuration or audit support only if policy allows)

---

## 6. Business Goal of This Page
This page supports the following business outcomes:
- fast visibility into all PRs
- easy identification of Draft / Submitted / Approved / Rejected PRs
- quick navigation to PR detail
- faster approval follow-up
- operational monitoring of PRs pending conversion to PO

---

## 7. Entry Points
Users may reach this page from:
- left sidebar navigation → Procurement → PR List
- dashboard “My PRs” widget
- dashboard “Pending PR Approval” widget
- approval work queue
- global search result
- breadcrumb return from PR Detail page
- notification link related to PR action

---

## 8. Route / Navigation Position
### Suggested Route
- `/procurement/purchase-requests`

### Sidebar Position
- Procurement
  - PR List

### Related Routes
- `/procurement/purchase-requests/new`
- `/procurement/purchase-requests/:id`

---

## 9. Page Header Structure
The top section should include:
- breadcrumb
- page title: Purchase Requisitions
- optional subtitle: view and manage procurement requests
- primary CTA: Create PR
- optional secondary actions: Export, Saved Views, Refresh

### Example Header Actions
- Create PR
- Export List
- Refresh
- Manage Filters / Saved View (future)

---

## 10. KPI / Status Summary Strip
A compact summary strip should appear above the table.

### Recommended Summary Cards
- Total PRs
- Draft
- Submitted
- Approved
- Rejected
- Awaiting PO Conversion

### Purpose
This helps users quickly understand workload and status distribution.

### Click Behavior
Each summary card may act as a quick filter.

Examples:
- click “Submitted” → table filtered to Submitted PRs
- click “Approved” → show PRs eligible for PO conversion

---

## 11. Filter and Search Section
The PR List page should support high-utility ERP filtering.

### Basic Search
Search by:
- PR Number
- requester name
- department
- keyword in justification
- item name / item code (if supported through indexed search)

### Standard Filters
- Status
- Date Range
- Requested By
- Department
- Branch
- Priority
- Required By Date
- Approval State
- PO Conversion State (future)
- My Records / Team Records

### Advanced Filters (future-ready)
- Cost Center
- Project
- Plant / Factory
- Aging Bucket
- Vendor Intent / Preferred Vendor

### Filter Actions
- Apply
- Reset
- Save View (future)
- Clear All

---

## 12. Table Structure
This page should use a transaction-heavy table, not a card layout.

### Recommended Columns
- PR Number
- PR Date
- Requested By
- Department
- Branch
- Priority
- Total Line Count
- Total Estimated Amount
- Required By Date
- Status
- Approval Age / Pending Since
- Linked PO Indicator
- Last Updated
- Actions

### Optional Columns
- Cost Center
- Project Reference
- Plant / Warehouse Context
- Rejection Reason Snapshot
- Created By

### Column Behavior
- status should use clear status badge
- PR Number should be clickable to open detail page
- columns should be sortable where meaningful
- column visibility may be configurable in future

---

## 13. Suggested Default Sort
Default table order:
- newest created first
or
- most urgent actionable records first for certain roles

### Recommended Default
- `created_at DESC`

### Alternative Role-Based Sort
For approvers:
- Submitted first, oldest pending first

---

## 14. Pagination
Use server-driven pagination.

### Standard Pagination Controls
- page number
- rows per page
- total count
- next / previous

### Recommended Default Page Size
- 20 rows

---

## 15. Row Click Behavior
Default row click:
- open PR Detail page

### Route Pattern
- `/procurement/purchase-requests/{id}`

---

## 16. Row Actions
Row actions should depend on role and status.

### Common Row Actions
- View
- Edit
- Submit
- Approve
- Reject
- Cancel
- Convert to PO
- Print / Export
- View Linked PO(s)

### Status + Role Logic
#### Procurement Executive
- Draft → View, Edit, Submit, Cancel
- Submitted → View only
- Approved → View, Convert to PO, View Linked PO
- Rejected → View, Revise (if enabled), Cancel

#### Procurement Manager
- Draft → View only
- Submitted → View, Approve, Reject
- Approved → View
- Rejected → View
- Cancelled → View

### Important Rule
Do not overload the row action menu with too many risky actions; detail page remains the full-control workspace.

---

## 17. Bulk Actions
Bulk actions should be limited and permission-aware.

### Possible Bulk Actions
- Bulk Submit (Draft only)
- Bulk Export
- Bulk Print
- Bulk Approve (only if policy explicitly allows and business confirms it)

### Guardrails
- mixed-status selection should block incompatible bulk actions
- only authorized roles may see bulk actions
- approvals may remain single-record only if risk is high

---

## 18. Primary CTA
### Button
- Create PR

### Visibility
Visible to:
- Procurement Executive
- any other maker role explicitly granted create permission

Hidden from:
- approval-only users if no create permission

### Action
Navigates to:
- `/procurement/purchase-requests/new`

---

## 19. Status-Based UI Rules
### Draft
- editable by maker
- show submit-ready indicators if valid
- show edit/cancel actions

### Submitted
- lock direct edit
- show pending approval state
- approver sees approval actions

### Approved
- mark as ready for downstream conversion
- highlight Convert to PO path where allowed

### Rejected
- show rejection reason indicator
- optionally show resubmission path if revision policy exists

### Cancelled
- view only
- dim or archive-style treatment if needed

---

## 20. Role-Based UI Rules
### Procurement Executive
Should see:
- Create PR button
- own/team PRs depending on scope
- Draft and Approved actions relevant to maker role
- conversion path to PO after approval

Should not see:
- approval buttons

### Procurement Manager
Should see:
- submitted PR queue clearly
- approve/reject actions
- aging indicators
- audit/priority cues

May not need:
- create button, if manager is approval-focused only

### Management / Read-Only Viewer
Should see:
- filtered list
- statuses
- totals
- export if authorized
- no workflow actions

---

## 21. Visual Priority Rules
The UI should visually prioritize:
1. Submitted PRs awaiting approval
2. Approved PRs awaiting PO conversion
3. Rejected PRs needing correction
4. Draft PRs still incomplete

This helps operational users focus on what matters.

---

## 22. Empty State
### No Records At All
Show:
- friendly empty state
- explanation of what PRs are
- Create PR CTA for authorized users

### No Match After Filter
Show:
- “No purchase requisitions match current filters”
- clear filters action

---

## 23. Loading State
Use:
- table skeleton rows
- loading indicator in summary strip
- disabled filter actions while request is in flight where necessary

---

## 24. Error State
Possible errors:
- list fetch failed
- filter request failed
- permissions issue
- server unavailable

Display:
- clear error message
- retry action
- preserve filters if possible

---

## 25. No-Access State
If the user does not have permission:
- show restricted-access message
- hide table data
- do not expose unauthorized actions

---

## 26. Data Requirements
This page needs:
- paginated PR list
- counts by status
- user/role permissions for row actions
- approval state data
- conversion status data (if supported)
- filter option data (department, branch, users, etc.)

---

## 27. API Dependencies
### Core APIs
- Get PR list
- Get PR summary counts
- Submit PR
- Approve PR
- Reject PR
- Cancel PR
- Export PR list
- Get linked PO summary for PR row (direct or embedded)

### Example Endpoint Needs
- `GET /api/v1/purchase-requests`
- `GET /api/v1/purchase-requests/summary`
- `POST /api/v1/purchase-requests/{id}/submit`
- `POST /api/v1/purchase-requests/{id}/approve`
- `POST /api/v1/purchase-requests/{id}/reject`
- `POST /api/v1/purchase-requests/{id}/cancel`

### Query Support Needed
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

---

## 28. Validation / Action Rules on This Page
### Before Submit from List
- record must be in Draft
- user must have submit permission
- record must pass backend validation

### Before Approve from List
- record must be in Submitted
- user must have approval permission
- maker-checker rule must pass

### Before Reject from List
- rejection reason/comment should be collected
- status must be Submitted

### Before Convert to PO
- PR must be Approved
- user must have PO creation/conversion permission

---

## 29. Suggested Modals / Drawers
### Optional Quick Actions
- Submit Confirmation Modal
- Reject with Reason Modal
- Cancel Confirmation Modal
- Quick Preview Drawer (future)
- Quick PO Conversion Drawer / Shortcut (future)

### Note
Heavy business review should still happen on the detail page when complexity is high.

---

## 30. Export / Print Behavior
### Export
Allowed only for permitted roles.
Export should respect active filters.

### Print
Optional; less important on list page than detail page.

---

## 31. Audit / Traceability Signals on List
The table should show enough metadata to support operational traceability:
- who requested
- when created
- current status
- last updated
- pending since
- linked PO presence

Detailed audit remains on detail page.

---

## 32. Notification / Deep Link Behavior
This page should support deep links from:
- approval notifications
- rejection notifications
- dashboard widgets
- global search
- PO conversion reminders

If deep-linked with filters, page should preserve context if possible.

---

## 33. Wireframe Notes
For wireframe execution:
- use wide enterprise table layout
- summary strip above filters
- compact but readable filters
- clear sticky header if page is long
- status badges visually strong
- action menu compact
- Create PR CTA top-right
- table should prioritize scannability over decoration

---

## 34. Frontend Build Notes
### Reusable Components Suggested
- PageHeader
- SummaryCardStrip
- FilterBar
- DataTable
- StatusBadge
- RowActionMenu
- BulkActionBar
- EmptyState
- ErrorState
- ConfirmDialog

### State Handling Needed
- filter state
- pagination state
- sort state
- permission-aware action rendering
- mutation refresh after row action
- export trigger state

---

## 35. Backend Build Notes
Backend should ensure:
- filtered list performance
- role-based row/action permission resolution
- summary counts are efficient
- status transition endpoints enforce rules
- linked PO indicator is available without N+1 issues
- auditability is preserved on list-triggered actions

---

## 36. Dependencies
This page depends on:
- PR transaction logic
- role definitions for Procurement Executive / Manager
- approval rules
- PR summary/reporting capability
- Step 9 frontend architecture patterns

---

## 37. Completion Criteria
PR List page design is complete when:
- header, filters, summary strip, and table columns are fixed
- row actions are defined by role and status
- pagination/search/filter behavior is clear
- empty/loading/error/no-access states are defined
- API dependencies are listed
- downstream PR → PO visibility is included

---

## 38. Next Related Files
Recommended next files:
1. page_pr_detail.md
2. page_po_list.md
3. page_po_detail.md
4. txn_goods_receipt_note.md
