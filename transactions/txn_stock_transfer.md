# Transaction: Stock Transfer

## 1. Business Purpose
Stock Transfer is the transaction used to move inventory from one warehouse, location, bin, store, plant, or operational storage point to another within the organization while preserving full inventory traceability and stock control.

This transaction ensures:
- internal movement of stock is formal and auditable
- source and destination inventory are both updated correctly
- transfer-in-transit can be tracked where required
- stock balancing across warehouses/locations is controlled
- production, dispatch, procurement, and warehouse needs can be supported through internal movement

Stock Transfer is a core internal inventory movement transaction and must not be treated as an informal manual quantity change.

---

## 2. Source Alignment
This transaction must remain aligned with:
- Step 2 – Inventory Module
- Step 4 – Inventory / Process Flow Design
- Step 5 – Screen Map
- Step 8 – Backend API
- Step 9 – Frontend Development
- txn_inventory_movement.md
- page_stock_transfer.md
- page_stock_overview.md
- page_stock_movement.md
- role_store_user.md

Future alignment:
- role_store_manager.md
- role_production_planner.md
- role_dispatch_user.md
- page_stock_transfer_detail.md (future if created)

---

## 3. Trigger / When Stock Transfer Is Created
A Stock Transfer may be created when:
- one warehouse needs replenishment from another warehouse
- stock must move between internal locations/bins
- production needs material to be transferred to a production store/location
- dispatch needs stock moved to dispatch staging area
- branch/plant balancing is required
- stock is moved from receiving to storage or from storage to issue point
- operational correction requires controlled internal movement rather than adjustment

Typical triggers:
- low stock at destination warehouse
- manual replenishment request
- transfer planning by inventory controller
- production demand
- dispatch preparation
- stock optimization or balancing initiative

---

## 4. Roles Involved
### Primary Roles
- Store User / Warehouse User
- Store Manager
- Inventory Controller

### Secondary / Related Roles
- Production Planner
- Dispatch User
- Management (approval only if needed)
- Finance (view only for audit/reconciliation if needed)

---

## 5. Role Responsibility Summary
### Store User
- create transfer request
- record item, source, destination, and quantity
- dispatch stock from source
- receive stock at destination if assigned

### Store Manager / Inventory Controller
- approve transfer if policy requires
- monitor in-transit and pending transfers
- handle exceptions and reconciliation

### Production Planner / Dispatch User
- request or trigger transfer demand
- view inbound transfer status for operational planning

---

## 6. Transaction Type
- Category: Internal Inventory Transaction
- Module: Inventory
- Upstream Source: Stock availability / operational demand / replenishment request
- Downstream Target: Inventory movement update at destination

---

## 7. Page Coverage
This transaction is represented across or linked to:
- Stock Transfer page
- Stock Overview
- Stock Movement
- future Transfer Detail page
- future Transfer Approval / Transfer Receipt page

---

## 8. Core Business Outcome
The Stock Transfer ensures:
- inventory is moved internally with control
- source and destination locations remain accurate
- quantity in transit can be tracked
- stock history remains auditable
- downstream operations can rely on internal replenishment status

---

## 9. Transfer Header Fields
Recommended header fields:

### Identification
- Transfer ID
- Transfer Number
- Transfer Date
- Transfer Type

### Source / Destination
- From Warehouse
- To Warehouse
- From Location / Bin
- To Location / Bin
- Branch / Plant context if relevant

### Ownership / Responsibility
- Requested By
- Approved By
- Dispatched By
- Received By

### Business Context
- Reason Code
- Transfer Reason / Notes
- Priority
- Requested Date
- Expected Receipt Date

### Workflow / Status
- Current Status
- Submitted At
- Approved At
- Dispatched At
- Received At
- Cancelled At
- Rejected At
- Rejection Reason

### Audit / System
- Created At
- Created By
- Updated At
- Updated By
- Version No

---

## 10. Transfer Line Item Fields
Each transfer may contain one or more item lines.

### Item Identification
- Line ID
- Item ID
- Item Code
- Item Name / Description
- UOM

### Quantity
- Available Quantity at Source
- Requested Transfer Quantity
- Approved Transfer Quantity
- Dispatched Quantity
- Received Quantity
- Short / Damaged Quantity (if partial transfer issues occur)
- Remaining Quantity (derived)

### Stock Context
- Source Batch No
- Source Serial No
- Source Stock State
- Destination Stock State (if changed)
- From Bin / To Bin

### Control / Notes
- Line Remark
- Special Handling Note
- Quality Condition / Restriction Note if needed

---

## 11. Status Model
Recommended Stock Transfer statuses:

- Draft
- Submitted
- Approved
- Rejected
- In Transit
- Received
- Completed
- Cancelled

### Optional Additional Statuses
- Partially Dispatched
- Partially Received
- On Hold

---

## 12. Status Meaning
### Draft
Transfer is being prepared and not yet submitted.

### Submitted
Transfer request is awaiting approval or operational release.

### Approved
Transfer is approved and ready for dispatch.

### Rejected
Transfer request is not approved.

### In Transit
Stock has left source but not yet been fully received at destination.

### Received
Destination has received the transfer.

### Completed
All inventory impact and reconciliation steps are finished.

### Cancelled
Transfer request or execution was cancelled before completion.

---

## 13. Status Transition Rules
Allowed transitions:

- Draft → Submitted
- Draft → Cancelled
- Submitted → Approved
- Submitted → Rejected
- Approved → In Transit
- In Transit → Received
- Received → Completed
- Approved → Cancelled (restricted)
- In Transit → Cancelled (exceptional, controlled)
- Rejected → Draft (if revise flow exists)

