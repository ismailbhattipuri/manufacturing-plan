# Transaction: Goods Receipt Note (GRN)

## 1. Business Purpose
Goods Receipt Note (GRN) is the transaction used to record the physical receipt of goods against an approved Purchase Order (PO). It confirms what was received, where it was received, in what quantity and condition, and whether further quality inspection is required.

This transaction continues the procurement lifecycle:
PR → Approved → PO → Approved → GRN → Completed

GRN is the operational bridge between procurement and inventory. It is the point where ordered goods become receivable stock, subject to business and quality rules.

---

## 2. Source Alignment
This transaction file is derived from and must remain aligned with:
- Step 2 – Procurement Module
- Step 3 – User Role Design
- Step 4 – Procurement Flow
- Step 5 – Screen Map
- Step 8 – Backend API
- Step 9 – Frontend Development
- txn_purchase_order.md
- txn_purchase_requisition.md
- page_po_detail.md

---

## 3. Trigger / When GRN Is Created
A GRN is created when:
- goods arrive from a vendor against an approved PO
- the warehouse/store team begins receipt processing
- partial delivery is received against an order
- receipt must be recorded before inventory becomes usable
- receipt requires QC routing before final stock acceptance

Typical triggers:
- Receive Goods action from Approved PO
- warehouse inbound processing
- partial vendor delivery against open PO
- manual receipt entry from delivery documentation, with PO reference

---

## 4. Roles Involved
### Primary Roles
- Store User / Warehouse User
- Store Manager (optional approval/control role)

### Secondary / Related Roles
- Procurement Executive
- Procurement Manager
- QA Inspector / QA Team
- Finance Team (view/reference only)
- Admin (configuration or audit support only)

---

## 5. Role Responsibility Summary
### Store User / Warehouse User
- create GRN against approved PO
- record received quantity
- record warehouse/bin/location
- capture delivery reference and receiving notes
- submit GRN for QC or acceptance as per policy

### Store Manager
- review exceptional receipts
- approve controlled receipt adjustments if workflow requires
- monitor receipt completion and discrepancies

### QA Team
- inspect goods if QC-required material is received
- approve or reject quality disposition
- release accepted goods to inventory if business flow requires QC gate

### Procurement Team
- view receipt progress
- track shortages, over-delivery attempts, or vendor delivery behavior

---

## 6. Transaction Type
- Category: Transaction Header + Line Item
- Module: Procurement / Inventory boundary transaction
- Upstream Source: Approved Purchase Order (PO)
- Downstream Target: Stock update / Inventory movement / QC record

---

## 7. Page Coverage
This transaction is represented across these pages:
- GRN List
- GRN Entry
- GRN Detail View

Related future pages:
- Inbound Receipt Queue
- QC Pending Receipt Queue
- Vendor Delivery Performance View
- Receipt Discrepancy Report

---

## 8. Core Business Outcome
The GRN ensures:
- physical receipt is recorded formally
- receipt is linked to approved procurement
- ordered vs received quantity is traceable
- stock is updated correctly
- QC-required goods are routed properly
- downstream inventory and audit records remain reliable

---

## 9. GRN Header Fields
Recommended header fields:

### Identification
- GRN ID (internal system ID)
- GRN Number / Receipt No
- GRN Date
- Receipt Type
- Receipt Source

### Source References
- Source PO ID
- Source PO Number
- Source Vendor ID
- Source Vendor Name Snapshot (optional)
- Delivery Note / Challan Number
- Invoice Reference (optional)
- Vehicle / Shipment Reference (optional)

### Receiving Context
- Received By User ID
- Receiving Warehouse
- Receiving Location / Bin
- Branch ID
- Plant / Factory ID (future-ready)
- Receipt Time
- Gate Entry Reference (optional)

### Operational Details
- Delivery Date
- Supplier Delivery Reference
- Inward Note
- QC Required Overall Flag
- Receipt Remark
- Discrepancy Flag

### Workflow / Status
- Current Status
- Submitted At
- QC Started At
- Accepted At
- Rejected At
- Cancelled At
- Processed By
- Rejection Reason

### Audit / System
- Created At
- Created By
- Updated At
- Updated By
- Version No
- Is Active / Archived Flag if needed

---

## 10. GRN Line Item Fields
Each GRN may contain one or more received lines.

### Item Identification
- Line ID
- Product / Item ID
- Item Code
- Item Name / Description
- UOM

### PO Reference
- Source PO Line ID
- Ordered Quantity
- Previously Received Quantity
- Remaining Open Quantity

### Receipt Quantity
- Received Quantity
- Accepted Quantity
- Rejected Quantity
- Damaged Quantity (optional)
- Short Quantity (derived or captured)
- Excess Quantity (derived or captured, normally blocked)

### Receiving Context
- Warehouse ID
- Bin / Location ID
- Batch No (if applicable)
- Serial Numbers (if applicable)
- Expiry Date (if applicable)
- Manufacturing Date (if applicable)
- QC Required Flag

