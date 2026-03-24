# Frontend Build Mapping: Pages, Components, APIs, and State

## 1. Purpose
This file converts the current ERP design slice into direct frontend implementation guidance.

It maps each page to:
- route
- required components
- required APIs
- key UI state
- role-based behavior
- mutation flow
- linked navigation behavior

This is the handoff layer between design documents and actual frontend development.

---

## 2. Recommended Folder Location
Keep this file at:

`frontend/frontend_page_component_api_mapping.md`

If you do not yet have a `frontend/` folder, create one.

---

## 3. Scope Covered
This file currently covers the first completed ERP vertical slice:

- PR List
- PR Detail
- PO List
- PO Detail
- GRN Entry
- GRN Detail
- Incoming QC
- Stock Overview

---

## 4. Standard Mapping Template
Each page should define:
- page name
- route
- primary roles
- core components
- data queries
- mutations/actions
- local UI state
- derived UI state
- permission rules
- navigation/deep link behavior
- response refresh behavior

---

# 5. Page Mapping

## 5.1 PR List

### Route
`/procurement/purchase-requests`

### Primary Roles
- Procurement Executive
- Procurement Manager

### Main Components
- PageHeader
- SummaryCardStrip
- FilterBar
- DataTable
- StatusBadge
- RowActionMenu
- ConfirmDialog
- EmptyState
- ErrorState

### Required Queries
- getPRList
- getPRSummaryCounts
- getCurrentUserPermissions

### Required Mutations
- submitPR
- approvePR
- rejectPR
- cancelPR
- exportPRList

### Local UI State
- filters
- search text
- current page
- page size
- sort
- selected rows
- active modal
- reject reason input

### Derived UI State
- whether Create button is visible
- whether row actions are enabled
- whether summary card filters are active
- whether bulk actions are available

### Refresh Rules
- refetch list after submit/approve/reject/cancel
- refetch summary strip after mutation
- preserve current filters after mutation

---

## 5.2 PR Detail

### Route
`/procurement/purchase-requests/{id}`

### Primary Roles
- Procurement Executive
- Procurement Manager

### Main Components
- RecordStatusHeader
- ActionBar
- SummarySection
- LineItemGrid
- ApprovalTimeline
- CommentThreadPanel
- AttachmentPanel
- AuditTrailPanel
- LinkedRecordsPanel
- ConfirmDialog
- RejectReasonModal

### Required Queries
- getPRDetail
- getPRComments
- getPRAttachments
- getPRAuditLog
- getPRLinkedPOs
- getCurrentUserPermissions

### Required Mutations
- updatePRDraft
- submitPR
- approvePR
- rejectPR
- cancelPR
- addPRComment
- uploadPRAttachment

### Local UI State
- edit mode
- unsaved changes state
- comment input
- attachment upload state
- reject modal open/close
- action loading state

### Derived UI State
- whether record is editable
- whether submit button is visible
- whether approve/reject buttons are visible
- whether Convert to PO is visible
- whether linked PO block should be emphasized

### Refresh Rules
- refetch detail after mutation
- refetch linked PO section after PO conversion
- refetch comments/attachments after add/upload

---

## 5.3 PO List

### Route
`/procurement/purchase-orders`

### Primary Roles
- Procurement Executive
- Procurement Manager
- Store User (view)

### Main Components
- PageHeader
- SummaryCardStrip
- FilterBar
- DataTable
- StatusBadge
- RowActionMenu
- ConfirmDialog

### Required Queries
- getPOList
- getPOSummaryCounts
- getCurrentUserPermissions

### Required Mutations
- submitPO
- approvePO
- rejectPO
- cancelPO
- exportPOList

### Local UI State
- filters
- search text
- current page
- sort
- selected rows
- active modal

### Derived UI State
- Create PO visibility
- approval action visibility
- receipt-readiness indicators
- linked GRN indicator

### Refresh Rules
- refetch list and summary after workflow actions
- preserve filters after mutation

---

## 5.4 PO Detail

### Route
`/procurement/purchase-orders/{id}`

### Primary Roles
- Procurement Executive
- Procurement Manager
- Store User
- QA Inspector (view context)

