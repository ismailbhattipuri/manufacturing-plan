# Flow: Rework

## 1. Business Purpose
Rework is the controlled operational flow used when a finished good, semi-finished good, or received material does not meet required quality or specification standards but can still be corrected, repaired, reprocessed, or reworked instead of being scrapped or returned.

This flow ensures:
- non-conforming material is handled systematically
- recoverable stock is not wasted unnecessarily
- production, quality, inventory, and cost impact remain traceable
- rework operations are planned and executed with control
- final disposition after rework is recorded clearly

Rework is a critical manufacturing and quality exception flow.

---

## 2. Source Alignment
This flow must remain aligned with:
- Step 2 – Production Module
- Step 2 – Quality Module
- Step 4 – Production Flow
- Step 4 – Quality Flow
- txn_inventory_movement.md
- page_incoming_qc.md
- page_stock_overview.md
- role_qa_inspector.md
- role_store_user.md

Future alignment:
- txn_work_order.md
- txn_production_output.md
- txn_qc_inspection.md
- role_production_supervisor.md
- role_production_planner.md

---

## 3. Trigger Events
Rework may be triggered when:
- finished goods fail final QC but are recoverable
- in-process output fails inspection and needs correction
- incoming material fails QC but can be reprocessed internally
- customer returns are classified as repairable/reworkable
- production variance causes a part to need corrective processing
- batch quality issues affect only part of a produced lot and rework is viable

---

## 4. Roles Involved
### Primary Roles
- QA Inspector
- Production Supervisor
- Production Planner
- Store User / Warehouse User

### Secondary / Related Roles
- QA Manager (future)
- Procurement Team (if source was incoming material)
- Sales/Service Team (if triggered by customer return)
- Finance / Costing (for valuation impact if needed)

---

## 5. Role Responsibility Summary
### QA Inspector
- identify non-conformance
- classify item as reworkable vs scrap/reject
- record defect type and reason
- approve or reject rework outcome after processing

### Production Supervisor
- evaluate practical rework execution method
- supervise rework operation
- ensure corrected product/process output is recorded

### Production Planner
- plan rework job / schedule / capacity
- allocate rework order if formal production structure is used

### Store User
- move rejected or reworkable material to rework location/staging
- record inventory movement into and out of rework state

---

## 6. Business Preconditions
Rework should generally be allowed only when:
- non-conforming goods/material are identifiable
- quantity is known
- item/batch/lot is traceable
- material is classified as reworkable by QA or authorized operational role
- inventory/production state allows controlled rework
- rework route/process is known or approved

### Important Restriction
Material already fully scrapped, consumed beyond recovery, or legally unusable must not enter rework flow.

---

## 7. Source Documents and References
A Rework flow may link to:
- QC Inspection result
- GRN / incoming receipt
- Work Order / Production Output
- Batch / lot / serial reference
- Sales Return record
- Inventory movement record
- defect / non-conformance report
- future rework order document

---

## 8. Flow Overview
Base flow:

1. Non-conformance identified
2. QA classifies item as reworkable
3. Rework record/order is created
4. inventory is moved to rework state/location
5. rework operation is planned/executed
6. re-inspection is performed
7. output is classified as accepted, partially accepted, or scrap
8. stock and production records are updated
9. flow is closed

---

## 9. Detailed Flow Steps

### Step 1 – Identify Non-Conforming Material
Trigger source may be:
- Incoming QC
- In-Process QC
- Final QC
- Sales Return inspection
- stock review or defect investigation

System/user captures:
- item/batch/lot
- non-conforming quantity
- defect type
- source document
- current stock/location/state

### Step 2 – Classify as Reworkable
QA or authorized decision-maker determines:
- material is reworkable
- rework method exists
- expected outcome is economically and operationally acceptable

If not reworkable:
- move to scrap or purchase return / sales return / reject flow

### Step 3 – Create Rework Record / Rework Order
A rework document or operation record should capture:
- source reference
- item/batch reference
- quantity for rework
- rework reason
- defect classification
- planned rework process
- assigned team / work center
- status

### Step 4 – Move Inventory to Rework State
System records movement such as:
- available / rejected / QC-hold stock → rework staging
- work-in-progress rework allocation if production-controlled
- batch/serial traceability retained

### Step 5 – Execute Rework
Production executes corrective processing:
- repair
- reprocessing
- reassembly
- re-blending / re-finishing
- corrective adjustment

### Step 6 – Record Rework Output
System captures:
- input quantity
- recovered quantity
- remaining rejected quantity
- process notes
- wastage generated during rework
- time/resource/cost if tracked

### Step 7 – Re-Inspect Output
QA validates reworked output:
- accepted
- partially accepted
- failed again / scrap
- further rework required (if policy allows multi-pass rework)

### Step 8 – Update Stock / Production State
Based on final disposition:
- accepted quantity moves to usable/available stock or next process
- scrap quantity moves to scrap flow
- remaining unresolved quantity may stay in rework or be blocked

### Step 9 – Close Rework
Flow is closed when:
- quantities are fully reconciled
- final accepted/scrap disposition is recorded
- all related stock movements and audit records exist

---

## 10. Status Model
Recommended Rework statuses:

- Draft
- Submitted
- Approved
- In Rework
- Re-Inspection Pending
- Accepted
- Partially Accepted
- Rejected
- Closed
- Cancelled

### Optional Additional Statuses
- Planned
- Released
- On Hold
- Further Rework Required

---

