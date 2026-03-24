# Transaction: Rework

## 1. Business Purpose
Rework is the transaction used to formally manage recoverable non-conforming material, semi-finished goods, finished goods, or returned items that can be corrected, repaired, reprocessed, or reconditioned instead of being scrapped or returned.

This transaction ensures:
- recoverable quality failures are handled in a controlled way
- rework quantities are tracked from source to final disposition
- inventory state changes are traceable
- production and quality teams work from the same controlled record
- accepted, partially accepted, rejected, and scrap outcomes are formally recorded
- auditability is preserved across the full rework lifecycle

Rework is a core manufacturing and quality exception transaction and must not be handled informally.

---

## 2. Source Alignment
This transaction must remain aligned with:
- flow_rework.md
- txn_qc_inspection.md
- txn_inventory_movement.md
- txn_scrap.md
- page_incoming_qc.md
- page_stock_overview.md
- role_qa_inspector.md

Future alignment:
- txn_work_order.md
- txn_production_output.md
- role_production_supervisor.md
- role_production_planner.md
- page_rework_detail.md (future)

---

## 3. Trigger / When Rework Is Created
A Rework transaction may be created when:
- incoming material fails QC but is recoverable
- in-process output fails inspection but can be corrected
- finished goods fail final QC and are reworkable
- returned goods are classified as repairable/reworkable
- re-inspection after prior failure requires controlled rework
- production supervisor and QA agree material can be recovered

Typical triggers:
- QC disposition = Rework Recommended
- Production rejection classified as recoverable
- sales return inspection marks item as repairable
- failed output is routed back to production for correction

---

## 4. Roles Involved
### Primary Roles
- QA Inspector
- Production Supervisor
- Production Planner

### Secondary / Related Roles
- Store User / Warehouse User
- QA Manager (future)
- Finance / costing team (view only if needed)
- Procurement Team (if source is incoming material)

---

## 5. Role Responsibility Summary
### QA Inspector
- classify source item as reworkable
- define or confirm defect reason
- approve/reject final re-inspection result

### Production Supervisor
- own execution of rework
- confirm process/method
- record output after rework

### Production Planner
- schedule or release rework activity
- allocate rework capacity or work center if formal planning is used

### Store User
- move stock into and out of rework state/location
- maintain inventory traceability

---

## 6. Transaction Type
- Category: Quality / Production Exception Transaction
- Module: Production / Quality / Inventory boundary
- Upstream Source: QC failure / production non-conformance / return inspection
- Downstream Target: accepted stock / next process / scrap / further rework

---

## 7. Core Business Outcome
The Rework transaction ensures:
- reworkable material is not mixed with normal stock
- rework quantities are traceable
- final recovered output is recorded
- failed rework can move to scrap or other controlled disposition
- yield, recovery, and loss can be measured

---

## 8. Header Fields

### Identification
- Rework ID
- Rework Number
- Rework Date

### Source Context
- Source Module
- Source Document Number
- Source Line ID
- Source Batch / Lot / Serial
- Source Item ID
- Source Item Code
- Source Item Name

### Responsibility
- Requested By
- Assigned To
- Approved By
- Re-Inspected By

### Operational Context
- Warehouse / Rework Location
- Work Center / Process Step (if applicable)
- Rework Method / Notes
- Defect Category
- Rework Reason

### Audit Fields
- Created At
- Created By
- Updated At
- Updated By

---

## 9. Quantity / Result Fields

### Source / Input
- Reworkable Quantity
- Rework Input Quantity

### Output
- Accepted Quantity
- Rejected Quantity
- Scrap Quantity
- Further Rework Quantity (optional if multi-pass allowed)

### Reconciliation
- Accepted + Rejected + Scrap + Further Rework = Input Quantity

### Additional Fields
- Wastage Qty (if separately tracked)
- Time Used
- Resource / Cost Note (optional future)

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
- Completed
- Cancelled

---

## 11. Status Meaning

### Draft
Rework case being prepared

### Submitted
Awaiting approval/release

### Approved
Authorized to proceed

### In Rework
Material is under corrective process

### Re-Inspection Pending
Rework completed, waiting for QA check

### Accepted
Reworked output accepted

### Partially Accepted
Some qty recovered, some not

### Rejected
Rework failed / not accepted

### Completed
All quantity outcomes posted and reconciled

### Cancelled
Rework withdrawn before completion

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
- Accepted → Completed
- Partially Accepted → Completed
- Rejected → Completed

