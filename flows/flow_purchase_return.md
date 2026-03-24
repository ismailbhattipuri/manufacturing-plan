# Flow: Purchase Return

## 1. Business Purpose
Purchase Return is the business flow used when goods received from a vendor must be returned fully or partially after receipt because of quality failure, damage, wrong item delivery, excess quantity, expiry risk, specification mismatch, or other approved business reasons.

This flow ensures:
- non-conforming or unwanted inbound goods are removed correctly
- vendor return is traceable to the original receipt and purchase order
- stock is corrected accurately
- quality, procurement, warehouse, and finance all remain aligned
- auditability is preserved

This is an exception/advanced procurement flow and must be handled explicitly, not as an informal stock reduction.

---

## 2. Source Alignment
This flow must remain aligned with:
- Step 2 – Procurement Module
- Step 4 – Process Flow Design
- txn_goods_receipt_note.md
- txn_inventory_movement.md
- page_grn_detail.md
- page_incoming_qc.md
- page_stock_overview.md
- role_store_user.md
- role_qa_inspector.md
- role_procurement_executive.md
- role_procurement_manager.md

---

## 3. Trigger Events
A Purchase Return may be triggered when:
- received goods fail QC inspection
- goods are damaged in delivery
- wrong item or wrong specification was supplied
- excess quantity was received and vendor agrees to take it back
- goods are near expiry or unusable
- hidden defects are found after GRN acceptance but before use/consumption
- business decides to reverse part of a receipt before downstream consumption

---

## 4. Roles Involved
### Primary Roles
- Store User / Warehouse User
- QA Inspector
- Procurement Executive
- Procurement Manager

### Secondary / Related Roles
- Store Manager
- Finance Officer
- Management (exception approval only if needed)

---

## 5. Role Responsibility Summary
### Store User
- identify physical returnable stock
- verify return quantity and storage location
- prepare goods for dispatch back to vendor
- record operational return details

### QA Inspector
- confirm quality failure or rejection basis
- record rejection reason and quality evidence
- validate return-causing defect or non-conformance

### Procurement Executive
- coordinate with vendor
- create or process purchase return document
- ensure vendor communication and document linkage

### Procurement Manager
- approve return if approval is required
- validate that return is legitimate and policy-compliant
- monitor vendor issue patterns

### Finance Officer
- process financial adjustment, debit note, vendor adjustment, or reference for invoice matching if applicable

---

## 6. Business Preconditions
Purchase Return should generally be allowed only when:
- source PO exists
- source GRN exists
- returned stock is identifiable
- quantity to return is available in returnable stock state
- goods have not already been fully consumed/issued/sold
- reason for return is recorded
- approval policy is satisfied where required

### Important Restriction
Returned quantity must not exceed the currently returnable quantity tied to the source receipt/inventory state.

---

## 7. Source Documents and References
A Purchase Return should normally link to:
- Purchase Order (PO)
- Goods Receipt Note (GRN)
- QC result / rejection record if quality-based
- inventory movement reference
- vendor reference / delivery note for reverse logistics
- finance reference (debit note / adjustment), if applicable

---

## 8. Flow Overview
Base flow:

1. Goods are received through GRN
2. QC or warehouse identifies issue
3. Returnable quantity is confirmed
4. Purchase Return is created
5. Return is reviewed/approved if policy requires
6. Stock is reduced or moved to return staging
7. Goods are physically dispatched to vendor
8. Vendor acknowledgement/financial adjustment is recorded
9. Flow is closed

---

## 9. Detailed Flow Steps

### Step 1 – Identify Returnable Goods
Trigger source:
- GRN Detail
- Incoming QC result
- Stock Overview / stock drilldown
- warehouse discrepancy review

The system/user identifies:
- item
- source GRN
- source PO
- available returnable quantity
- reason for return

### Step 2 – Validate Return Eligibility
System should validate:
- source document exists
- quantity requested for return is valid
- stock is still available in returnable state
- item has not been fully consumed/locked downstream
- user has permission to initiate return

### Step 3 – Create Purchase Return Record
A Purchase Return document/record is created with:
- vendor reference
- source PO/GRN reference
- return date
- warehouse/location
- line items and quantities
- return reason
- QC/discrepancy evidence if applicable

### Step 4 – Review / Approve Return
If policy requires:
- Procurement Manager or authorized approver reviews
- supporting reason/evidence is checked
- return is approved or rejected

For lower-risk operations, return may be operationally controlled without heavy approval, but the rule must be explicit.

### Step 5 – Prepare Stock for Return
Stock may be:
- moved to return staging
- marked return-pending
- blocked from normal usage
- separated from available stock

### Step 6 – Dispatch Return to Vendor
Warehouse records:
- dispatch date
- quantity sent
- carrier/vehicle/reference if needed
- return note/challan number

### Step 7 – Confirm Stock and Document Impact
System records:
- stock reduction or movement to vendor-return state
- source document linkage
- audit trail
- vendor return status

### Step 8 – Finance / Commercial Adjustment
If applicable:
- debit note created
- invoice value adjusted
- vendor payable adjusted
- replacement/credit path recorded

### Step 9 – Close Return
Return is closed when:
- goods physically returned
- quantity reconciled
- financial adjustment or vendor acknowledgement completed where required

---

## 10. Status Model
Recommended Purchase Return statuses:

- Draft
- Submitted
- Approved
- Rejected
- Return Pending Dispatch
- Returned
- Closed
- Cancelled

