# Transaction: Purchase Requisition (PR)

## 1. Business Purpose
Purchase Requisition (PR) is the internal request document used to initiate procurement of goods, materials, or services needed by the business. It starts the procurement lifecycle and acts as the formal source record for review, approval, and conversion into Purchase Order (PO).

This transaction supports the procurement flow defined for ERP V2:
PR → Approved → PO → Approved → GRN → Completed

---

## 2. Source Alignment
This transaction file is derived from and must remain aligned with:
- Step 2 – Procurement Module
- Step 3 – User Role Design
- Step 4 – Procurement Flow
- Step 5 – Screen Map
- Step 8 – Backend API
- Step 9 – Frontend Development

---

## 3. Trigger / When PR Is Created
A PR is created when:
- stock or material is required for procurement
- a department requests non-stock or service purchase
- production or operations identify shortage or planned requirement
- management-approved procurement need must enter workflow formally

Typical triggers:
- manual request by Procurement Executive or authorized requester
- shortage-driven procurement planning
- department purchase request
- production requirement escalation

---

## 4. Roles Involved
### Primary Roles
- Procurement Executive
- Procurement Manager

### Secondary / Related Roles
- Management (optional, depending on approval policy)
- Finance (optional, for budget control)
- Store / Warehouse Team (view or downstream reference only)

---

## 5. Role Responsibility Summary
### Procurement Executive
- create PR
- edit PR while Draft
- submit PR
- view PR history
- convert approved PR to PO if business rule allows

### Procurement Manager
- review submitted PR
- approve PR
- reject PR
- return PR for correction if return flow is implemented
- view audit and justification details

### Admin
- system configuration only
- no automatic business approval authority unless explicitly assigned by policy

---

## 6. Transaction Type
- Category: Transaction Header + Line Item
- Module: Procurement
- Upstream Source: None in standard flow
- Downstream Target: Purchase Order (PO)

---

## 7. Page Coverage
This transaction is represented across these pages:
- PR List
- Create PR
- PR Detail View

Related future pages:
- PR Approval Queue
- PR Dashboard Widgets
- PR Report / Export View

---

## 8. Core Business Outcome
The PR ensures:
- procurement demand is documented
- requester intent is auditable
- approval is enforced before ordering
- downstream PO creation is controlled
- procurement workflow is standardized

---

## 9. PR Header Fields
Recommended header fields:

### Identification
- PR ID (internal system ID)
- PR Number / Request No
- PR Date
- Request Type
- Priority

### Request Ownership
- Requested By User ID
- Requesting Department
- Branch ID
- Plant / Factory ID (future-ready)
- Warehouse / Location Context (if applicable)

### Business Context
- Purpose / Justification
- Required By Date
- Budget Reference (optional)
- Cost Center (optional)
- Project / Job Reference (optional)

### Workflow / Status
- Current Status
- Submitted At
- Approved At
- Rejected At
- Approved By
- Rejected By
- Rejection Reason

### Audit / System
- Created At
- Created By
- Updated At
- Updated By
- Version No
- Is Active / Archived Flag if needed

---

## 10. PR Line Item Fields
Each PR may contain one or more requested items.

### Item Identification
- Line ID
- Item / Product ID
- Item Code
- Item Name / Description
- UOM

### Quantity / Need
- Requested Quantity
- Estimated Unit Cost (optional)
- Estimated Total Cost
- Required Date

### Supply Context
- Preferred Vendor (optional)
- Warehouse / Delivery Location
- BOM / Production Reference (optional)
- Stock Shortage Reference (optional)

### Control / Notes
- Line Remark
- QC Required Flag (optional)
- Attachment Required Flag (optional if rule applies)

---

## 11. Status Model
Base PR status flow:

- Draft
- Submitted
- Approved
- Rejected
- Cancelled

Optional future statuses:
- Returned for Correction
- Partially Converted
- Fully Converted
- Closed

### Status Meaning
#### Draft
PR is created but not yet submitted. Fully editable by authorized maker.

#### Submitted
PR is sent for approval. Editing is restricted.

#### Approved
PR is approved and eligible for PO conversion.

#### Rejected
PR is rejected by approver. Rejection reason required.

#### Cancelled
PR is cancelled before downstream use, based on policy.

---

## 12. Status Transition Rules
Allowed transitions:

- Draft → Submitted
- Draft → Cancelled
- Submitted → Approved
- Submitted → Rejected
- Submitted → Cancelled (only if policy allows)
- Rejected → Draft (only if revision flow is supported)
- Approved → Partially Converted (future)
- Approved → Fully Converted (future)
- Approved → Closed (only after downstream completion if business uses close state)

### Transition Guardrails
- cannot approve Draft PR
- cannot submit PR with invalid or incomplete data
- cannot edit Approved PR directly
- cannot convert Rejected PR to PO
- cannot delete submitted/approved PR in normal business operation

---

## 13. Actions by Status
### Draft
Allowed actions:
- Save Draft
- Edit
- Add / Remove Line Items
- Attach Supporting Documents
- Submit
- Cancel

### Submitted
Allowed actions:
- View
- Approve (authorized approver only)
- Reject (authorized approver only)
- Comment
- Download / Print if allowed

### Approved
Allowed actions:
- View
- Convert to PO
- Export / Print
- View Linked PO(s)
- Comment

### Rejected
Allowed actions:
- View
- View Rejection Reason
- Revise / Resubmit (if enabled)
- Cancel

### Cancelled
Allowed actions:
- View only

---

## 14. Approval Logic
### Standard Model
- Maker: Procurement Executive
- Checker: Procurement Manager

