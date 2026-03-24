# Frontend Component Library Plan

## 1. Purpose
This file defines the reusable frontend component system for the current ERP design slice.

It standardizes:
- shared layout components
- page-level components
- workflow/action components
- collaboration components
- procurement/inventory-specific business components
- component ownership and reuse rules

This is the file that prevents screen-by-screen frontend duplication.

---

## 2. Recommended Folder Location
Keep this file at:

`frontend/frontend_component_library_plan.md`

Component code should later be organized broadly like:

`src/shared/components/`
`src/shared/workflow/`
`src/shared/layout/`
`src/modules/procurement/components/`
`src/modules/inventory/components/`
`src/modules/quality/components/`

---

## 3. Scope Covered
This plan currently supports the first completed ERP vertical slice:

- PR List
- PR Detail
- PO List
- PO Detail
- GRN Entry
- GRN Detail
- Incoming QC
- Stock Overview

---

## 4. Component Design Principles
All components should follow these rules:

### Reusable before page-specific
If a pattern appears in more than one page, make it reusable.

### Role- and status-aware
Components should accept permission/status props where actions change by workflow state.

### Data-light presentation components
Keep business logic out of display components where possible.

### Enterprise density
Components should support dense operational data, not only marketing-style UI.

### Stable composition
Pages should be assembled from consistent building blocks.

---

## 5. Component Grouping Model

### A. Layout Components
Used across the whole app.

### B. Page Components
Used to structure list/detail/form pages.

### C. Workflow Components
Used for statuses, approvals, actions, linked-record flow.

### D. Collaboration Components
Used for comments, attachments, audit, activity.

### E. Domain Components
Used for procurement, warehouse, QC, and stock-specific UI.

---

# 6. Layout Components

## 6.1 AppShell
### Purpose
Wraps the entire ERP application shell.

### Responsibilities
- sidebar
- topbar
- content area
- responsive layout behavior
- optional contextual panel region

### Used By
- all pages

---

## 6.2 SidebarNav
### Purpose
Shows module navigation.

### Responsibilities
- module grouping
- active page highlighting
- role-aware visibility
- nested menu support

### Used By
- all pages

---

## 6.3 Topbar
### Purpose
Displays global app controls.

### Responsibilities
- user menu
- notifications
- global search
- context selector (future)
- quick actions (future)

---

## 6.4 Breadcrumbs
### Purpose
Shows page hierarchy.

### Responsibilities
- list/detail/create navigation context
- stable return path

---

# 7. Core Page Components

## 7.1 PageHeader
### Purpose
Standard page top section.

### Props
- title
- subtitle
- breadcrumb data
- primary actions
- secondary actions

### Used By
- list pages
- detail pages
- entry pages
- overview pages

---

## 7.2 ActionBar
### Purpose
Renders context-aware top actions.

### Props
- actions[]
- loading state
- permission flags
- status flags

### Used By
- PR Detail
- PO Detail
- GRN Detail

---

## 7.3 SummaryCardStrip
### Purpose
Shows compact KPI/status summary cards.

### Props
- cards[]
- loading
- clickable filter behavior

### Used By
- PR List
- PO List
- Stock Overview

---

## 7.4 FilterBar
### Purpose
Handles search, filters, reset, apply, saved views.

### Props
- filter definitions
- current values
- callbacks

### Used By
- PR List
- PO List
- Incoming QC
- Stock Overview

---

## 7.5 DataTable
### Purpose
Standard enterprise table component.

### Capabilities
- sortable columns
- server pagination
- row click
- row action menu
- bulk selection
- empty state slot
- loading state
- column config

### Used By
- PR List
- PO List
- Incoming QC
- Stock Overview

---

## 7.6 FormSection
### Purpose
Standard wrapper for grouped form fields.

### Used By
- GRN Entry
- future create/edit pages

---

## 7.7 StickyActionBar
### Purpose
Persistent bottom/top action controls for long forms.

### Used By
- GRN Entry
- future create PR / create PO pages

---

## 7.8 EmptyState
## 7.9 ErrorState
## 7.10 LoadingSkeleton
### Purpose
Shared feedback-state components.

