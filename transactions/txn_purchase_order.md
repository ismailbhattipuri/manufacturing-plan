# Transaction: Purchase Order (PO)

## 1. Business Purpose
Purchase Order (PO) is the formal procurement document issued to a vendor after an approved Purchase Requisition (PR) has been reviewed and converted into an order. It represents the organization's authorized intent to procure goods or services under agreed quantities, terms, pricing, and delivery conditions.

This transaction continues the procurement lifecycle:
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
- txn_purchase_requisition.md

---

## 3. Trigger / When PO Is Created
A PO is created when:
- an approved PR needs to be converted into a vendor-facing order
- procurement decides vendor, commercial terms, and order quantities
- urgent direct procurement is permitted by policy
- approved business demand must move into purchase execution

Typical triggers:
- Convert Approved PR → PO
- Direct PO creation (only if business policy allows)
- Consolidation of one or more approved PRs into one PO (future-ready)

---

## 4. Roles Involved
### Primary Roles
- Procurement Executive
- Procurement Manager

### Secondary / Related Roles
- Finance Officer (optional approval or budget control)
- Store User / Warehouse Team
- QA Team (downstream reference only)
- Management (optional escalation approval)

---

## 5. Role Responsibility Summary
### Procurement Executive
- create PO draft
- convert approved PR to PO
- edit PO while Draft
- submit PO for approval
- attach supporting commercial documents
- view vendor/order history

### Procurement Manager
- review submitted PO
- approve PO
- reject PO
- return PO for correction if enabled
- monitor vendor/order compliance

### Finance Officer (if policy requires)
- review budget or high-value order
- approve PO above threshold
- view financial commitment data

### Store / Warehouse Team
- view approved PO for expected receipt reference
- use PO during GRN process

---

## 6. Transaction Type
- Category: Transaction Header + Line Item
- Module: Procurement
- Upstream Source: Approved Purchase Requisition (PR)
- Downstream Target: Goods Receipt Note (GRN)

---

## 7. Page Coverage
This transaction is represented across these pages:
- PO List
- Create PO
- PO Detail View

Related future pages:
- PO Approval Queue
- PO Aging Dashboard
- Vendor Performance / PO Analysis View
- PO Print / Export View

---

## 8. Core Business Outcome
The PO ensures:
- approved procurement intent becomes a formal vendor order
- vendor, price, quantity, and terms are documented
- approval control exists before goods receipt
- downstream receipt and inventory updates are traceable
- procurement commitments are auditable

---

## 9. PO Header Fields
Recommended header fields:

### Identification
- PO ID (internal system ID)
- PO Number / Order No
- PO Date
- Order Type
- Priority

### Source References
- Source PR ID
- Source PR Number
- Multiple PR References (future if consolidation is supported)

### Vendor / Commercial
- Vendor ID
- Vendor Code
- Vendor Name Snapshot (optional)
- Currency
- Exchange Rate (if multi-currency)
- Payment Terms
- Delivery Terms
- Shipping / Incoterm (optional)
- Tax Profile
- Discount Type / Discount Amount (optional)

### Ownership / Organization
- Buyer / Procurement Owner
- Department
- Branch ID
- Plant / Factory ID (future-ready)
- Delivery Warehouse / Location

### Business Context
- Order Justification / Notes
- Expected Delivery Date
- Quotation Reference (optional)
- Contract Reference (optional)
- Budget Reference (optional)
- Cost Center (optional)
- Project / Job Reference (optional)

### Commercial Totals
- Subtotal
- Discount Amount
- Tax Amount
- Other Charges
- Grand Total

### Workflow / Status
- Current Status
- Submitted At
- Approved At
- Rejected At
- Approved By
- Rejected By
- Rejection Reason
- Closed At
- Cancelled At

### Audit / System
- Created At
- Created By
- Updated At
- Updated By
- Version No
- Is Active / Archived Flag if needed

---

## 10. PO Line Item Fields
Each PO may contain one or more ordered items.

### Item Identification
- Line ID
- Product / Item ID
- Item Code
- Item Name / Description
- Vendor Item Reference (optional)
- UOM

### Ordered Quantity / Pricing
- Ordered Quantity
- Unit Price
- Discount %
- Discount Amount
- Tax %
- Tax Amount
- Line Total
- Currency

### Delivery / Receipt Context
- Expected Delivery Date
- Receiving Warehouse
- Receiving Bin / Location (optional)
- QC Required Flag
- Batch / Serial Required Flag (if applicable)

### Source Linkage
- Source PR Line ID
- Requested Quantity
- Converted Quantity
- Remaining Quantity (future tracking if partial conversion allowed)

### Notes / Controls
- Line Remark
- Technical Specification Reference
- Attachment / Document Indicator if required