### Rules
- creator should not approve own PR unless business rule explicitly permits
- approval should only be available when status = Submitted
- rejection should require structured reason/comment
- future extension may include amount-based approval routing
- future extension may include multi-step approval

### Approval Data to Capture
- approver user
- approval action
- action timestamp
- action comment / reason
- sequence number if multi-step
- delegation reference if used later

---

## 15. Validation Rules
### Header-Level Validation
- PR date required
- requested by required
- department required if policy applies
- purpose / justification required
- required-by date should not be invalid past date where business forbids it
- branch / location required if organization uses scoped operations

### Line-Level Validation
- at least one line item required
- item required
- quantity must be greater than 0
- UOM required
- required date required if line-level planning is enabled
- estimated amount cannot be negative
- duplicate line handling rule should be defined
- warehouse / delivery context required where applicable

### Submission Validation
Before submit:
- mandatory header fields completed
- at least one valid line exists
- user has permission to submit
- PR still in Draft
- no blocked validation errors remain

### Approval Validation
Before approve:
- status must be Submitted
- approver must have approval permission
- maker-checker separation must pass
- PR must not already be cancelled or rejected

---

## 16. Derived / Calculated Fields
Optional or recommended calculated values:
- Total Estimated Amount = sum of line estimated totals
- Total Line Count
- Pending Conversion Quantity / Value (future)
- Approval Age / Time in Status
- Days Until Required Date

---

## 17. Linked Documents and Relations
### Upstream
- none in standard initial flow

### Downstream
- Purchase Order(s)

### Related References
- attachments
- comments
- approval history
- audit log
- requester profile
- department / branch / plant / warehouse reference
- future budget or project references

---

## 18. Inventory / Finance / System Impact
### Inventory Impact
- no stock movement at PR stage
- may act as demand signal for procurement planning

### Finance Impact
- no accounting posting at PR stage
- optional budget validation or estimated commitment only

### System Impact
On create:
- PR number generated
- draft record stored
- audit log written

On submit:
- status changes to Submitted
- approval workflow created or advanced
- approver notified

On approve:
- status changes to Approved
- PR becomes eligible for PO creation
- audit and approval history stored

On reject:
- status changes to Rejected
- rejection reason stored
- requester notified

---

## 19. Exception Scenarios
- PR submitted with missing required data → blocked
- PR rejected by manager → reason mandatory
- PR needs correction after rejection → revise flow
- PR approved but no PO created for long time → dashboard alert / aging report
- partial PO conversion from one PR → requires downstream quantity/value tracking
- duplicate request suspected → optional system warning

---

## 20. Audit Requirements
Track at minimum:
- PR creation
- draft updates
- submit action
- approve action
- reject action
- cancel action
- line item additions/removals where required
- field change history for critical fields
- linked PO creation reference

Audit should capture:
- acted by
- acted at
- previous value / new value where applicable
- source action
- status transition

---

## 21. Attachment and Comment Rules
### Attachments
Possible supporting files:
- quotation
- specification sheet
- departmental request note
- approval memo
- technical requirement document

### Comments
Support:
- general comment
- approval note
- rejection comment
- internal discussion if policy allows

---

## 22. Frontend Notes
### PR List UI Should Show
- PR Number
- Date
- Requester
- Department
- Priority
- Status
- Total Estimated Amount
- Required By Date
- Linked PO indicator

### Create PR Page Should Support
- header + line item form layout
- Save Draft
- Submit
- attachment block
- dynamic line item grid
- validation feedback
- sticky action bar for long form

### PR Detail Page Should Show
- status header
- summary block
- line items
- approval history
- comments
- attachments
- audit panel
- linked PO section
- action buttons based on role + status

### Role-Based UI Behavior
Procurement Executive:
- can create/edit/submit in Draft
- cannot approve
- cannot edit after submit

Procurement Manager:
- can approve/reject when Submitted
- sees approval controls prominently
- mostly read-only content

---

## 23. Backend Notes
### Core Endpoint Needs
- list PRs
- create PR
- get PR detail
- update PR draft
- submit PR
- approve PR
- reject PR
- cancel PR
- get PR history
- get PR comments
- get PR attachments
- get linked POs

### Backend Enforcement
- status transition checks
- maker-checker enforcement
- line validation
- audit logging
- approval history persistence
- role-based authorization
- future scope filtering by branch/department/location

---

## 24. Database Notes
Recommended base tables:
- purchase_requests
- purchase_request_items
- approvals or purchase_request_approvals
- comments
- attachments
- audit_logs

Recommended key relations:
- purchase_requests.requested_by_user_id → users.id
- purchase_requests.department_id → departments.id
- purchase_requests.branch_id → branches.id
- purchase_request_items.purchase_request_id → purchase_requests.id
- purchase_request_items.product_id → products.id

---

## 25. Reporting / Dashboard Notes
Useful PR metrics:
- PR count by status
- pending approvals
- average approval time
- PR aging
- PR value by department
- approved PR awaiting PO conversion
- rejection count / reasons

---

## 26. Dependency Notes
This transaction depends on:
- Master Data: items, vendors, UOM, warehouse, departments
- User / Role design
- Approval workflow
- Comments / attachments support
- Audit framework

---

## 27. Completion Criteria for PR Design
PR transaction design is considered complete when:
- header and line item structure are fixed
- statuses and transitions are defined
- role actions are defined
- approval logic is defined
- validations are documented
- frontend and backend needs are explicit
- downstream PO linkage is clear

---

## 28. Next Related Files
After this file, the recommended next files are:
1. txn_purchase_order.md
2. page_pr_list.md
3. page_pr_detail.md
4. role_store_user.md or GRN-related operational role file
