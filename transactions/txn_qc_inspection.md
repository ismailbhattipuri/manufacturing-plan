# Transaction: QC Inspection

## 1. Business Purpose
QC Inspection is the transaction used to record quality verification results for materials, semi-finished goods, finished goods, or returned items that require formal inspection before they can move to the next allowed state.

This transaction ensures:
- quality decisions are recorded in a structured and auditable way
- accepted, rejected, reworkable, and scrap outcomes are clearly classified
- inspection results are linked to source documents such as GRN, production output, or returns
- inventory and workflow transitions depend on recorded quality disposition, not informal manual decisions
- quality performance and defect analytics can be measured over time

QC Inspection is the control transaction that formalizes pass/fail/disposition logic inside the ERP.

---

## 2. Source Alignment
This transaction must remain aligned with:
- Step 2 – Quality Module
- Step 4 – Quality Flow
- Step 4 – Procurement Flow
- Step 4 – Production Flow
- Step 5 – Screen Map
- Step 8 – Backend API
- Step 9 – Frontend Development
- txn_goods_receipt_note.md
- txn_inventory_movement.md
- flow_rework.md
- flow_scrap_handling.md
- flow_purchase_return.md
- flow_sales_return.md
- page_incoming_qc.md
- page_grn_detail.md
- role_qa_inspector.md

---

## 3. Trigger / When QC Inspection Is Created
A QC Inspection may be created when:
- incoming material is received and flagged QC-required
- in-process production output requires inspection
- finished goods require final quality approval
- returned goods require condition assessment
- reworked goods require re-inspection
- warehouse or production flags a questionable item for controlled review

Typical triggers:
- GRN sent to QC
- production output moved to QC
- sales return received
- rework completed and awaiting disposition
- manual quality hold process initiated

---

## 4. Roles Involved
### Primary Roles
- QA Inspector
- QA Manager (future extension if multi-level QC approval is needed)

### Secondary / Related Roles
- Store User / Warehouse User
- Production Supervisor
- Procurement Executive
- Procurement Manager
- Sales / Service team (returns context)
- Finance / Management (view/report only if required)

---

## 5. Role Responsibility Summary
### QA Inspector
- perform inspection
- record findings
- classify accepted/rejected/rework/scrap quantities
- attach evidence
- submit final quality disposition

### QA Manager (future)
- review escalated or sensitive inspections
- approve conditional acceptance
- handle repeated or high-value failures

### Store / Warehouse User
- provide physical item/batch for inspection
- consume inspection result operationally
- move stock according to disposition

### Production Supervisor
- consume inspection result for rework or scrap decisions in production-related scenarios

---

## 6. Transaction Type
- Category: Control / Quality Transaction
- Module: Quality
- Upstream Source: GRN / Production Output / Sales Return / Rework / Manual Hold
- Downstream Target: Accepted stock / Rework / Scrap / Return / next workflow state

---

## 7. Page Coverage
This transaction is represented across or linked to:
- Incoming QC page
- GRN Detail (QC section)
- future In-Process QC page
- future Final QC page
- future QC Inspection Detail page
- QC Reports

---

## 8. Core Business Outcome
The QC Inspection ensures:
- the organization does not rely on informal pass/fail decisions
- stock state changes depend on recorded quality disposition
- rejected and non-conforming materials are classified properly
- rework/scrap/return paths can be triggered with traceable evidence
- quality data becomes usable for analysis and vendor/production improvement

---

## 9. Source Context Fields
Every QC Inspection should clearly capture source context.

### Recommended Source Fields
- QC Inspection ID
- QC Number / Inspection No
- Inspection Type
- Source Module
- Source Document ID
- Source Document Number
- Source Line ID (if line-level inspection)
- Item ID
- Item Code
- Item Name / Description
- Batch No / Lot No
- Serial No (if applicable)
- Warehouse / Location
- Vendor / Customer / Work Order reference if relevant

---

## 10. Inspection Header Fields
Recommended header fields:

