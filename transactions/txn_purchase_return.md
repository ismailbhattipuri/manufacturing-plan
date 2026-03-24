# Transaction: Purchase Return

## 1. Business Purpose
Purchase Return is the transaction used to formally return goods to a vendor after receipt due to quality issues, damage, incorrect supply, excess quantity, or other business-approved reasons.

This transaction ensures:
- returned goods are traceable to original PO and GRN
- inventory is correctly reduced or moved
- vendor return is documented and auditable
- financial adjustment (debit note / credit) can be aligned
- quality and procurement issues are recorded for analysis

Purchase Return must not be handled as informal stock reduction.

---

## 2. Source Alignment
This transaction must align with:
- flow_purchase_return.md
- txn_goods_receipt_note.md
- txn_inventory_movement.md
- txn_qc_inspection.md
- page_grn_detail.md
- page_stock_overview.md

---

## 3. Trigger / When Purchase Return Is Created
A Purchase Return may be created when:
- QC rejects material
- goods are damaged on receipt
- wrong item delivered
- excess quantity needs to be returned
- post-receipt issue is identified before consumption

---

## 4. Roles Involved
- Store User
- QA Inspector
- Procurement Executive
- Procurement Manager
- Finance (view)

---

## 5. Transaction Type
- Category: Procurement Exception
- Module: Procurement / Inventory

---

## 6. Core Outcome
- stock reduced or moved to return state
- return linked to PO/GRN
- vendor notified
- audit trail created

---

## 7. Header Fields

### Identification
- Return ID
- Return Number
- Return Date

### Source
- PO Number
- GRN Number
- Vendor

### Ownership
- Created By
- Approved By

### Business Fields
- Return Reason
- Notes

---

## 8. Line Fields

### Item Info
- Item ID
- Item Code
- Item Name

### Quantity
- Received Qty
- Already Returned Qty
- Return Qty

### Stock Context
- Batch No
- Serial No

---

## 9. Status Model
- Draft
- Submitted
- Approved
- Rejected
- Returned
- Closed
- Cancelled

---

## 10. Status Flow
- Draft → Submitted
- Submitted → Approved / Rejected
- Approved → Returned
- Returned → Closed

---

## 11. Quantity Logic
- Return Qty ≤ (Received Qty - Already Returned Qty)
- partial return allowed
- multiple returns allowed

---

## 12. Inventory Impact
- reduce stock
- or move to return state
- update inventory movement

---

## 13. API Endpoints
- GET /purchase-returns
- POST /purchase-returns
- GET /purchase-returns/{id}
- POST /purchase-returns/{id}/submit
- POST /purchase-returns/{id}/approve
- POST /purchase-returns/{id}/reject
- POST /purchase-returns/{id}/return
- POST /purchase-returns/{id}/close

---

## 14. Validation Rules
- source PO/GRN required
- qty must be valid
- reason required
- cannot exceed eligible qty

---

## 15. Audit Requirements
- who created
- who approved
- qty
- reason
- timestamp

---

## 16. Edge Cases
- partial return
- repeated return
- return after QC reject
- return after partial usage

---

## 17. Completion Criteria
- status defined
- qty logic defined
- inventory impact defined

---

## 18. Next Files
- txn_sales_return.md
