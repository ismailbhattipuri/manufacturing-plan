# Page: PR Detail

## 1. Page Purpose
The PR Detail page is the full record workspace for a single Purchase Requisition (PR). It allows authorized users to:
- view full PR details
- review header and line item information
- track current status and approval stage
- perform role-based actions
- review comments, attachments, audit history, and approval history
- access linked downstream documents such as Purchase Orders (PO)

This page is the main control center for reviewing and acting on one PR.

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
- page_pr_list.md

---

## 3. Module
- Procurement

---

## 4. Page Type
- Standard ERP Detail View Page

This page follows the enterprise detail-page pattern:
- breadcrumb + title
- record header with status
- top-right action cluster
- main content area
- side panel or supporting tabs for approval, comments, attachments, and audit
- linked records section

---

## 5. Primary Roles
### Main Users
- Procurement Executive
- Procurement Manager

### Secondary Users
- Management (view/export only if permitted)
- Admin (audit or configuration support only if policy allows)

---

## 6. Business Goal of This Page
This page supports the following business outcomes:
- full visibility of a PR record
- controlled workflow action based on role and status
- maker-checker review process
- auditability of procurement request decisions
- downstream PR → PO continuity

---

## 7. Entry Points
Users may reach this page from:
- PR List row click
- PR List row action “View”
- dashboard widget
- approval work queue
- notification link
- global search result
- linked record navigation from PO
- breadcrumb return from related pages

---

## 8. Route / Navigation Position
### Suggested Route
- `/procurement/purchase-requests/{id}`

### Related Routes
- `/procurement/purchase-requests`
- `/procurement/purchase-requests/new`
- `/procurement/purchase-orders/new?source_pr={id}`

---

## 9. Breadcrumb
Recommended breadcrumb:
- Procurement / Purchase Requisitions / PR-XXXXXX

Where:
- “Purchase Requisitions” links back to PR List
- final breadcrumb item is current record number

---

## 10. Record Header Structure
The header area should contain:
- PR Number
- current Status badge
- priority badge
- requested by
- PR Date
- department / branch
- required by date
- total estimated amount
- top-right action cluster

### Header Purpose
The user should understand the document identity, current stage, and available next actions immediately.

---

## 11. Top-Right Action Cluster
Actions should render based on role + status.

### Possible Actions
- Edit
- Submit
- Approve
- Reject
- Cancel
- Convert to PO
- Print
- Export
- Add Comment
- View Linked PO

### Action Visibility Rules
#### Procurement Executive
- Draft → Edit, Submit, Cancel
- Submitted → View only, Comment
- Approved → Convert to PO, View Linked PO, Print/Export if allowed
- Rejected → View, Revise/Resubmit if enabled, Cancel
- Cancelled → View only

#### Procurement Manager
- Draft → View only
- Submitted → Approve, Reject, Comment
- Approved → View only
- Rejected → View only
- Cancelled → View only

### Important Rule
High-risk actions must use confirmation and backend validation.

---

## 12. Main Layout Structure
Recommended layout:

### Option A – Main Content + Right Context Panel
#### Left / Main Content
- summary block
- line items
- business justification
- related business references

#### Right Context Panel
- approval timeline
- comments
- attachments
- audit summary

### Option B – Main Content with Tabs
Tabs:
- Overview
- Line Items
- Approvals
- Comments
- Attachments
- Audit Log
- Linked Records

### Recommendation
Use Option A for operational speed, with tabs inside the main area only if data density becomes too high.

---

## 13. Main Summary Section
The first content block should show key PR metadata.

### Recommended Fields
- PR Number
- PR Date
- Request Type
- Priority
- Requested By
- Department
- Branch
- Plant / Warehouse Context if used
- Required By Date
- Purpose / Justification
- Cost Center (optional)
- Project / Job Reference (optional)
- Budget Reference (optional)

### Behavior
- fields are editable only in Draft by authorized maker
- read-only after submission unless revise flow explicitly reopens editability

---

## 14. Line Items Section
This section shows all requested items.

### Recommended Grid Columns
- Line No
- Item Code
- Item Name / Description
- UOM
- Requested Quantity
- Estimated Unit Cost
- Estimated Total Cost
- Required Date
- Preferred Vendor (optional)
- Warehouse / Delivery Context
- Line Remark