### Identification
- QC ID
- QC Number
- Inspection Date
- Inspection Type
- Inspection Status

### Ownership / Responsibility
- Inspected By User ID
- Assigned QA User / Team
- Branch / Plant / Warehouse
- Inspection Stage (Incoming / In-Process / Final / Return / Rework)

### Source Linkage
- Source Module
- Source Document ID
- Source Document Number
- Source Transaction Date
- Source Line Reference
- Source Qty

### Business Context
- Inspection Standard / Template (optional)
- Reason for Inspection
- Priority
- Due Date / SLA (optional)

### Audit / System
- Created At
- Created By
- Updated At
- Updated By
- Version No

---

## 11. Inspection Result / Line Fields
If inspection is line-level or quantity-based, capture:

### Quantity Structure
- Inspected Quantity
- Accepted Quantity
- Rejected Quantity
- Rework Quantity
- Scrap Quantity
- Conditional Acceptance Quantity (optional)
- Pending / Hold Quantity (optional)

### Result Fields
- Inspection Result
- Defect Category
- Defect Severity
- Defect Description
- Observation Notes
- Rejection Reason
- Rework Recommendation
- Scrap Recommendation
- Return Recommendation

### Evidence / Control
- Attachment Indicator
- Image / file references
- Checklist result references
- Measured values / parameters if needed

---

## 12. Status Model
Recommended QC Inspection statuses:

- Pending
- In Inspection
- Completed
- Accepted
- Rejected
- Partially Accepted
- Rework Recommended
- Scrap Recommended
- Cancelled

### Optional Additional Statuses
- Escalated
- Awaiting QA Manager Review
- On Hold
- Conditional Acceptance

---

## 13. Status Meaning
### Pending
Inspection exists but not yet started.

### In Inspection
Inspection is being performed.

### Completed
Findings recorded, but final mapped disposition may still be in process depending on design.

### Accepted
Quantity passed inspection and can move to the next allowed state.

### Rejected
Quantity failed inspection and is not acceptable.

### Partially Accepted
Some quantity passed, some failed.

### Rework Recommended
Inspection result indicates rework path should be used.

### Scrap Recommended
Inspection result indicates scrap path should be used.

### Cancelled
Inspection was invalidated or withdrawn before completion.

---

## 14. Status Transition Rules
Allowed transitions:

- Pending → In Inspection
- In Inspection → Completed
- Completed → Accepted
- Completed → Rejected
- Completed → Partially Accepted
- Completed → Rework Recommended
- Completed → Scrap Recommended
- Pending → Cancelled
- In Inspection → Cancelled (restricted)

### Guardrails
- cannot finalize result without inspected quantity
- cannot exceed source quantity
- accepted/rejected/rework/scrap split must reconcile with inspected quantity
- cannot move accepted quantity into available stock before QC is completed
- cannot recommend rework/scrap without reason coding

---

## 15. Quantity Logic
This is one of the most important parts of QC Inspection.

### Base Rule
All outcome quantities must reconcile with inspected quantity.

### Example
Inspected Qty = 100  
Accepted = 70  
Rejected = 10  
Rework = 15  
Scrap = 5  

System must validate:
70 + 10 + 15 + 5 = 100

### Additional Rules
- outcome qty must not exceed source qty
- multiple inspections against same source should track already inspected qty if partial inspection is supported
- partial acceptance must be allowed
- rework and scrap quantities should not appear as accepted stock

---

## 16. Disposition Logic
QC Inspection may result in multiple allowed outcomes:

### Accepted
- move to usable/available stock
- release to next workflow step

### Rejected
- keep blocked / initiate return / hold
- depends on source context

### Rework Recommended
- trigger rework flow
- move to rework stock/state

### Scrap Recommended
- trigger scrap flow
- move to scrap-hold/scrap state

### Return Recommended
- where incoming/vendor/customer context supports it
- link to purchase return or sales return flow

---