---

## 11. Status Model
Base PO status flow:

- Draft
- Submitted
- Approved
- Rejected
- Closed
- Cancelled

Optional future statuses:
- Partially Received
- Fully Received
- Partially Invoiced
- Fully Invoiced
- Returned for Correction

### Status Meaning
#### Draft
PO is prepared but not submitted for approval.

#### Submitted
PO is awaiting approval.

#### Approved
PO is authorized and can be used for goods receipt.

#### Rejected
PO was rejected and requires correction or replacement.

#### Closed
PO is completed or intentionally closed after fulfillment.

#### Cancelled
PO is cancelled before or during downstream use according to policy.

---

## 12. Status Transition Rules
Allowed transitions:

- Draft → Submitted
- Draft → Cancelled
- Submitted → Approved
- Submitted → Rejected
- Submitted → Cancelled (if policy allows)
- Rejected → Draft (if revise flow is enabled)
- Approved → Partially Received (future)
- Approved → Fully Received (future)
- Approved → Closed
- Approved → Cancelled (restricted, special authority only)
- Partially Received → Fully Received (future)
- Partially Received → Closed (future policy case)

### Transition Guardrails
- cannot approve Draft PO
- cannot receive goods against non-approved PO unless exception policy exists
- cannot directly edit Approved PO in normal flow
- cannot close PO before approval
- cannot cancel fully received PO
- cannot exceed ordered quantity during GRN without exception rule

---

## 13. Actions by Status
### Draft
Allowed actions:
- Save Draft
- Edit
- Add / Remove Line Items
- Attach Vendor Documents
- Submit
- Cancel
- Recalculate Totals

### Submitted
Allowed actions:
- View
- Approve (authorized approver only)
- Reject (authorized approver only)
- Comment
- Print / Download if permitted

### Approved
Allowed actions:
- View
- Print / Export
- Use for GRN
- View Linked GRN(s)
- Close (authorized role only)
- Cancel (special policy only)

### Rejected
Allowed actions:
- View
- View Rejection Reason
- Revise / Resubmit (if enabled)
- Cancel

### Closed
Allowed actions:
- View
- Print / Export
- View Receipt History

### Cancelled
Allowed actions:
- View only

---

## 14. Approval Logic
### Standard Model
- Maker: Procurement Executive
- Checker: Procurement Manager

### Extended Model (optional)
- Finance approval required if PO value exceeds threshold
- management escalation required for strategic/high-value procurement

### Rules
- creator should not approve own PO unless business rule explicitly allows
- approval only possible when status = Submitted
- rejection requires reason
- edit after submission should happen through return/revise flow, not silent modification
- downstream GRN should normally require Approved status

### Approval Data to Capture
- approver user
- approval action
- action timestamp
- action comment / reason
- approval sequence
- delegated / escalated reference if applicable

---

## 15. Validation Rules
### Header-Level Validation
- PO date required
- vendor required
- source PR required if policy mandates PR-based procurement
- currency required
- payment terms required if business policy needs it
- expected delivery date required
- receiving warehouse required
- department / branch required if scoped

### Line-Level Validation
- at least one line item required
- item required
- ordered quantity must be greater than 0
- unit price must be valid and non-negative
- tax and discount values must be valid
- receiving context required where applicable
- source PR line linkage required for PR-based PO
- duplicate item handling policy should be defined

### Submission Validation
Before submit:
- mandatory commercial fields completed
- vendor selected
- all lines valid
- totals calculated correctly
- user has submit permission
- PO is still in Draft

### Approval Validation
Before approve:
- status must be Submitted
- approver must have approval permission
- maker-checker separation must pass
- vendor must be active
- downstream conflict checks pass if required

### GRN Readiness Validation
Before goods receipt:
- PO must be Approved
- ordered quantity and remaining quantity must support receipt
- warehouse / receiving context must be valid

---

## 16. Derived / Calculated Fields
Recommended calculated values:
- Line Total = qty × unit price - discount + tax
- Subtotal = sum of line subtotals
- Grand Total = subtotal - discount + tax + other charges
- Pending Receipt Quantity
- Received Quantity
- Remaining Quantity
- PO Aging
- Days to Delivery / Overdue Delivery

---

## 17. Linked Documents and Relations
### Upstream
- Purchase Requisition (PR)

### Downstream
- Goods Receipt Note(s) (GRN)
- Invoice(s) (if finance linkage is used later)

### Related References
- vendor master
- quotation / comparison
- attachments
- comments
- approval history
- audit log
- warehouse / branch / department references

---

## 18. Inventory / Finance / System Impact
### Inventory Impact
- no stock increase on PO approval alone
- PO becomes source reference for expected receipts