### Behavior
- editable in Draft for maker
- read-only in Submitted / Approved / Rejected / Cancelled unless revision flow applies

### Extra Support
- line total footer
- overall estimated amount summary
- optional item-level attachment indicator in future

---

## 15. Status and Workflow Section
The page should clearly communicate current workflow state.

### Must Show
- current status
- current approval stage
- who submitted
- submitted at
- approved/rejected by
- approved/rejected at
- rejection reason where applicable

### Optional Enhancements
- workflow timeline visualization
- time in current status
- SLA / aging indicator for pending approval

---

## 16. Approval History Section
This section should show all approval actions taken on the PR.

### Recommended Fields
- sequence
- approver name
- role
- action taken
- action timestamp
- action comment / reason
- delegation / escalation marker if used later

### Importance
This is critical for auditability and operational trust.

---

## 17. Comments Section
Support structured discussion on the record.

### Recommended Behavior
- show author, timestamp, comment text
- distinguish normal comments vs approval/rejection comments
- allow adding new comment for authorized users
- comments remain immutable or audit-tracked if edited

---

## 18. Attachments Section
Support supporting documents for the PR.

### Typical Attachments
- request memo
- technical spec
- approval document
- quotation (if attached early)
- departmental supporting file

### Behavior
- show file name, uploaded by, uploaded at, file type
- allow upload in Draft and policy-allowed stages
- restrict delete based on status and role

---

## 19. Audit Log Section
This section should show major record activity.

### Recommended Events
- created
- edited
- submitted
- approved
- rejected
- cancelled
- line changes
- linked PO created

### Recommended Fields
- event type
- acted by
- acted at
- short description
- optional old/new value summary for critical changes

---

## 20. Linked Records Section
This section connects PR to downstream business documents.

### Initial Downstream Link
- linked PO(s)

### Show
- PO Number
- PO Status
- vendor
- created date
- quick link to PO detail

### Future Extensions
- partial conversion summary
- fully converted indicator
- linked GRN visibility through PO
- procurement chain overview widget

---

## 21. Status-Based UI Rules
### Draft
- editable by authorized maker
- submit button visible
- cancel allowed
- full form-style interaction

### Submitted
- content locked
- manager sees approve/reject actions
- comments and approval context visible

### Approved
- content locked
- Convert to PO action visible for permitted maker role
- linked PO section emphasized

### Rejected
- rejection reason prominently visible
- revise / resubmit flow shown if enabled
- direct edit only if revision policy reopens it

### Cancelled
- read-only state
- action set removed except print/export if allowed

---

## 22. Role-Based UI Rules
### Procurement Executive
Should see:
- editable Draft PR
- submit action
- approved PR conversion action
- linked PO visibility
- attachment/comment support

Should not see:
- approve/reject controls

### Procurement Manager
Should see:
- Submitted PR review state
- approve/reject controls
- approval history
- comments
- audit signals
- mostly read-only form content

### Management / Viewer
Should see:
- full view if permitted
- export/print if authorized
- no business actions

---

## 23. Confirmation and Guardrail Behavior
### Submit
- confirmation optional
- backend validation mandatory

### Approve
- confirmation dialog
- backend transition check
- maker-checker enforcement

### Reject
- rejection reason required
- comment box mandatory
- backend transition check

### Cancel
- confirmation required
- reason optional or required depending on policy

### Convert to PO
- only available when status = Approved
- opens PO creation flow with PR reference

---

## 24. Empty / Missing Data States
### Missing Optional Data
Show graceful placeholders like:
- “No attachment uploaded”
- “No comments yet”
- “No linked PO yet”

### Missing Record
If record not found:
- show record-not-found state
- provide back-to-list action

---

## 25. Loading State
Use:
- page header skeleton
- section skeletons for summary and line items
- right panel/tabs loading placeholders

---

## 26. Error State
Possible errors:
- record fetch failed
- attachments/comments fetch failed
- approval action failed
- linked PO fetch failed

Display:
- clear message
- retry action
- partial section recovery where possible

---

## 27. No-Access State
If the user lacks permission:
- show restricted-access message
- hide record content
- prevent action visibility