### Main Components
- RecordStatusHeader
- SourceReferenceCard
- VendorCommercialSummary
- TotalsSummaryCard
- LineItemGrid
- ReceiptProgressWidget
- ApprovalTimeline
- LinkedRecordsPanel
- CommentThreadPanel
- AttachmentPanel
- AuditTrailPanel
- ActionBar
- ConfirmDialog
- RejectReasonModal

### Required Queries
- getPODetail
- getPOComments
- getPOAttachments
- getPOAuditLog
- getPOLinkedGRNs
- getPOReceiptProgress
- getCurrentUserPermissions

### Required Mutations
- updatePODraft
- submitPO
- approvePO
- rejectPO
- cancelPO
- closePO
- addPOComment
- uploadPOAttachment

### Local UI State
- edit mode
- unsaved changes
- comment input
- action loading state
- reject modal state

### Derived UI State
- whether draft edit is allowed
- whether approval actions are visible
- whether Create GRN is visible
- whether receipt progress widget should be shown
- whether linked GRN block should be emphasized

### Refresh Rules
- refetch detail after mutation
- refetch linked GRNs and receipt progress after GRN creation
- refetch comments/attachments after updates

---

## 5.5 GRN Entry

### Route
`/procurement/grn/new?source_po={id}`

### Primary Roles
- Store User
- Store Manager

### Main Components
- PageHeader
- SourceReferenceCard
- FormSection
- LineItemGrid
- StickyActionBar
- ValidationMessageBlock
- AttachmentPanel
- ConfirmDialog

### Required Queries
- getPOReceivableContext
- getWarehouseOptions
- getLocationOptions
- getItemBatchSerialRules
- getCurrentUserPermissions

### Required Mutations
- createGRN
- updatePendingGRN
- markGRNReceived
- sendGRNToQC
- uploadGRNAttachment

### Local UI State
- form values
- line item values
- selected warehouse/bin
- batch/serial input state
- discrepancy note state
- action loading state
- validation error state

### Derived UI State
- whether Send to QC is visible
- whether quantity reconciliation passes
- whether Mark Received is enabled
- whether QC-required lines exist

### Refresh Rules
- redirect to GRN Detail after successful create/mark received
- preserve source PO context during form lifecycle

---

## 5.6 GRN Detail

### Route
`/procurement/grn/{id}`

### Primary Roles
- Store User
- Store Manager
- QA Inspector
- Procurement Team (view)

### Main Components
- RecordStatusHeader
- SourceReferenceCard
- ReceiptSummaryCard
- LineItemGrid
- QCDispositionPanel
- StockImpactPanel
- LinkedRecordsPanel
- CommentThreadPanel
- AttachmentPanel
- AuditTrailPanel
- ActionBar
- ConfirmDialog
- RejectReasonModal

### Required Queries
- getGRNDetail
- getGRNComments
- getGRNAttachments
- getGRNAuditLog
- getGRNLinkedInventoryMovements
- getGRNLinkedQCRecords
- getGRNStockImpact
- getCurrentUserPermissions

### Required Mutations
- updatePendingGRN
- markGRNReceived
- sendGRNToQC
- acceptGRN
- rejectGRN
- cancelGRN
- addGRNComment
- uploadGRNAttachment

### Local UI State
- edit mode for pending receipt
- action loading state
- reject reason state
- comment input
- attachment upload state

### Derived UI State
- whether Accept is visible
- whether Reject is visible
- whether Send to QC is visible
- whether stock impact panel should show posted state
- whether source PO link is visible

### Refresh Rules
- refetch detail after receipt/QC actions
- refetch stock impact after acceptance
- refetch linked QC/inventory references after mutation

---

## 5.7 Incoming QC

### Route
`/procurement/incoming-qc`

### Primary Roles
- QA Inspector

### Main Components
- PageHeader
- FilterBar
- DataTable or QueueList
- InspectionActionPanel
- QtySplitInputs
- CommentBox
- EvidenceUploadPanel
- ConfirmDialog

### Required Queries
- getIncomingQCPendingList
- getGRNQCContext
- getCurrentUserPermissions

### Required Mutations
- submitQCResult
- acceptQC
- rejectQC
- uploadQCEvidence
- addQCComment