### Guardrails
- cannot dispatch before approval if approval is required
- cannot receive before dispatch
- cannot complete before source and destination inventory effects reconcile
- cannot transfer more than eligible source quantity
- source and destination must not be the same effective stock point unless same-location movement is intentionally allowed

---

## 14. Quantity Logic
This is one of the most important parts of Stock Transfer.

### Base Rules
- Requested Transfer Qty ≤ source eligible available qty
- Approved Transfer Qty ≤ requested qty
- Dispatched Qty ≤ approved qty
- Received Qty ≤ dispatched qty

### Partial Handling
System should support:
- partial dispatch
- partial receipt
- short/damaged in-transit quantity if business tracks it
- multiple receives for one dispatch if needed later

### Reconciliation
For each line:
Dispatched Qty = Received Qty + In-Transit Pending Qty + Lost/Damaged Qty (if tracked)

At simpler level:
Received Qty ≤ Dispatched Qty

---

## 15. Eligibility Rules
Stock Transfer should generally only allow transfer of eligible stock.

### Typically Transferable
- Available stock
- Reserved stock only if policy allows and reservation logic supports movement
- batch/serial-controlled stock with full traceability
- internal QC-approved stock

### Usually Not Transferable Without Special Policy
- scrap stock
- blocked stock
- rejected stock
- QC Hold stock unless transfer is specifically for QC/rework operation
- vendor/subcontractor-owned stock unless modeled explicitly

---

## 16. Inventory Impact
This is the core effect of Stock Transfer.

### Source Impact
- reduce source warehouse/location available stock
- or move source stock into in-transit bucket depending on design

### Destination Impact
- increase destination stock on receipt
- if transfer uses in-transit logic, destination updates only at receive stage

### Recommended Pattern
- On dispatch: source stock decreases, in-transit record created
- On receipt: destination stock increases, in-transit clears

### Important Rule
Source and destination must both be visible in movement history with traceable linkage.

---

## 17. Movement / Ledger Impact
A Stock Transfer should produce inventory movement records such as:
- source outbound movement
- in-transit movement/state (if modeled)
- destination inbound movement

This ensures:
- stock movement ledger remains accurate
- auditability is preserved
- transfer chain is visible in Stock Movement page

---

## 18. Quality / Operational Impact
Stock Transfer may affect:
- production staging
- dispatch readiness
- warehouse balancing
- batch/serial traceability
- quality-controlled stock relocation
- internal replenishment planning

If quality-restricted stock is transferred, policy and reason must be explicit.

---

## 19. Finance / Cost Impact
Stock Transfer is usually an internal inventory movement, but may affect:
- warehouse/location valuation views
- branch/plant internal accounting treatment if multi-entity logic exists
- in-transit stock valuation if tracked

Usually there is no external payable/receivable effect, but internal costing may still care.

---

## 20. Document/Data Structure Guidance
A Stock Transfer transaction should contain:

### Header
- transfer number
- date
- from warehouse/location
- to warehouse/location
- requested by
- approved by
- status
- reason
- expected receipt date

### Line Items
- item
- source qty available
- requested qty
- approved qty
- dispatched qty
- received qty
- source batch/serial
- source/destination location
- notes

---

## 21. UI/Page Impact
This transaction powers pages such as:
- Stock Transfer page
- future Transfer Detail page
- Stock Overview transfer actions
- Stock Movement traceability
- future approval/receipt workflow pages

### Existing page touchpoints
- Stock Overview should show transfer action where allowed
- Stock Movement should show transfer-related movements
- receiving/dispatch operations may link to transfer status

---

## 22. API Impact
This transaction requires endpoints such as:
- list stock transfers
- create stock transfer
- get stock transfer detail
- update draft transfer
- submit stock transfer
- approve stock transfer
- reject stock transfer
- dispatch stock transfer
- receive stock transfer
- cancel stock transfer
- get transferable quantity by source item/location

### Important backend validations
- source qty availability
- source vs destination difference
- stock state eligibility
- status transition rules
- role authorization
- batch/serial consistency

---

## 23. Audit Requirements
Track at minimum:
- transfer creation
- qty changes
- source/destination changes
- approval/rejection
- dispatch confirmation
- receipt confirmation
- cancellation
- movement references generated

Audit should capture:
- actor
- timestamp
- old/new values for critical fields
- status transition
- source and destination context

---

## 24. Exceptions and Edge Cases
- partial transfer
- partial receipt
- lost/damaged quantity in transit
- urgent transfer bypassing approval (if policy exists)
- same item transferred in multiple batches
- batch/serial mismatch at receipt
- transfer cancelled after partial dispatch
- destination rejects transfer receipt
- source stock changes after draft creation

---

## 25. Reporting Needs
Useful metrics:
- transfer count by warehouse
- transfer volume by item
- in-transit aging
- transfer delays
- partial transfer frequency
- replenishment trend between warehouses
- stock balancing efficiency
- transfer exception count

---

## 26. Completion Criteria
This transaction design is complete when:
- header and line structure are defined
- statuses are defined
- quantity logic is defined
- source/destination inventory impact is defined
- movement/ledger impact is defined
- UI/API implications are identified
- audit and edge cases are documented

---

## 27. Next Related Files
Recommended next files:
1. txn_stock_adjustment.md
2. page_stock_transfer_detail.md (future)
3. page_stock_adjustment_detail.md (future)
4. transactions/txn_scrap.md (future)