---

## 28. Data Requirements
This page needs:
- PR header detail
- PR line items
- role-based action permissions
- approval history
- comments
- attachments
- audit history
- linked PO summary
- conversion eligibility state

---

## 29. API Dependencies
### Core APIs
- Get PR detail
- Update PR draft
- Submit PR
- Approve PR
- Reject PR
- Cancel PR
- Get PR comments
- Add PR comment
- Get PR attachments
- Upload attachment
- Get PR audit log
- Get linked PO records
- Start PO creation from PR

### Example Endpoint Needs
- `GET /api/v1/purchase-requests/{id}`
- `PATCH /api/v1/purchase-requests/{id}`
- `POST /api/v1/purchase-requests/{id}/submit`
- `POST /api/v1/purchase-requests/{id}/approve`
- `POST /api/v1/purchase-requests/{id}/reject`
- `POST /api/v1/purchase-requests/{id}/cancel`
- `GET /api/v1/purchase-requests/{id}/comments`
- `POST /api/v1/purchase-requests/{id}/comments`
- `GET /api/v1/purchase-requests/{id}/attachments`
- `GET /api/v1/purchase-requests/{id}/audit-log`
- `GET /api/v1/purchase-requests/{id}/linked-purchase-orders`

---

## 30. Validation / Action Rules on This Page
### Edit
- only allowed in Draft by permitted maker role

### Submit
- record must be Draft
- mandatory data must be complete
- line items must be valid

### Approve
- record must be Submitted
- approver must have permission
- creator cannot approve own PR unless policy allows

### Reject
- record must be Submitted
- rejection reason must be provided

### Convert to PO
- record must be Approved
- downstream conversion rule must pass
- user must have create/conversion permission for PO

---

## 31. Suggested UI Components
Recommended reusable components:
- RecordStatusHeader
- SummarySection
- LineItemGrid
- ApprovalTimeline
- CommentThreadPanel
- AttachmentPanel
- AuditTrailPanel
- LinkedRecordsPanel
- ActionBar
- ConfirmDialog
- RejectReasonModal

---

## 32. Export / Print Behavior
### Print
Useful for formal review and offline discussion.

### Export
Allowed only for authorized roles.

### Rule
Export/print should reflect record state and not expose unauthorized data.

---

## 33. Notification / Deep Link Behavior
This page should support direct opening from:
- approval task notification
- rejection notification
- “PR approved” notification
- “PR ready for PO conversion” reminder
- linked PO navigation

Deep links should preserve context where useful.

---

## 34. Wireframe Notes
For wireframe execution:
- keep status and actions above the fold
- line items should occupy the main reading area
- approval/comments/attachments should be easy to scan
- right-side contextual panel is preferred for decision support
- linked PO area should be visible after approval
- rejection state should visually highlight rejection reason

---

## 35. Frontend Build Notes
### State Handling Needed
- detail fetch state
- draft edit state
- section loading states
- role-aware action rendering
- action mutation refresh state
- comment/attachment refresh
- linked record refresh after PO conversion

### Edit Mode Suggestion
- toggle edit mode only in Draft
- auto-lock after submit
- preserve unsaved-change warning

---

## 36. Backend Build Notes
Backend should ensure:
- detail payload includes enough context for action rendering
- approval history is complete and ordered
- linked PO summary is efficient
- comments/attachments are scoped properly
- submit/approve/reject/cancel enforce status transition rules
- maker-checker rule is enforced centrally
- audit log captures all high-risk actions

---

## 37. Dependencies
This page depends on:
- PR transaction design
- Procurement Executive and Procurement Manager role definitions
- approval workflow support
- comments/attachments/audit support
- PO creation flow
- Step 9 frontend page standards

---

## 38. Completion Criteria
PR Detail page design is complete when:
- header and action cluster are fixed
- summary and line item sections are clear
- role-based and status-based actions are defined
- approval/comments/attachments/audit/linked records are defined
- API dependencies are listed
- PR → PO conversion path is explicit

---

## 39. Next Related Files
Recommended next files:
1. page_po_list.md
2. page_po_detail.md
3. txn_goods_receipt_note.md
4. connection_document_to_document.md