### Used By
- all pages

---

# 8. Workflow Components

## 8.1 RecordStatusHeader
### Purpose
Displays record identifier + status + top metadata.

### Props
- record number
- status
- badges
- metadata
- actions slot

### Used By
- PR Detail
- PO Detail
- GRN Detail

---

## 8.2 StatusBadge
### Purpose
Standard visual badge for workflow states.

### Supported Examples
- Draft
- Submitted
- Approved
- Rejected
- Pending
- QC Pending
- Accepted
- Cancelled
- Closed

### Used By
- list tables
- detail headers
- linked-record cards

---

## 8.3 ApprovalTimeline
### Purpose
Shows approval history and workflow sequence.

### Used By
- PR Detail
- PO Detail

---

## 8.4 LinkedRecordsPanel
### Purpose
Shows upstream/downstream document relationships.

### Responsibilities
- linked record cards
- quick links
- status indicators
- document counts

### Used By
- PR Detail
- PO Detail
- GRN Detail

---

## 8.5 ConfirmDialog
### Purpose
Reusable confirmation dialog for sensitive actions.

### Used By
- submit
- approve
- cancel
- close
- send to QC
- accept/reject

---

## 8.6 RejectReasonModal
### Purpose
Structured rejection reason capture.

### Used By
- PR Detail/List
- PO Detail/List
- GRN Detail
- Incoming QC

---

# 9. Collaboration Components

## 9.1 CommentThreadPanel
### Purpose
Displays and adds comments.

### Responsibilities
- comment list
- add comment
- timestamps
- author display
- comment type labels

### Used By
- PR Detail
- PO Detail
- GRN Detail
- Incoming QC (optional)

---

## 9.2 AttachmentPanel
### Purpose
Displays and uploads file attachments.

### Responsibilities
- attachment list
- upload action
- metadata
- download/view actions

### Used By
- PR Detail
- PO Detail
- GRN Entry
- GRN Detail
- Incoming QC

---

## 9.3 AuditTrailPanel
### Purpose
Displays record event history.

### Responsibilities
- event list
- actor
- timestamp
- event description
- value change summary

### Used By
- PR Detail
- PO Detail
- GRN Detail

---

# 10. Domain Components: Procurement

## 10.1 SourceReferenceCard
### Purpose
Shows source document reference like PR on PO, PO on GRN.

### Used By
- PO Detail
- GRN Entry
- GRN Detail

---

## 10.2 VendorCommercialSummary
### Purpose
Displays vendor, payment, delivery, and commercial terms.

### Used By
- PO Detail

---

## 10.3 TotalsSummaryCard
### Purpose
Displays subtotal, tax, discount, and grand total.

### Used By
- PO Detail
- future invoice/order pages

---

## 10.4 ReceiptProgressWidget
### Purpose
Shows PO receipt progress.

### Used By
- PO Detail

---

## 10.5 LineItemGrid
### Purpose
Reusable line-item presentation/edit grid.

### Modes
- read-only detail mode
- editable entry mode

### Used By
- PR Detail
- PO Detail
- GRN Entry
- GRN Detail

---

# 11. Domain Components: Quality / Inventory

## 11.1 ReceiptSummaryCard
### Purpose
Shows GRN receipt summary.

### Used By
- GRN Detail

---

## 11.2 QCDispositionPanel
### Purpose
Handles QC outcome display and actions.

### Responsibilities
- accepted qty
- rejected qty
- inspection notes
- accept/reject actions

### Used By
- GRN Detail
- Incoming QC

---

## 11.3 StockImpactPanel
### Purpose
Shows how accepted/rejected quantities affected stock.

### Used By
- GRN Detail

---

## 11.4 StockStatusBadge
### Purpose
Shows stock state labels.

### Supported Examples
- Available
- Reserved
- QC Hold
- Rejected
- Blocked

### Used By
- Stock Overview
- drilldown panels
- stock-linked sections

---

## 11.5 DrilldownDrawer
### Purpose
Shows row-level detail without leaving overview context.

