# Flow: Scrap Handling

## 1. Business Purpose
Scrap Handling is the controlled business flow used when material, semi-finished goods, finished goods, or returned goods are no longer usable, recoverable, saleable, or reworkable and must be formally classified as scrap.

This flow ensures:
- unusable stock is identified and isolated correctly
- scrap decisions are controlled and auditable
- stock quantities are reduced accurately
- quality, production, warehouse, and finance/costing impacts remain traceable
- scrap analytics support loss reduction and process improvement

Scrap must never be handled as an informal stock reduction. It requires explicit business and system treatment.

---

## 2. Source Alignment
This flow must remain aligned with:
- Step 2 – Inventory Module
- Step 2 – Production Module
- Step 2 – Quality Module
- Step 4 – Production Flow
- Step 4 – Quality Flow
- txn_inventory_movement.md
- page_stock_overview.md
- page_incoming_qc.md
- role_store_user.md
- role_qa_inspector.md

Future alignment:
- txn_work_order.md
- txn_material_issue.md
- txn_production_output.md
- txn_qc_inspection.md
- role_production_supervisor.md
- role_store_manager.md

---

## 3. Trigger Events
Scrap Handling may be triggered when:
- incoming material fails QC and is not returnable or reworkable
- in-process or finished goods fail quality checks beyond recovery
- production generates unavoidable scrap/wastage
- damaged stock is discovered in warehouse
- expired or contaminated stock is found
- customer return is found non-recoverable
- rework attempt fails
- obsolete or deteriorated stock is approved for disposal

---

## 4. Roles Involved
### Primary Roles
- Store User / Warehouse User
- QA Inspector
- Production Supervisor

### Secondary / Related Roles
- Store Manager
- Procurement Executive
- Finance Officer
- Management / authorized approver for high-value scrap
- Environmental / compliance role if needed in future

---

## 5. Role Responsibility Summary
### Store User
- identify physical stock to be scrapped
- record quantity, location, and condition
- isolate and move material into scrap state/location
- execute physical disposal or handover if approved

### QA Inspector
- confirm quality-based scrap cause
- classify material as non-recoverable
- attach defect evidence and reason codes

### Production Supervisor
- validate production scrap or process-loss related scrap
- confirm source stage or operation where scrap occurred

### Store Manager / Authorized Approver
- approve scrap where policy requires
- verify value, quantity, and reason before final disposal

### Finance Officer
- recognize inventory loss or valuation impact
- support cost/loss reporting
- process financial adjustment if required

---

## 6. Business Preconditions
Scrap should generally be allowed only when:
- stock/item/batch/serial is identifiable
- quantity is known and available in stock or process context
- source reason for scrap is documented
- material is confirmed non-usable / non-recoverable
- required approvals are completed
- stock has not already been fully consumed, disposed, or written off elsewhere

### Important Restriction
Material that can be returned or reworked should not directly enter scrap flow unless those paths are intentionally rejected or not applicable.

---

## 7. Source Documents and References
Scrap Handling may link to:
- GRN / Incoming QC
- Production Output / Work Order
- Material Issue / WIP record
- QC Inspection result
- Rework record
- Sales Return record
- Inventory movement / stock balance
- batch / lot / serial reference

---

## 8. Flow Overview
Base flow:

1. Unusable material is identified
2. Scrap eligibility is confirmed
3. Scrap record/request is created
4. Quantity is reviewed/approved if needed
5. stock moves into scrap state or scrap-hold location
6. physical disposal or write-off is recorded
7. inventory loss is posted
8. flow is closed

---

## 9. Detailed Flow Steps

### Step 1 – Identify Scrap Candidate
Trigger source may be:
- Incoming QC rejection
- In-Process QC / Final QC failure
- production floor loss report
- warehouse damage check
- expired stock review
- failed rework result
- customer return assessment

System/user captures:
- item/batch/serial
- quantity
- location
- source reference
- reason category
- evidence if applicable

### Step 2 – Confirm Non-Recoverable Status
Authorized operational/quality user confirms:
- not returnable
- not reworkable
- not reusable
- should be scrapped

This decision should be explicit for traceability.

### Step 3 – Create Scrap Record
A scrap record/request should capture:
- source document
- item/batch reference
- quantity to scrap
- scrap reason
- warehouse/location
- defect/cause
- status
- requested by

### Step 4 – Review / Approve Scrap
If policy requires:
- store manager or authorized approver reviews
- high-value or unusual scrap is escalated
- approval/rejection is recorded

### Step 5 – Move Stock to Scrap State
System records movement such as:
- available / QC hold / rejected / rework → scrap-hold
- isolate stock from normal usage
- preserve traceability to source

### Step 6 – Record Final Scrap / Disposal
Warehouse or authorized role records:
- final scrapped quantity
- disposal date
- disposal method / reference if needed
- witness/approval reference if required by policy

### Step 7 – Post Inventory and Financial Impact
System records:
- stock reduction
- inventory movement to scrap
- loss/valuation effect
- reporting impact

### Step 8 – Close Scrap Case
Scrap case is closed when:
- quantity is fully reconciled
- stock impact is posted
- approvals/audit are complete
- disposal is recorded where required

---

## 10. Status Model
Recommended Scrap Handling statuses:

- Draft
- Submitted
- Approved
- Rejected
- Scrap Pending
- Scrapped
- Closed
- Cancelled