### Finance Impact
- no accounting posting at draft/submission stage by default
- optional committed procurement value tracking
- supports later invoice and payment references

### System Impact
On create:
- PO number generated
- draft stored
- audit logged

On submit:
- status changes to Submitted
- approval workflow created or advanced
- approver notified

On approve:
- status changes to Approved
- PO becomes valid for GRN
- audit and approval history updated

On reject:
- status changes to Rejected
- rejection reason stored
- requester notified

On close:
- PO marked complete
- remaining actions restricted according to policy

---

## 19. Exception Scenarios
- PO created from non-approved PR → blocked unless exception policy
- PO rejected due to vendor/pricing issue
- PO modified after approval → should require revision flow
- partial receipt against PO
- over-receipt attempt during GRN
- vendor changed after draft creation
- price mismatch between source quotation and PO
- PO approved but not received for long period → aging alert
- cancelled PO with partial receipt → special handling required

---

## 20. Audit Requirements
Track at minimum:
- PO creation
- draft updates
- submit action
- approve action
- reject action
- cancel action
- close action
- line item modifications
- commercial term changes
- vendor changes
- linked GRN creation reference

Audit should capture:
- acted by
- acted at
- old value / new value for critical changes
- action type
- status transition
- source IP / context if required by policy

---

## 21. Attachment and Comment Rules
### Attachments
Possible supporting files:
- quotation
- comparative statement
- vendor offer
- contract / agreement
- technical specification
- approval memo

### Comments
Support:
- procurement note
- approval note
- rejection reason
- vendor communication note (internal summary)
- exception note

---

## 22. Frontend Notes
### PO List UI Should Show
- PO Number
- PO Date
- Vendor
- Source PR
- Buyer
- Status
- Grand Total
- Expected Delivery Date
- Receipt Progress
- Linked GRN count

### Create PO Page Should Support
- source PR reference block
- vendor/commercial header section
- line item grid
- total calculation summary
- Save Draft
- Submit
- attachment block
- sticky action bar
- validation messages

### PO Detail Page Should Show
- status header
- vendor/commercial summary
- source PR reference
- line items
- totals block
- approval history
- comments
- attachments
- audit panel
- linked GRN section
- action buttons based on role + status

### Role-Based UI Behavior
Procurement Executive:
- can create/edit/submit in Draft
- can convert from approved PR
- cannot approve
- cannot edit after submit

Procurement Manager:
- can approve/reject when Submitted
- sees approval controls prominently
- mostly read-only view for submitted content

Store / Warehouse Team:
- can view approved PO for GRN reference
- cannot alter commercial terms

---

## 23. Backend Notes
### Core Endpoint Needs
- list POs
- create PO
- get PO detail
- update PO draft
- submit PO
- approve PO
- reject PO
- cancel PO
- close PO
- get PO history
- get PO comments
- get PO attachments
- get linked GRNs
- create PO from approved PR

### Backend Enforcement
- PR-to-PO linkage validation
- status transition checks
- maker-checker enforcement
- total and line validation
- vendor status validation
- audit logging
- approval history persistence
- role-based authorization
- future receipt quantity tracking

---

## 24. Database Notes
Recommended base tables:
- purchase_orders
- purchase_order_items
- approvals or purchase_order_approvals
- comments
- attachments
- audit_logs

Recommended key relations:
- purchase_orders.vendor_id → vendors.id
- purchase_orders.source_purchase_request_id → purchase_requests.id
- purchase_order_items.purchase_order_id → purchase_orders.id
- purchase_order_items.product_id → products.id
- purchase_order_items.source_purchase_request_line_id → purchase_request_items.id

---

## 25. Reporting / Dashboard Notes
Useful PO metrics:
- PO count by status
- PO value by vendor
- PO aging
- pending approvals
- approved POs awaiting receipt
- partial receipt count
- vendor delivery performance
- PO rejection count / reasons

---

## 26. Dependency Notes
This transaction depends on:
- Master Data: vendors, items, UOM, warehouse, tax setup, branch/department
- Approved PR data
- User / Role design
- Approval workflow
- Comments / attachments support
- Audit framework
- GRN design for downstream execution

---

## 27. Completion Criteria for PO Design
PO transaction design is considered complete when:
- header and line item structure are fixed
- source PR linkage is defined
- statuses and transitions are defined
- role actions are defined
- approval logic is defined
- validations are documented
- frontend and backend needs are explicit
- downstream GRN linkage is clear

---

## 28. Next Related Files
After this file, the recommended next files are:
1. page_pr_list.md
2. page_pr_detail.md
3. page_po_list.md
4. page_po_detail.md
5. txn_goods_receipt_note.md
