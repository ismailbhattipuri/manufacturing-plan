# Transaction: Scrap

## 1. Business Purpose
Scrap is the transaction used to formally record the disposal or write-off of inventory that is no longer usable, recoverable, or sellable.

This transaction ensures:
- unusable stock is removed from inventory correctly
- scrap is controlled and auditable
- inventory valuation reflects actual usable stock
- loss is traceable and analyzable
- scrap is not done informally

Scrap must always be a controlled transaction.

---

## 2. Source Alignment
This transaction must align with:
- flow_scrap_handling.md
- txn_inventory_movement.md
- txn_qc_inspection.md
- txn_stock_adjustment.md
- page_stock_overview.md
- page_stock_movement.md

---

## 3. Trigger / When Scrap Is Created
Scrap is created when:
- QC rejects material as non-recoverable
- rework fails
- stock is damaged, expired, or obsolete
- production generates scrap
- audit identifies unusable stock

---

## 4. Roles Involved
- Store User
- Store Manager
- QA Inspector
- Production Supervisor
- Finance (view)

---

## 5. Transaction Type
- Category: Inventory Disposal
- Module: Inventory / Quality / Production

---

## 6. Core Outcome
- stock removed from usable inventory
- scrap recorded with reason
- movement logged
- audit trail created

---

## 7. Header Fields

### Identification
- Scrap ID
- Scrap Number
- Date

### Context
- Warehouse
- Location
- Source Document (optional)

### Ownership
- Created By
- Approved By

### Business Fields
- Scrap Reason
- Notes

---

## 8. Line Fields

### Item Info
- Item ID
- Item Code
- Item Name

### Quantity
- Available Qty
- Scrap Qty

### Stock Context
- Batch No
- Serial No
- Stock State

---

## 9. Status Model
- Draft
- Submitted
- Approved
- Rejected
- Completed
- Cancelled

---

## 10. Status Flow
- Draft → Submitted
- Submitted → Approved / Rejected
- Approved → Completed

---

## 11. Quantity Logic
- Scrap Qty ≤ Available Qty
- cannot scrap zero
- partial scrap allowed

---

## 12. Inventory Impact
- reduce stock
- move to scrap ledger
- update stock movement

---

## 13. API Endpoints
- GET /scrap
- POST /scrap
- GET /scrap/{id}
- POST /scrap/{id}/submit
- POST /scrap/{id}/approve
- POST /scrap/{id}/reject
- POST /scrap/{id}/complete

---

## 14. Validation Rules
- qty must be valid
- reason required
- approval required for high value

---

## 15. Audit Requirements
- who created
- who approved
- qty
- reason
- timestamp

---

## 16. Edge Cases
- partial scrap
- repeated scrap
- scrap after rework
- scrap after QC

---

## 17. Completion Criteria
- status defined
- qty logic defined
- inventory impact defined

---

## 18. Next Files
- txn_rework.md