### Optional Additional Statuses
- Under Review
- Disposal Pending
- Finance Pending
- Evidence Pending

---

## 11. Status Meaning
### Draft
Scrap case is being prepared.

### Submitted
Awaiting review/approval.

### Approved
Authorized to proceed.

### Rejected
Scrap request was not approved.

### Scrap Pending
Approved, but final physical scrap/disposal not yet recorded.

### Scrapped
Stock has been formally scrapped / written off operationally.

### Closed
All reconciliation and control steps completed.

### Cancelled
Request withdrawn before completion.

---

## 12. Status Transition Rules
Allowed transitions:

- Draft → Submitted
- Draft → Cancelled
- Submitted → Approved
- Submitted → Rejected
- Approved → Scrap Pending
- Scrap Pending → Scrapped
- Scrapped → Closed
- Rejected → Draft (if revision allowed)

### Guardrails
- cannot scrap without quantity and source identification
- cannot mark Scrapped before required approval
- cannot close before stock impact is recorded
- cannot scrap more than eligible quantity
- cannot return scrapped stock to normal use without explicit reversal flow

---

## 13. Quantity Logic
Scrap quantity must be controlled carefully.

### Rules
- Scrap Qty ≤ eligible quantity
- eligible qty depends on source state and currently available physical quantity
- partial scrap must be supported
- repeated scrap entries against same source must track remaining eligible qty

### Reconciliation
If scrap comes from rework or QC:
- accepted qty + rework qty + scrap qty should reconcile with source quantity as applicable

---

## 14. Inventory Impact
This is a core part of the flow.

### Typical stock state transitions
- Available → Scrap Hold → Scrapped
- QC Hold → Scrap Hold → Scrapped
- Rejected → Scrapped
- Rework → Scrapped (if rework fails)

### Important Rule
Scrapped quantity must be removed from usable inventory and must not appear in available stock.

### Recommended Pattern
- isolate in scrap-hold first if approval/disposal is delayed
- final scrapped status triggers stock write-off/inventory movement

---

## 15. Production Impact
If scrap originates from production:
- source operation or work order should be recorded
- scrap loss should be attributable to process stage
- variance/yield reporting should include scrap outcome
- production analytics should track scrap by item/batch/operation

---

## 16. Quality Impact
If scrap is quality-driven:
- source QC record should be linked
- defect category should be recorded
- trend analysis should capture repeat failures
- root-cause reporting should be enabled

---

## 17. Finance / Cost Impact
Possible finance/cost effects:
- inventory write-off
- variance/loss recognition
- production loss costing
- damage/obsolescence reporting
- management reporting on scrap value

### Rule
Operational scrap and financial loss recognition may be separate steps, but linkage must exist.

---

## 18. Document/Data Structure Guidance
A future Scrap transaction/record should contain:

### Header
- scrap id / number
- source type/document
- item/batch reference
- warehouse/location
- reason category
- requested qty
- approved qty
- status
- approved by / timestamps
- disposal reference

### Line / detail
- item
- source line
- quantity
- defect reason
- qc/rework reference
- stock state source
- value estimate if tracked

---

## 19. UI/Page Impact
This flow will eventually require pages such as:
- Scrap List
- Scrap Detail
- Scrap Approval View
- Scrap Disposal Confirmation page
- Scrap analytics/report page

### Existing page touchpoints
- Incoming QC may support “Move to Scrap”
- future production/QC pages may support “Scrap” disposition
- Stock Overview may show scrap-hold / blocked stock
- linked scrap references should appear on source GRN/rework/production records

---

## 20. API Impact
This flow will eventually require endpoints such as:
- list scrap records
- create scrap record
- get scrap detail
- submit scrap
- approve/reject scrap
- mark scrap pending
- mark scrapped
- close scrap
- get scrapable qty by source reference

### Important backend validations
- source validity
- quantity eligibility
- stock-state eligibility
- approval rule validation
- status transition validation
- audit enforcement

---

## 21. Audit Requirements
Track at minimum:
- scrap creation
- qty changes
- reason changes
- approval/rejection
- stock movement to scrap-hold
- final scrapped confirmation
- closure/cancellation
- finance linkage updates if used

Audit should capture:
- actor
- timestamp
- source reference
- old/new values for critical fields
- status transition

---

## 22. Exceptions and Edge Cases
- partial scrap
- repeated scrap against same source
- expired stock scrap
- damaged warehouse stock scrap
- QC failure to scrap
- rework failure to scrap
- customer return to scrap
- high-value scrap requiring extra approval
- disposal documentation required by compliance
- mistaken scrap requiring reversal workflow

---

## 23. Reporting Needs
Useful metrics:
- scrap qty/value by item
- scrap by source (QC, production, warehouse damage, expiry)
- scrap reason trend
- scrap by batch/lot
- scrap by warehouse
- production scrap yield loss
- rework failure → scrap rate
- monthly scrap value trend

---

## 24. Completion Criteria
This flow is complete when:
- triggers are defined
- roles are defined
- status model is defined
- quantity logic is defined
- inventory/production/quality/finance impacts are defined
- UI/API implications are identified
- audit and edge cases are documented

---

## 25. Next Related Files
Recommended next files:
1. flow_subcontracting.md
2. transactions/txn_scrap.md (future)
3. pages/inventory/page_scrap_detail.md (future)
4. pages/production/page_scrap_review.md (future)