### Optional Additional Statuses
- QC Confirmed
- Finance Pending
- Partially Returned
- Vendor Acknowledged

---

## 11. Status Meaning
### Draft
Return is being prepared but not yet submitted.

### Submitted
Return is awaiting review/approval.

### Approved
Return is authorized to proceed.

### Rejected
Return request is not approved.

### Return Pending Dispatch
Return is approved but goods not yet physically sent.

### Returned
Goods have been dispatched/returned to vendor.

### Closed
Operational and business reconciliation complete.

### Cancelled
Return request withdrawn before completion.

---

## 12. Status Transition Rules
Allowed transitions:

- Draft → Submitted
- Draft → Cancelled
- Submitted → Approved
- Submitted → Rejected
- Approved → Return Pending Dispatch
- Return Pending Dispatch → Returned
- Returned → Closed
- Rejected → Draft (if revise flow exists)
- Approved → Cancelled (restricted)
- Return Pending Dispatch → Cancelled (restricted, before actual dispatch)

### Guardrails
- cannot return without a valid source record
- cannot approve invalid quantity
- cannot mark Returned before approval if approval is mandatory
- cannot close before dispatch/stock impact is recorded
- cannot return more quantity than eligible

---

## 13. Return Quantity Logic
Purchase Return quantity must be controlled carefully.

### Rules
- Return Qty ≤ eligible returnable qty
- eligible qty depends on source GRN and current stock state
- accepted and usable stock may be returnable if not consumed
- rejected stock may be directly returnable
- stock already issued, consumed, transferred, or sold should normally not be returnable through standard purchase return

### Partial Returns
Must be supported:
- one GRN line may be returned in multiple smaller returns
- system should track already returned qty vs remaining returnable qty

---

## 14. Inventory Impact
This is a critical part of the flow.

### Inventory effect options
Depending on policy:
1. immediately reduce stock on approval
2. reduce stock on dispatch/returned status
3. move stock first into return staging, then reduce on dispatch

### Recommended Pattern
- move stock from available/QC/rejected bucket into return-pending bucket
- on final dispatch, reduce stock and create outbound inventory movement

### Important Rule
Returned goods must not remain counted as usable stock after operational return is confirmed.

---

## 15. QC Impact
If return is quality-driven:
- source QC result should be linked
- rejection reason should be reused where possible
- quality evidence should remain attached
- vendor issue analytics should capture the case

If return is non-quality-driven:
- return reason still required
- QC link can be optional

---

## 16. Finance Impact
Possible finance impacts:
- debit note
- payable adjustment
- invoice matching correction
- replacement vs refund classification
- vendor claim tracking

### Rule
Operational return and financial settlement may be separate steps, but linkage must exist.

---

## 17. Document/Data Structure Guidance
A future Purchase Return transaction should contain:

### Header
- return id / return number
- vendor
- source PO
- source GRN
- return date
- warehouse
- reason summary
- status
- approved by / timestamps
- dispatch reference
- finance reference

### Line Items
- item
- source GRN line
- source PO line
- received qty
- already returned qty
- current eligible qty
- return qty
- return reason
- QC reference
- stock state source

---

## 18. UI/Page Impact
This flow will eventually require pages such as:
- Purchase Return List
- Purchase Return Create
- Purchase Return Detail
- Return Approval View (optional)
- Return Dispatch Section / Vendor Return Note section

### Existing page touchpoints
- GRN Detail should show “Create Purchase Return” where allowed
- Incoming QC should support return-triggering rejection path
- Stock Overview / drilldown may show returnable stock source
- linked return references should appear on source GRN/PO

---

## 19. API Impact
This flow will eventually require endpoints such as:
- list purchase returns
- create purchase return
- get purchase return detail
- update draft purchase return
- submit purchase return
- approve purchase return
- reject purchase return
- mark return pending dispatch
- mark returned
- close purchase return
- get linked source documents
- get returnable quantity by source line

### Important backend validations
- source document validity
- returnable qty check
- stock state check
- status transition check
- permission/approval check

---

## 20. Audit Requirements
Track at minimum:
- return creation
- qty changes
- reason changes
- submit/approve/reject actions
- dispatch/returned confirmation
- stock impact reference
- finance linkage updates
- cancellation/closure

Audit should capture:
- actor
- timestamp
- old/new values for critical fields
- status transition
- source reference

---

## 21. Exceptions and Edge Cases
- partial return of a line
- multiple returns against one GRN line
- return after partial consumption (should usually block or require exception flow)
- return after QC rejection
- return after GRN acceptance but before issue/use
- excess receipt return
- wrong item received and returned
- vendor replacement shipment expected
- financial document already posted before return

---

## 22. Reporting Needs
Useful metrics:
- return count by vendor
- return qty/value by item
- return reason analysis
- QC-driven return percentage
- vendor quality issue trend
- time from GRN to return
- open vs closed return cases

---

## 23. Completion Criteria
This flow is complete when:
- triggers are defined
- roles are defined
- source linkage is defined
- status model is defined
- quantity logic is defined
- inventory and finance impact are defined
- UI/API implications are identified
- exception handling is documented

---

## 24. Next Related Files
Recommended next files:
1. flow_sales_return.md
2. flow_rework.md
3. flow_scrap_handling.md
4. transactions/txn_purchase_return.md (future)
5. pages/procurement/page_purchase_return_detail.md (future)
