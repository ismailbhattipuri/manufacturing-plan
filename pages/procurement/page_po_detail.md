# Page: PO Detail

## 1. Page Purpose
The PO Detail page is the full record workspace for a single Purchase Order (PO). It allows authorized users to:
- review all purchase order information
- view vendor and commercial terms
- track current workflow status
- perform approval and control actions
- monitor receipt progress and linked GRN activity
- access comments, attachments, audit log, and approval history

This page is the main control center for a PO after creation and before / during goods receipt.

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
- txn_purchase_order.md
- txn_purchase_requisition.md
- page_po_list.md
- page_pr_detail.md

---

## 3. Module
- Procurement

---

## 4. Page Type
- Standard ERP Detail View Page

This page follows the enterprise detail-page pattern:
- breadcrumb + title
- record header with status and totals
- top-right action cluster
- main commercial and line item content
- right-side or tabbed contextual panels for approvals, comments, attachments, and audit
- linked downstream record section for GRN and future invoice references

---

## 5. Primary Roles
### Main Users
- Procurement Executive
- Procurement Manager

### Secondary Users
- Store / Warehouse Team
- Finance Team
- Management (view/export only if permitted)
- Admin (audit/configuration support only if policy allows)

---

## 6. Business Goal of This Page
This page supports the following business outcomes:
- complete review of a PO before approval
- accurate vendor/commercial visibility
- controlled maker-checker workflow
- clear readiness for goods receipt
- auditability of procurement commitment
- traceable PO → GRN continuity

---

## 7. Entry Points
Users may reach this page from:
- PO List row click
- PO List row action “View”
- PR Detail → linked PO section
- PO creation flow after save/create
- dashboard widgets
- approval work queue
- notification link
- global search result
- linked GRN navigation return

---

## 8. Route / Navigation Position
### Suggested Route
- `/procurement/purchase-orders/{id}`

### Related Routes
- `/procurement/purchase-orders`
- `/procurement/purchase-orders/new`
- `/procurement/purchase-requests/{id}`
- `/procurement/grn/new?source_po={id}`

---

## 9. Breadcrumb
Recommended breadcrumb:
- Procurement / Purchase Orders / PO-XXXXXX

Where:
- “Purchase Orders” links back to PO List
- final breadcrumb item is current PO Number

---

## 10. Record Header Structure
The header area should contain:
- PO Number
- current Status badge
- vendor name
- PO Date
- expected delivery date
- source PR reference
- buyer / procurement owner
- branch / delivery warehouse
- grand total
- top-right action cluster

### Header Purpose
The user should understand immediately:
- what order this is
- which vendor it belongs to
- current approval/receipt stage
- what actions are available next

---

## 11. Top-Right Action Cluster
Actions should render based on role + status.

### Possible Actions
- Edit
- Submit
- Approve
- Reject
- Cancel
- Close
- Create GRN / Receive Goods
- View Linked GRN
- Print
- Export
- Add Comment

### Action Visibility Rules
#### Procurement Executive
- Draft → Edit, Submit, Cancel
- Submitted → View only, Comment
- Approved → View, Print/Export, View Linked GRN
- Approved + if permitted process ownership → Create GRN shortcut may be shown as navigation support only
- Rejected → View, Revise/Resubmit if enabled, Cancel
- Closed / Cancelled → View only

#### Procurement Manager
- Draft → View only
- Submitted → Approve, Reject, Comment
- Approved → View, Close (if policy allows)
- Rejected → View only
- Closed / Cancelled → View only

#### Store / Warehouse Team
- Approved → View, Create GRN / Receive Goods, View Linked GRN
- other statuses → view only if allowed

### Important Rule
Commercial and lifecycle actions must always be validated by backend transition rules.

---

## 12. Main Layout Structure
Recommended layout:

### Option A – Main Content + Right Context Panel
#### Left / Main Content
- vendor/commercial summary
- source PR reference block
- line items
- totals / tax summary
- linked records section