### Quality / Remarks
- Condition Note
- Inspection Note
- Line Remark
- Rejection Reason
- Attachment Indicator if required

---

## 11. Status Model
Base GRN status flow:

- Pending
- Received
- QC Pending
- Accepted
- Rejected
- Cancelled

Optional future statuses:
- Partially Accepted
- Partially Rejected
- Closed
- Returned to Vendor

### Status Meaning
#### Pending
GRN is initiated but not finalized.

#### Received
Goods receipt has been recorded operationally.

#### QC Pending
Goods received but awaiting quality disposition.

#### Accepted
Goods accepted and eligible for inventory update or final stock posting.

#### Rejected
Goods rejected fully or materially enough to fail receipt acceptance.

#### Cancelled
GRN entry cancelled as per policy before final impact.

---

## 12. Status Transition Rules
Allowed transitions:

- Pending → Received
- Received → QC Pending
- Received → Accepted
- Received → Rejected
- QC Pending → Accepted
- QC Pending → Rejected
- Pending → Cancelled
- Received → Cancelled (restricted, before stock impact if policy allows)
- Accepted → Closed (future, if close state is used)

### Transition Guardrails
- cannot create GRN from non-approved PO unless exception policy explicitly allows
- cannot receive more than remaining PO quantity unless over-receipt exception exists
- cannot mark Accepted if mandatory QC is pending
- cannot edit finalized accepted receipt without reversal/correction flow
- cannot silently overwrite received quantities after stock impact
- rejected quantity handling must be explicit

---

## 13. Actions by Status
### Pending
Allowed actions:
- Save Draft / Save Pending
- Edit
- Add / Update Receipt Quantities
- Add Receiving Details
- Cancel
- Submit / Mark Received

### Received
Allowed actions:
- View
- Send to QC (if required)
- Accept (if QC not required)
- Reject
- Comment
- Attach Documents

### QC Pending
Allowed actions:
- View
- Record Inspection Result
- Accept
- Reject
- Comment

### Accepted
Allowed actions:
- View
- Print / Export
- View Stock Impact
- View Linked QC / Inventory Movement

### Rejected
Allowed actions:
- View
- View Rejection Details
- Print / Export
- Initiate Return / discrepancy flow in future

### Cancelled
Allowed actions:
- View only

---

## 14. Approval / Control Logic
GRN may be operational rather than manager-approved in simple flows, but control rules must still exist.

### Standard Operational Model
- Store User records receipt
- QA validates quality if required
- system finalizes stock update based on acceptance

### Optional Extended Control
- Store Manager approval required for discrepancy / over-receipt / unusual conditions
- QA Manager approval for rejected or conditional acceptance items

### Rules
- receipt must reference Approved PO
- accepted quantity should not exceed received quantity
- rejected quantity + accepted quantity should reconcile with received quantity
- QC-required lines cannot bypass quality gate
- maker-checker may apply for exceptional receipt adjustments, not all standard receipts

---

## 15. Validation Rules
### Header-Level Validation
- GRN date required
- source PO required
- receiving warehouse required
- receiver required
- branch/location context required where policy applies
- delivery reference required if business policy mandates it

### Line-Level Validation
- at least one line item required
- source PO line required
- received quantity must be greater than 0 for active lines
- received quantity should not exceed remaining PO quantity unless authorized exception
- accepted quantity must not exceed received quantity
- rejected quantity must not exceed received quantity
- accepted + rejected (+ damaged if used) must reconcile
- batch/serial required when item policy requires it
- expiry/manufacturing date required for applicable items

### Acceptance Validation
Before accept:
- receipt status must be valid
- QC-required items must have QC completion if policy requires
- warehouse/bin must be valid
- quantities must reconcile
- authorized user must perform action

### Rejection Validation
Before reject:
- reason required
- rejected quantity must be explicit
- downstream inventory update must be blocked or adjusted correctly

---

## 16. Derived / Calculated Fields
Recommended calculated values:
- Remaining PO Quantity = Ordered - Previously Received - Current Accepted
- Short Quantity = Expected/Remaining reference - Received
- Excess Quantity = Received - Remaining open quantity
- Receipt Completion %
- Accepted % / Rejected %
- QC Pending Count
- Time Since Receipt

---

## 17. Linked Documents and Relations
### Upstream
- Purchase Order (PO)
- indirectly Purchase Requisition (PR) through PO

### Downstream
- Inventory Movement
- Stock Balance Update
- QC Inspection Record
- Return to Vendor flow (future)
- Invoice matching context (future)

### Related References
- attachments
- comments
- approval / control history if used
- audit log
- warehouse/location records
- batch/serial records

---

## 18. Inventory / Finance / System Impact
### Inventory Impact
This is the most important impact of GRN.

Depending on business rule:
- stock may increase immediately on receipt acceptance
- or stock may move first into QC-hold / quarantine stock
- accepted goods update available/on-hand stock
- rejected goods should not become unrestricted stock

