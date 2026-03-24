# Page: Purchase Return

## 1. Page Purpose
The Purchase Return page is used to manage and process returns of goods back to vendors after receipt.

It allows users to:
- create purchase return records
- track return status
- manage return quantities
- link returns to PO and GRN
- ensure inventory and vendor reconciliation

---

## 2. Module
- Procurement / Inventory

---

## 3. Recommended Folder Location
`pages/procurement/page_purchase_return.md`

---

## 4. Page Type
- Transactional + List + Detail

---

## 5. Primary Roles
- Store User
- QA Inspector
- Procurement Executive
- Procurement Manager

---

## 6. Entry Points
- GRN Detail → Create Return
- QC Rejection → Return
- Procurement → Purchase Return

---

## 7. Page Structure

### Header
- title: Purchase Return
- create button
- export (optional)

---

### Section 1: Return Form

Fields:
- Return ID
- Date
- Vendor
- Source PO
- Source GRN
- Item
- Batch/Serial (optional)
- Received Qty
- Already Returned Qty
- Return Qty
- Reason
- Notes

---

### Section 2: Return List

Columns:
- Return ID
- Date
- Vendor
- PO
- GRN
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
- Returned
- Closed
- Cancelled

---

## 9. Workflow
1. Create return
2. Submit
3. Approve/reject
4. Process return
5. Close

---

## 10. Inventory Impact
- reduce stock
- update movement

---

## 11. Validation Rules
- source PO/GRN required
- qty ≤ available
- reason required

---

## 12. API Dependencies
- GET /purchase-returns
- POST /purchase-returns
- POST /purchase-returns/{id}/submit
- POST /purchase-returns/{id}/approve
- POST /purchase-returns/{id}/return

---

## 13. UI Behavior
- highlight rejected/returned
- show linkage to GRN
- show qty tracking

---

## 14. Audit
- user actions
- qty changes
- status changes

---

## 15. Edge Cases
- partial return
- repeated return
- return after QC reject

---

## 16. Completion Criteria
- workflow defined
- inventory linked
- audit defined

---

## 17. Next Files
- page_sales_return.md