### Guardrails
- cannot start rework without identified source and quantity
- cannot complete without reconciled output quantities
- cannot move accepted qty to usable stock before re-inspection if QA gate is required
- cannot classify rejected rework output as accepted stock

---

## 13. Quantity Logic
This is the most important control layer.

### Base Rules
- Rework Input Qty ≤ currently eligible reworkable qty
- Accepted Qty + Rejected Qty + Scrap Qty + Further Rework Qty = Input Qty
- zero-input rework should be blocked
- partial recovery must be supported

### Examples
Input = 100  
Accepted = 80  
Rejected = 10  
Scrap = 10  
System validates 80 + 10 + 10 = 100

Input = 50  
Accepted = 20  
Further Rework = 30  
System validates 20 + 30 = 50

---

## 14. Eligibility Rules
Rework should generally only apply when:
- source qty is identifiable
- item is classified reworkable
- stock/process state permits controlled rework
- item is not already finalized as scrap or returned beyond recovery

### Typically Eligible
- QC-held stock
- rejected production output
- failed final QC output
- repairable sales returns
- recoverable incoming material

### Typically Not Eligible
- fully scrapped material
- prohibited or legally non-usable items
- stock already consumed beyond recovery
- blocked stock with no approved rework path

---

## 15. Inventory Impact
Rework changes stock state in a controlled way.

### Typical stock state transitions
- QC Hold / Rejected → Rework
- Rework → Available / next production stage
- Rework → Scrap (failed)
- Rework → Rejected / blocked (failed or pending further action)

### Important Rule
Material in rework must not appear as normal available stock until accepted.

### Recommended Posting Pattern
- move input qty into rework state/location
- on final result:
  - accepted qty → usable/available or next stage
  - scrap qty → scrap flow/state
  - rejected qty → blocked/rejected state
  - further rework qty → remain in rework state if supported

---

## 16. Production Impact
Rework may consume:
- labor
- machine time
- capacity
- additional material/consumables
- quality checkpoints

If formal production controls exist, rework may later link to:
- rework work order
- operation logs
- batch cost / yield reporting

---

## 17. Quality Impact
Quality is central to the rework lifecycle.

### Required quality controls
- original defect classification
- rework recommendation
- re-inspection after rework
- final acceptance or failure decision

### Quality analytics support
- rework rate
- recovery rate
- repeat defect rate
- scrap after rework
- source defect patterns

---

## 18. Finance / Cost Impact
Rework may indirectly affect:
- labor and machine cost
- product/batch cost
- variance/loss reporting
- scrap loss if rework fails
- costing analysis for recoverability

Direct finance posting may be limited at first, but linkage should remain possible.

---

## 19. API Impact
Required or future endpoints:
- GET /rework
- POST /rework
- GET /rework/{id}
- POST /rework/{id}/submit
- POST /rework/{id}/approve
- POST /rework/{id}/start
- POST /rework/{id}/mark-reinspection-pending
- POST /rework/{id}/accept
- POST /rework/{id}/reject
- POST /rework/{id}/complete
- POST /rework/{id}/cancel

### Important backend validations
- source validity
- eligible rework qty
- quantity reconciliation
- status transition rules
- role authorization
- accepted vs scrap/rejected output rules

---

## 20. UI / Page Impact
This transaction will power future pages such as:
- Rework List
- Rework Detail
- Rework Execution page
- Re-Inspection page
- Rework reports

### Existing page touchpoints
- Incoming QC may trigger “Send to Rework”
- Stock Overview may reflect rework state
- future production pages may display linked rework references

---

## 21. Audit Requirements
Track at minimum:
- rework creation
- source linkage
- quantity changes
- submit/approve/start actions
- output/result changes
- re-inspection result
- completion/cancellation
- stock movement references

Audit should capture:
- actor
- timestamp
- old/new values for critical fields
- status transition
- source context

---

## 22. Edge Cases
- partial rework success
- repeated rework attempts
- rework fails and becomes scrap
- sales return enters rework path
- incoming QC enters rework path
- batch/serial split during rework
- further rework required after first pass
- wrong source quantity selected
- item moved out of rework location before completion

---

## 23. Reporting Needs
Useful metrics:
- rework count by item/batch
- rework recovery %
- rework failure %
- scrap after rework
- defect reason trend
- cycle time to recover
- repeat failure rate

---

## 24. Completion Criteria
This transaction design is complete when:
- statuses are defined
- quantity reconciliation is defined
- inventory and quality impact are defined
- API implications are identified
- audit and edge cases are documented

---

## 25. Next Files
- page_stock_return_detail.md (future)
- txn_purchase_return.md (future)
- txn_sales_return.md (future)