#### Right Context Panel
- approval timeline
- comments
- attachments
- audit summary
- receipt progress widget

### Option B – Main Content with Tabs
Tabs:
- Overview
- Line Items
- Approvals
- Comments
- Attachments
- Audit Log
- Linked Records
- Receipt Progress

### Recommendation
Use a hybrid structure:
- summary + line items visible by default
- supporting tabs or right-side contextual panel for traceability and collaboration

---

## 13. Source PR Reference Section
This section should clearly show the upstream PR relationship.

### Show
- Source PR Number
- PR Status
- Requester
- Department
- linked line mapping indicator
- quick link to PR Detail

### Purpose
This preserves upstream traceability and helps users understand why the PO exists.

---

## 14. Vendor and Commercial Summary Section
This is the first major content block after the header.

### Recommended Fields
- Vendor Name
- Vendor Code
- Vendor Contact (optional)
- Currency
- Exchange Rate (if used)
- Payment Terms
- Delivery Terms
- Tax Profile
- Shipping / Incoterm (optional)
- Expected Delivery Date
- Delivery Warehouse / Location
- Buyer / Procurement Owner
- Order Note / Remarks

### Behavior
- editable in Draft by authorized maker
- read-only after submission unless explicit revise flow reopens record

---

## 15. Totals Summary Section
Show the commercial totals clearly.

### Recommended Fields
- Subtotal
- Discount
- Tax
- Other Charges
- Grand Total

### Optional Fields
- Budget reference summary
- committed amount flag
- currency-converted total (if multi-currency)

### Purpose
This is critical for approval review.

---

## 16. Line Items Section
This section shows all ordered items.

### Recommended Grid Columns
- Line No
- Item Code
- Item Name / Description
- UOM
- Ordered Quantity
- Received Quantity
- Remaining Quantity
- Unit Price
- Discount
- Tax
- Line Total
- Expected Delivery Date
- Receiving Warehouse / Location
- QC Required Flag
- Source PR Line Reference
- Line Remark

### Behavior
- editable in Draft by authorized maker
- read-only after submission/approval
- receipt progress values become important after GRN begins

### Extra Support
- line total footer
- quantity and financial totals
- overdue or partial receipt indicators

---

## 17. Status and Workflow Section
The page should clearly communicate current lifecycle state.

### Must Show
- current status
- current approval stage
- submitted by / submitted at
- approved/rejected by / timestamp
- rejection reason if applicable
- close/cancel information if applicable
- receipt progress summary for approved POs

### Optional Enhancements
- workflow timeline visualization
- pending approval age
- overdue delivery indicator
- receipt completion bar

---

## 18. Approval History Section
Show all approval actions taken on the PO.

### Recommended Fields
- sequence
- approver name
- role
- action taken
- action timestamp
- action comment / reason
- delegation / escalation marker if used later

### Importance
This supports procurement governance and auditability.

---

## 19. Receipt Progress Section
This is one of the most important PO-specific sections.

### Show
- total ordered quantity
- total received quantity
- total remaining quantity
- receipt completion percentage
- linked GRN count
- last GRN date
- pending receipt status

### Line-Level View
If possible, show receipt progress per line item.

### Purpose
This connects PO directly to inventory receiving operations.

---

## 20. Linked GRN Section
This section connects the PO to downstream goods receipts.

### Show
- GRN Number
- GRN Date
- GRN Status
- received by
- warehouse
- received quantity summary
- quick link to GRN Detail

### Future Extensions
- QC result visibility
- partial receipt chain
- linked invoice visibility
- procurement-to-receipt timeline

---

## 21. Comments Section
Support structured discussion on the PO.

### Recommended Behavior
- show author, timestamp, comment text
- distinguish general comments from approval/rejection comments
- allow comment entry for authorized users
- keep comment history traceable

---

## 22. Attachments Section
Support commercial and operational supporting documents.

### Typical Attachments
- quotation
- vendor offer
- comparative statement
- contract copy
- specification document
- approval memo