### Used By
- Stock Overview
- future movement pages

---

# 12. Incoming QC Page-Specific Composite

## 12.1 InspectionActionPanel
### Purpose
Operational QC action panel for selected receipt line.

### Responsibilities
- accepted qty input
- rejected qty input
- inspection notes
- evidence upload
- action buttons

### Used By
- Incoming QC

### Note
This can start as page-specific, then become reusable if similar QC pages expand later.

---

# 13. Component Reuse Matrix

## Shared Across Many Pages
- PageHeader
- FilterBar
- DataTable
- StatusBadge
- ConfirmDialog
- CommentThreadPanel
- AttachmentPanel
- AuditTrailPanel

## Shared Across Detail Pages
- RecordStatusHeader
- ActionBar
- LinkedRecordsPanel
- LineItemGrid

## Shared Across Workflow Pages
- ApprovalTimeline
- RejectReasonModal

## Shared Across Inventory/Quality Pages
- ReceiptSummaryCard
- QCDispositionPanel
- StockImpactPanel
- StockStatusBadge

---

# 14. Props and Composition Guidance

## Keep presentation components generic
Examples:
- `StatusBadge(status)`
- `SummaryCardStrip(cards)`
- `DataTable(columns, rows, loading)`

## Keep domain components semi-structured
Examples:
- `VendorCommercialSummary(data)`
- `ReceiptSummaryCard(data)`
- `SourceReferenceCard(sourceType, sourceData)`

## Keep page logic outside shared components
Fetch data in pages or hooks, then pass shaped props into components.

---

# 15. State Ownership Guidance

## Page owns:
- queries
- mutations
- route params
- page-level filters
- modal open/close
- permission evaluation

## Component owns:
- local display toggles
- internal UI interaction state where isolated
- field focus/expand/collapse behavior

### Important Rule
Do not put workflow transition logic inside deeply nested dumb components.

---

# 16. Styling and Visual Consistency Rules
- dense but readable spacing
- consistent header/action hierarchy
- strong status visibility
- clear contrast between read-only detail and editable entry
- standard spacing for summary cards, tables, and contextual panels
- visual emphasis on exceptions: rejected, QC pending, low stock, blocked stock

---

# 17. Accessibility and Keyboard Rules
- tables should support keyboard navigation where practical
- dialogs should trap focus
- action buttons should have clear labels
- status badges should not rely only on color
- form validation should be readable and announced clearly

---

# 18. Build Sequence Recommendation

## Phase 1: Core Shared Components
- PageHeader
- FilterBar
- DataTable
- StatusBadge
- ConfirmDialog
- EmptyState / ErrorState / LoadingSkeleton

## Phase 2: Workflow + Collaboration
- RecordStatusHeader
- ActionBar
- ApprovalTimeline
- LinkedRecordsPanel
- CommentThreadPanel
- AttachmentPanel
- AuditTrailPanel
- RejectReasonModal

## Phase 3: Domain Components
- SourceReferenceCard
- VendorCommercialSummary
- TotalsSummaryCard
- ReceiptProgressWidget
- ReceiptSummaryCard
- QCDispositionPanel
- StockImpactPanel
- StockStatusBadge

## Phase 4: Composite Page Assemblies
- PR Detail composition
- PO Detail composition
- GRN Detail composition
- Incoming QC composition
- Stock Overview drilldown composition

---

# 19. Ownership Suggestion
If multiple frontend developers are involved:

### Shared UI Owner
Maintains:
- layout
- generic page components
- workflow/shared components

### Domain UI Owner
Maintains:
- procurement business components
- inventory/QC business components

### Rule
Shared components should not be changed in page-specific ways without review.

---

# 20. Completion Criteria
This file is complete when:
- component groups are defined
- reusable vs page-specific boundaries are clear
- major pages map cleanly to reusable building blocks
- build sequence is clear
- future expansion remains manageable

---

# 21. Next Related Files
Recommended next files:
1. frontend_route_map.md
2. frontend_permission_logic_matrix.md
3. api_page_dependency_matrix.md
4. frontend_state_management_plan.md