## 17. Inventory Impact
QC Inspection itself may or may not directly post stock depending on system design, but it must control stock disposition.

### Typical effects
- Accepted qty → Available stock or next process
- Rejected qty → Blocked / Rejected stock
- Rework qty → Rework stock
- Scrap qty → Scrap hold / scrap flow

### Important Rule
QC Inspection should determine stock classification, even if the stock movement posting is executed by another transaction/service.

---

## 18. Production Impact
When source is production:
- accepted quantity may move to finished/semi-finished usable stock
- rejected quantity may create rework/scrap demand
- production yield and variance reporting should capture QC outcome
- batch-level quality traceability should be preserved

---

## 19. Procurement / Return Impact
When source is incoming procurement receipt:
- accepted quantity supports GRN acceptance
- rejected quantity may support purchase return
- vendor quality performance should capture rejection trends
- receiving stock should respect QC result

When source is sales return:
- accepted quantity may move back to stock
- rejected quantity may move to rework or scrap

---

## 20. Finance / Cost Impact
QC Inspection may indirectly affect:
- inventory valuation state
- rework cost path
- scrap loss recognition
- return/credit processing
- vendor claim analysis

Direct financial posting may not occur inside QC itself, but the linkage matters.

---

## 21. Document/Data Structure Guidance
A QC Inspection transaction should contain:

### Header
- qc id / number
- source type/document
- inspection stage
- item/batch reference
- inspector
- status
- date/time
- source qty
- notes

### Result / Line Detail
- inspected qty
- accepted qty
- rejected qty
- rework qty
- scrap qty
- defect category
- observation
- evidence
- disposition notes

---

## 22. UI/Page Impact
This transaction will power pages such as:
- Incoming QC page
- future In-Process QC page
- future Final QC page
- QC Inspection Detail page
- QC Reports

### Existing page touchpoints
- GRN Detail should show linked QC result
- Incoming QC should act as queue/action page for QC inspection execution
- Stock Overview should reflect QC result states indirectly
- linked rework/scrap/return references should appear where disposition exists

---

## 23. API Impact
This transaction will require endpoints such as:
- list QC inspections or QC queue
- create QC inspection
- get QC inspection detail
- start inspection
- submit result
- accept inspection
- reject inspection
- mark rework recommended
- mark scrap recommended
- cancel inspection
- get source context by source document/id
- upload QC evidence
- get QC audit trail

### Important backend validations
- source document validity
- source qty validity
- quantity reconciliation
- status transition rules
- role authorization
- disposition-specific rule validation

---

## 24. Audit Requirements
Track at minimum:
- inspection creation
- start inspection
- quantity/result changes
- defect/reason updates
- acceptance/rejection/rework/scrap recommendation
- evidence upload
- cancellation
- linked stock/state change references

Audit should capture:
- actor
- timestamp
- old/new values for critical fields
- source reference
- status transition

---

## 25. Exceptions and Edge Cases
- partial acceptance
- multiple inspections on same source line
- failed rework re-inspection
- no-defect but conditional hold case
- repeated vendor defect on same item
- source qty changed after inspection creation
- batch/serial-level mixed result
- urgent override/escalation scenario
- inspection cancelled after stock already moved (should require correction flow)

---

## 26. Reporting Needs
Useful metrics:
- acceptance vs rejection %
- defect reason trends
- vendor quality performance
- production quality yield
- QC turnaround time
- rework recommendation rate
- scrap recommendation rate
- QC backlog aging

---

## 27. Completion Criteria
This transaction design is complete when:
- source context is defined
- statuses are defined
- quantity logic is defined
- disposition outcomes are defined
- inventory/production/procurement impacts are defined
- UI/API implications are identified
- audit and edge cases are documented

---

## 28. Next Related Files
Recommended next files:
1. page_stock_movement.md
2. page_stock_adjustment.md
3. page_stock_transfer.md
4. transactions/txn_stock_transfer.md
5. transactions/txn_stock_adjustment.md