### Behavior
- show file name, uploaded by, uploaded at, type
- upload allowed in Draft and policy-allowed cases
- delete restricted by role and status

---

## 23. Audit Log Section
This section should show major PO activity.

### Recommended Events
- created
- edited
- submitted
- approved
- rejected
- cancelled
- closed
- vendor change
- commercial term change
- line modification
- linked GRN created

### Recommended Fields
- event type
- acted by
- acted at
- short description
- old/new value summary for critical commercial changes where available

---

## 24. Status-Based UI Rules
### Draft
- editable by authorized maker
- submit button visible
- cancel allowed
- commercial and item sections fully editable

### Submitted
- content locked
- manager sees approve/reject controls
- totals and vendor data clearly emphasized for review

### Approved
- content locked
- receipt progress enabled
- linked GRN section emphasized
- Create GRN / Receive Goods action visible to authorized receiving roles

### Rejected
- rejection reason prominently visible
- revise / resubmit flow shown if enabled

### Closed
- read-only
- receipt history visible
- no further business mutation except view/export

### Cancelled
- read-only
- inactive lifecycle state

---

## 25. Role-Based UI Rules
### Procurement Executive
Should see:
- editable Draft PO
- submit action
- source PR reference
- vendor/commercial details
- linked GRN visibility after approval
- attachment/comment support

Should not see:
- approve/reject controls

### Procurement Manager
Should see:
- Submitted PO review state
- approve/reject controls
- totals and commercial terms prominently
- approval history and audit signals

### Store / Warehouse Team
Should see:
- approved PO details necessary for receiving
- receipt progress
- linked GRNs
- Create GRN / Receive Goods action if allowed

Should not see:
- editable commercial terms
- approval controls

### Finance / Viewer
Should see:
- commercial totals
- status
- linked invoice-ready context in future
- export/print if permitted

---

## 26. Confirmation and Guardrail Behavior
### Submit
- confirmation optional
- backend validation mandatory

### Approve
- confirmation dialog
- backend transition check
- maker-checker enforcement
- optional financial threshold check

### Reject
- rejection reason required
- comment mandatory

### Cancel
- confirmation required
- cancellation restrictions depend on receipt progress

### Close
- confirmation required
- only when business rules allow closure

### Create GRN / Receive Goods
- only available when status = Approved
- should open GRN flow with PO reference preloaded

---

## 27. Empty / Missing Data States
### Missing Optional Data
Show graceful placeholders like:
- “No attachment uploaded”
- “No comments yet”
- “No GRN linked yet”

### Missing Record
If record not found:
- show record-not-found state
- provide back-to-list action

---

## 28. Loading State
Use:
- page header skeleton
- summary skeleton
- line item table skeleton
- contextual panel/tabs skeleton
- linked GRN section loading placeholder

---

## 29. Error State
Possible errors:
- record fetch failed
- comments/attachments fetch failed
- approval action failed
- GRN link fetch failed
- receipt progress fetch failed

Display:
- clear message
- retry action
- partial section recovery where possible

---

## 30. No-Access State
If the user lacks permission:
- show restricted-access message
- hide sensitive record content
- prevent unauthorized action visibility

---

## 31. Data Requirements
This page needs:
- PO header detail
- PO line items
- source PR summary
- role-based action permissions
- approval history
- comments
- attachments
- audit history
- linked GRN summary
- receipt progress
- GRN creation eligibility state

---

## 32. API Dependencies
### Core APIs
- Get PO detail
- Update PO draft
- Submit PO
- Approve PO
- Reject PO
- Cancel PO
- Close PO
- Get PO comments
- Add PO comment
- Get PO attachments
- Upload attachment
- Get PO audit log
- Get linked GRN records
- Get receipt progress
- Start GRN creation from PO

