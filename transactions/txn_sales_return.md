# Transaction: Sales Return

## 1. Business Purpose
Sales Return is the transaction used to formally process goods returned by a customer after delivery due to defects, damage, incorrect supply, excess quantity, or other valid business reasons.

This transaction ensures:
- returned goods are linked to original sales order and invoice
- inventory is updated correctly upon return
- customer return is controlled and auditable
- financial adjustments (credit note/refund) are aligned
- quality issues are captured for analysis

Sales Return must not be handled as informal stock addition.

---

## 2. Source Alignment
This transaction must align with:
- flow_sales_return.md
- txn_inventory_movement.md
- txn_qc_inspection.md
- page_stock_overview.md
- page_stock_movement.md

---

## 3. Trigger / When Sales Return Is Created
A Sales Return may be created when:
- customer reports defective goods
- wrong item delivered
- excess quantity delivered
- goods rejected at delivery
- return under warranty/policy

---

## 4. Roles Involved
- Sales Executive
- Dispatch User
- Store User
- QA Inspector
- Finance Officer

---

## 5. Transaction Type
- Category: Sales Exception
- Module: Sales / Inventory / Finance

---

## 6. Core Outcome
- stock re-enters system (if usable)
- return linked to SO/invoice
- QC classification applied if needed
- financial adjustment initiated
- audit trail created

---

## 7. Header Fields

### Identification
- Return ID
- Return Number
- Return Date

### Source
- Sales Order Number
- Invoice Number
- Customer

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
- Delivered Qty
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
- Received
- Completed
- Closed
- Cancelled

---

## 10. Status Flow
- Draft → Submitted
- Submitted → Approved / Rejected
- Approved → Received
- Received → Completed
- Completed → Closed

---

## 11. Quantity Logic
- Return Qty ≤ (Delivered Qty - Already Returned Qty)
- partial return allowed
- multiple returns allowed

---

## 12. Inventory Impact
- increase stock (if reusable)
- or move to QC/rework/scrap state
- update inventory movement

---

## 13. QC Impact
- inspection may be required
- classify returned goods:
  - usable
  - rework
  - scrap

---

## 14. Finance Impact
- credit note
- refund
- invoice adjustment

---

## 15. API Endpoints
- GET /sales-returns
- POST /sales-returns
- GET /sales-returns/{id}
- POST /sales-returns/{id}/submit
- POST /sales-returns/{id}/approve
- POST /sales-returns/{id}/reject
- POST /sales-returns/{id}/receive
- POST /sales-returns/{id}/complete
- POST /sales-returns/{id}/close

---

## 16. Validation Rules
- source SO/invoice required
- qty must be valid
- reason required
- cannot exceed eligible qty

---

## 17. Audit Requirements
- who created
- who approved
- qty
- reason
- timestamp

---

## 18. Edge Cases
- partial return
- repeated return
- damaged return
- expired return
- no-invoice return (exception)

---

## 19. Completion Criteria
- status defined
- qty logic defined
- inventory impact defined
- finance linkage defined

---

## 20. Next Files
- page_sales_return.md