## 11. Status Meaning
### Draft
Rework case created but not yet approved/released.

### Submitted
Awaiting operational/QA approval if needed.

### Approved
Authorized to proceed.

### In Rework
Material is actively being reworked.

### Re-Inspection Pending
Rework completed and awaiting QA review.

### Accepted
Reworked output is approved for use.

### Partially Accepted
Some quantity recovered, some failed.

### Rejected
Rework unsuccessful; material requires scrap or alternate disposition.

### Closed
All quantity and stock outcomes are finalized.

### Cancelled
Rework request withdrawn before completion.

---

## 12. Status Transition Rules
Allowed transitions:

- Draft → Submitted
- Draft → Cancelled
- Submitted → Approved
- Submitted → Rejected
- Approved → In Rework
- In Rework → Re-Inspection Pending
- Re-Inspection Pending → Accepted
- Re-Inspection Pending → Partially Accepted
- Re-Inspection Pending → Rejected
- Accepted → Closed
- Partially Accepted → Closed
- Rejected → Closed

### Guardrails
- cannot enter rework without identified source and quantity
- cannot mark Accepted without re-inspection if QA gate is required
- cannot close before quantities reconcile
- cannot move rejected quantity into usable stock

---

## 13. Quantity Logic
Rework quantity must be controlled carefully.

### Rules
- Rework input qty ≤ currently reworkable qty
- Accepted qty + rejected qty + scrap/wastage qty must reconcile with rework input qty
- Partial recovery must be supported
- Multi-pass rework should be explicit if allowed

### Example
Input: 100 units  
Accepted after rework: 82  
Scrap after rework: 18  
System must reconcile 82 + 18 = 100

---

## 14. Inventory Impact
This is a major part of the flow.

### Typical stock state transitions
- QC Hold / Rejected / Blocked → Rework
- Rework → Available / Finished / WIP / next production stage
- Rework failure → Scrap / blocked stock

### Recommended Pattern
Track stock states distinctly:
- Available
- QC Hold
- Rejected
- Rework
- Scrap

### Important Rule
Material in rework should not appear as normal available stock until accepted.

---

## 15. Production Impact
Rework may affect:
- capacity planning
- work center allocation
- labor/time usage
- production efficiency
- output/yield reporting
- cost capture

If formal production control is used, rework may need:
- separate rework order
- linked work order reference
- operation step logging

---

## 16. Quality Impact
Quality is central to rework.

### Required quality controls
- defect capture
- reason coding
- reworkability classification
- re-inspection result
- final disposition approval

### Analytics impact
Rework should support:
- defect trend analysis
- repeated-failure tracking
- rework cost and yield loss visibility
- root-cause improvement programs

---

## 17. Finance / Cost Impact
Possible finance/cost effects:
- additional labor cost
- additional machine/resource cost
- valuation adjustment
- scrap loss recognition for unrecovered quantity
- impact on production cost or batch cost

Operational ERP may initially track this lightly, but linkage should exist for future costing.

---

## 18. Document/Data Structure Guidance
A future Rework transaction/order should contain:

### Header
- rework id / number
- source type/document
- item/batch reference
- source quantity
- rework quantity
- defect reason
- rework method / work center
- assigned owner
- status

### Line / result details
- input qty
- accepted qty
- rejected qty
- scrap qty
- re-inspection result
- notes
- time/cost fields if tracked

---

## 19. UI/Page Impact
This flow will eventually require pages such as:
- Rework Queue / List
- Rework Detail
- Rework Execution page
- Re-Inspection page
- Rework analytics/report page

### Existing page touchpoints
- Incoming QC should support “Send to Rework”
- future QC inspection pages should support rework disposition
- Stock Overview may show rework stock bucket
- source GRN / production output / sales return should show linked rework references

---

## 20. API Impact
This flow will eventually require endpoints such as:
- list rework records
- create rework record
- get rework detail
- submit/approve/reject rework
- start rework
- record rework output
- mark re-inspection pending
- accept rework result
- reject rework result
- close rework
- get reworkable qty by source reference

### Important backend validations
- source validity
- quantity reconciliation
- stock state eligibility
- role authorization
- status transition validity
- accepted vs scrap reconciliation

---

## 21. Audit Requirements
Track at minimum:
- rework creation
- defect classification
- qty changes
- approval/release
- start rework
- output recording
- re-inspection result
- stock state transitions
- closure/cancellation

Audit should capture:
- actor
- timestamp
- old/new values for critical fields
- source reference
- status transition

---

## 22. Exceptions and Edge Cases
- partial rework success
- repeated rework attempt on same batch
- rework fails and moves to scrap
- customer return enters rework path
- incoming material enters rework path
- production output enters rework path
- batch/serial split during rework
- cost of rework exceeds threshold
- item requires further QA escalation

---

## 23. Reporting Needs
Useful metrics:
- rework count by item/batch
- defect reason trend
- rework recovery %
- rework failure %
- scrap after rework
- average rework cycle time
- rework cost / loss estimate
- repeat defect frequency

---

## 24. Completion Criteria
This flow is complete when:
- triggers are defined
- roles are defined
- status model is defined
- quantity reconciliation is defined
- inventory/production/quality impacts are defined
- UI/API implications are identified
- audit and edge cases are documented

---

## 25. Next Related Files
Recommended next files:
1. flow_scrap_handling.md
2. flow_subcontracting.md
3. transactions/txn_rework.md (future)
4. pages/production/page_rework_detail.md (future)