### Example Endpoint Needs
- `GET /api/v1/purchase-orders/{id}`
- `PATCH /api/v1/purchase-orders/{id}`
- `POST /api/v1/purchase-orders/{id}/submit`
- `POST /api/v1/purchase-orders/{id}/approve`
- `POST /api/v1/purchase-orders/{id}/reject`
- `POST /api/v1/purchase-orders/{id}/cancel`
- `POST /api/v1/purchase-orders/{id}/close`
- `GET /api/v1/purchase-orders/{id}/comments`
- `POST /api/v1/purchase-orders/{id}/comments`
- `GET /api/v1/purchase-orders/{id}/attachments`
- `GET /api/v1/purchase-orders/{id}/audit-log`
- `GET /api/v1/purchase-orders/{id}/linked-grns`
- `GET /api/v1/purchase-orders/{id}/receipt-progress`

---

## 33. Validation / Action Rules on This Page
### Edit
- only allowed in Draft by permitted maker role

### Submit
- record must be Draft
- mandatory commercial and line data must be complete
- totals must calculate correctly

### Approve
- record must be Submitted
- approver must have permission
- creator cannot approve own PO unless policy allows

### Reject
- record must be Submitted
- rejection reason must be provided

### Close
- allowed only by authorized role
- closure preconditions must pass

### Create GRN / Receive Goods
- record must be Approved
- receiving role must have access
- remaining quantity must exist
- warehouse/delivery context must be valid

---

## 34. Suggested UI Components
Recommended reusable components:
- RecordStatusHeader
- SourceReferenceCard
- VendorCommercialSummary
- TotalsSummaryCard
- LineItemGrid
- ApprovalTimeline
- ReceiptProgressWidget
- LinkedRecordsPanel
- CommentThreadPanel
- AttachmentPanel
- AuditTrailPanel
- ActionBar
- ConfirmDialog
- RejectReasonModal

---

## 35. Export / Print Behavior
### Print
Useful for vendor-facing document review and operational reference.

### Export
Allowed only for authorized roles.

### Rule
Print/export should reflect official approved record state and respect access policy.

---

## 36. Notification / Deep Link Behavior
This page should support direct opening from:
- PO approval task notification
- PO rejection notification
- PO approved notification
- GRN receipt update notification
- linked GRN navigation
- PR → PO conversion completion

Deep links should preserve workflow context where useful.

---

## 37. Wireframe Notes
For wireframe execution:
- vendor + total + status should be above the fold
- line items should dominate the main workspace
- source PR and linked GRN should both be visible
- receipt progress should be easy to scan
- approval/comments/attachments should remain accessible without clutter
- approved state should visually signal downstream GRN readiness

---

## 38. Frontend Build Notes
### State Handling Needed
- detail fetch state
- draft edit state
- section loading states
- role-aware action rendering
- mutation refresh state
- comment/attachment refresh
- receipt progress refresh
- linked GRN refresh after new receipt

### Edit Mode Suggestion
- editable only in Draft
- auto-lock after submit
- preserve unsaved-change warning

---

## 39. Backend Build Notes
Backend should ensure:
- detail payload includes commercial, workflow, and receipt context
- approval history is complete and ordered
- linked GRN summary is efficient
- receipt progress is accurate and aggregated correctly
- comments/attachments are scoped properly
- submit/approve/reject/cancel/close enforce lifecycle rules
- maker-checker rule is enforced centrally
- audit log captures all high-risk and commercial changes

---

## 40. Dependencies
This page depends on:
- PO transaction design
- PR transaction linkage
- Procurement Executive and Procurement Manager role definitions
- Store / Warehouse receiving flow
- approval workflow support
- comments/attachments/audit support
- GRN transaction/page flow
- Step 9 frontend page standards

---

## 41. Completion Criteria
PO Detail page design is complete when:
- header and action cluster are fixed
- source PR, vendor/commercial, totals, and line item sections are clear
- role-based and status-based actions are defined
- approval/comments/attachments/audit/receipt progress/linked GRNs are defined
- API dependencies are listed
- PO → GRN path is explicit

---

## 42. Next Related Files
Recommended next files:
1. txn_goods_receipt_note.md
2. page_grn_entry.md
3. page_grn_detail.md
4. connection_document_to_document.md