### Finance Impact
- no direct payment posting at GRN stage by default
- may support later 3-way match context (PO, GRN, Invoice)
- may influence accrual or received-not-invoiced reporting in future

### System Impact
On create:
- GRN number generated
- draft/pending receipt stored
- audit log written

On mark received:
- receipt record stored with quantities and location
- status changes to Received or QC Pending
- procurement/warehouse visibility updated

On accept:
- inventory movement created
- stock balance updated
- receipt completion updated against PO
- audit and quality history written

On reject:
- rejection reason stored
- stock posting blocked or rejected quantity isolated
- procurement notified for vendor follow-up

---

## 19. Exception Scenarios
- partial delivery against PO
- over-delivery attempt
- under-delivery / short receipt
- damaged goods on arrival
- QC failure after receipt
- wrong warehouse/location capture
- duplicate receipt attempt for same PO quantity
- cancelled GRN after partial processing
- serial/batch mismatch
- PO already closed but receipt attempted

---

## 20. Audit Requirements
Track at minimum:
- GRN creation
- pending edits
- receipt confirmation
- QC routing
- acceptance
- rejection
- cancellation
- line quantity changes
- warehouse/bin changes
- batch/serial capture changes
- inventory posting reference

Audit should capture:
- acted by
- acted at
- action type
- old/new value summary for critical changes
- status transition
- source context

---

## 21. Attachment and Comment Rules
### Attachments
Possible supporting files:
- supplier delivery note
- packing slip
- gate entry document
- QC image/photo evidence
- discrepancy evidence
- rejection support document

### Comments
Support:
- receiving note
- QC note
- rejection comment
- discrepancy comment
- internal coordination note

---

## 22. Frontend Notes
### GRN List UI Should Show
- GRN Number
- GRN Date
- Source PO
- Vendor
- Warehouse
- Received By
- Status
- Accepted / Rejected summary
- QC Pending indicator
- Last Updated

### GRN Entry Page Should Support
- source PO reference block
- receivable line item list from PO
- received quantity entry
- warehouse/bin selection
- batch/serial input where required
- discrepancy note
- Save Pending
- Mark Received / Submit
- attachment block
- sticky action bar

### GRN Detail Page Should Show
- status header
- source PO summary
- receipt summary
- line items with quantity breakdown
- QC/acceptance section
- comments
- attachments
- audit panel
- linked inventory movement / QC references

### Role-Based UI Behavior
Store User:
- can create/edit pending receipt
- can mark received
- can record location and quantity
- cannot approve PO

QA Team:
- sees QC-required receipts
- can accept/reject based on inspection outcome

Procurement Team:
- view receipt progress and discrepancies
- no warehouse data mutation rights unless policy permits

---

## 23. Backend Notes
### Core Endpoint Needs
- list GRNs
- create GRN from approved PO
- get GRN detail
- update pending GRN
- mark GRN received
- send GRN to QC
- accept GRN
- reject GRN
- cancel GRN
- get GRN history
- get GRN comments
- get GRN attachments
- get linked inventory movement
- get receipt impact summary

### Backend Enforcement
- PO status validation
- remaining quantity validation
- batch/serial validation
- accepted vs rejected quantity reconciliation
- QC gate enforcement
- inventory posting integrity
- audit logging
- role-based authorization
- prevention of duplicate or excessive receipt posting

---

## 24. Database Notes
Recommended base tables:
- goods_receipts
- goods_receipt_items
- inventory_movements
- stock_balances
- qc_inspections (or linked quality tables)
- comments
- attachments
- audit_logs

Recommended key relations:
- goods_receipts.source_purchase_order_id → purchase_orders.id
- goods_receipt_items.goods_receipt_id → goods_receipts.id
- goods_receipt_items.source_purchase_order_line_id → purchase_order_items.id
- goods_receipt_items.product_id → products.id
- goods_receipts.receiving_warehouse_id → warehouses.id
- inventory_movements.source_record_id → goods_receipts.id

---

## 25. Reporting / Dashboard Notes
Useful GRN metrics:
- GRN count by status
- PO receipt completion
- pending QC receipts
- rejected receipt count
- vendor delivery performance
- short receipt / over-receipt incidents
- average receiving time
- warehouse inbound volume

---

## 26. Dependency Notes
This transaction depends on:
- approved PO data
- item / warehouse / location master data
- role definitions for store and QA users
- inventory posting logic
- QC logic for flagged items
- comments / attachments support
- audit framework

---

## 27. Completion Criteria for GRN Design
GRN transaction design is considered complete when:
- header and line item structure are fixed
- PO linkage is defined
- statuses and transitions are defined
- receipt, QC, and acceptance rules are defined
- validations are documented
- inventory impact is explicit
- frontend and backend needs are explicit
- downstream stock/QC linkage is clear

---

## 28. Next Related Files
After this file, the recommended next files are:
1. page_grn_entry.md
2. page_grn_detail.md
3. role_store_user.md
4. role_qa_inspector.md
5. connection_document_to_document.md