### Local UI State
- selected row / selected GRN line
- accepted qty
- rejected qty
- inspection note
- evidence upload state
- action loading state

### Derived UI State
- whether row is actionable
- whether qty reconciliation is valid
- whether Save QC Result is enabled

### Refresh Rules
- refetch queue after QC decision
- refetch linked GRN detail status if opened from GRN deep link

---

## 5.8 Stock Overview

### Route
`/inventory/stock-overview`

### Primary Roles
- Store User
- Store Manager
- Procurement Team (view)
- QA Team (view)
- Production Planner (view)

### Main Components
- PageHeader
- SummaryCardStrip
- FilterBar
- DataTable
- StockStatusBadge
- DrilldownDrawer or DetailPanel
- EmptyState
- ErrorState

### Required Queries
- getStockOverview
- getStockSummary
- getStockMovementDrilldown
- getLowStockAlerts
- getCurrentUserPermissions

### Required Mutations
- exportStockOverview
- future: createStockAdjustment
- future: createStockTransfer

### Local UI State
- filters
- current page
- page size
- sort
- selected stock row
- drilldown panel open/close
- export state

### Derived UI State
- whether QC hold rows should be highlighted
- whether low stock rows should be highlighted
- whether mutation actions are visible

### Refresh Rules
- refetch summary and list after stock-impacting events
- refresh drilldown when selected stock row changes

---

## 6. Common Permission Utility Rules
The frontend should implement shared permission helpers for:
- canReadModule
- canCreateRecord
- canEditDraft
- canSubmitRecord
- canApproveRecord
- canRejectRecord
- canCancelRecord
- canExport
- canOpenDownstreamAction

These helpers should accept:
- user roles
- permissions
- current record status
- optional scope context

---

## 7. Common State Management Rules
Split state into:
- server state → list/detail data, summaries, linked records
- local UI state → modal open/close, form input, filters, selection
- derived UI state → button visibility, editability, section emphasis

### Recommendation
Use:
- one query layer for server state
- page-level local state for transient UI
- shared permission selectors/utilities for role/status logic

---

## 8. Common Refresh Strategy
After every workflow mutation:
- refetch the current record
- refetch linked summary blocks
- refetch list summary if mutation happened from list page
- preserve current user context/filter state where possible

### Important Rule
Do not rely only on optimistic UI for workflow-critical transitions.

---

## 9. Shared Component Groups
### Shell Components
- AppShell
- SidebarNav
- Topbar
- Breadcrumbs

### Page Components
- PageHeader
- ActionBar
- SummaryCardStrip
- FilterBar
- DataTable
- EmptyState
- ErrorState

### Workflow Components
- RecordStatusHeader
- ApprovalTimeline
- LinkedRecordsPanel
- ConfirmDialog
- RejectReasonModal

### Collaboration Components
- CommentThreadPanel
- AttachmentPanel
- AuditTrailPanel

### Domain Components
- VendorCommercialSummary
- TotalsSummaryCard
- ReceiptSummaryCard
- QCDispositionPanel
- StockImpactPanel
- ReceiptProgressWidget

---

## 10. API Client Grouping Recommendation
Group frontend service files by domain:
- procurementService
- inventoryService
- qualityService
- workflowService
- fileService
- auditService
- dashboardService

### Example
`procurementService.getPRDetail(id)`
`procurementService.approvePO(id, payload)`
`inventoryService.getStockOverview(filters)`

---

## 11. Frontend Build Sequence Recommendation
Best build order for this slice:

1. route map
2. permission helpers
3. shared page shell components
4. PR List
5. PR Detail
6. PO List
7. PO Detail
8. GRN Entry
9. GRN Detail
10. Incoming QC
11. Stock Overview
12. shared linked-record and audit/comment/attachment panels

---

## 12. Completion Criteria
This file is complete when:
- each page has a route
- each page has required components
- each page has required queries/mutations
- local and derived UI state are identified
- refresh behavior is defined
- frontend build order is clear

---

## 13. Next Related Files
Recommended next files:
1. frontend_component_library_plan.md
2. frontend_route_map.md
3. frontend_permission_logic_matrix.md
4. api_page_dependency_matrix.md
