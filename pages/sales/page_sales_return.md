# Page: Sales Return

## 1. Page Purpose
The Sales Return page is used to manage and process goods returned by customers after delivery.

It allows users to:
- create sales return records
- track return approval and receipt status
- link returns to original Sales Order and Invoice
- manage return quantities and reasons
- coordinate QC, inventory, and finance impact

---

## 2. Module
- Sales / Inventory / Finance

---

## 3. Recommended Folder Location
`pages/sales/page_sales_return.md`

---

## 4. Page Type
- Transactional + List + Detail

---

## 5. Primary Roles
- Sales Executive
- Dispatch User
- Store User
- QA Inspector
- Finance Officer

---

## 6. Entry Points
- Delivery / Invoice Detail → Create Return
- Customer complaint workflow
- Sales → Sales Return

---

## 7. Page Structure

### Header
- title: Sales Return
- create button
- export (optional)

---

### Section 1: Return Form

Fields:
- Return ID
- Date
- Customer
- Source Sales Order
- Source Invoice
- Item
- Batch/Serial (optional)
- Delivered Qty
- Already Returned Qty
- Return Qty
- Reason
- Notes

---

### Section 2: Return List

Columns:
- Return ID
- Date
- Customer
- Sales Order
- Invoice
- Item
- Qty
- Status
- Created By
- Actions

---

## 8. Status Model
- Draft
- Submitted
- Approved
- Rejected
- Received
- Completed
- Closed
- Cancelled

---

## 9. Workflow
1. Create return
2. Submit
3. Approve/reject
4. Receive returned goods
5. QC classify if needed
6. Complete inventory + finance impact
7. Close

---

## 10. Inventory Impact
- add stock back if reusable
- or move to QC / rework / scrap state
- update inventory movement

---

## 11. QC Impact
- inspection may be required
- classify as:
  - usable
  - rework
  - scrap

---

## 12. Finance Impact
- create credit note
- refund or adjust customer balance
- invoice linkage

---

## 13. Validation Rules
- source SO/Invoice required
- qty ≤ eligible qty
- reason required
- partial return allowed

---

## 14. API Dependencies
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

## 15. UI Behavior
- highlight pending receipt / QC
- show source SO + invoice clearly
- show qty tracking and customer reason
- show inventory/finance status

---

## 16. Audit
- user actions
- qty changes
- status changes
- finance completion

---

## 17. Edge Cases
- partial return
- repeated return
- damaged return
- no-invoice exception
- QC rejection after customer return

---

## 18. Completion Criteria
- workflow defined
- inventory linked
- finance linked
- audit defined

---

## 19. Next Files
- txn_production_plan.md
