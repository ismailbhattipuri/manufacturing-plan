# Page: Stock Adjustment

## 1. Page Purpose
The Stock Adjustment page is used to correct inventory discrepancies by increasing or decreasing stock quantities due to physical count differences, damage, system mismatch, expiry, or audit corrections.

This page ensures:
- inventory corrections are controlled and auditable
- adjustments are properly recorded with reasons
- stock discrepancies are resolved systematically
- financial and operational impact is traceable

---

## 2. Module
- Inventory

---

## 3. Recommended Folder Location
`pages/inventory/page_stock_adjustment.md`

---

## 4. Page Type
- Transactional Entry + List/History

Supports:
- create adjustment
- view adjustment history
- track adjustment impact

---

## 5. Primary Roles
- Store User
- Store Manager
- Inventory Controller
- Finance (view)

---

## 6. Business Goal
- fix stock mismatch
- correct system vs physical stock
- handle damage/expiry/manual corrections
- maintain audit trail

---

## 7. Entry Points
- Inventory → Stock Adjustment
- Stock Overview discrepancy
- Audit workflows

---

## 8. Page Structure

### Header
- title: Stock Adjustment
- create button
- export (optional)

---

### Section 1: Adjustment Form (Create/Edit)

Fields:
- Adjustment ID
- Date
- Warehouse
- Item
- Batch/Serial (optional)
- Current System Qty
- Physical Qty
- Adjustment Qty (auto-calculated or manual)
- Adjustment Type (Increase / Decrease)
- Reason Code
- Notes
- Attachments (optional)

---

### Section 2: Adjustment List / History

Columns:
- Adjustment ID
- Date
- Item
- Warehouse
- Adjustment Type
- Qty Change (+/-)
- Reason
- Created By
- Status
- Actions

---

## 9. Status Model
- Draft
- Submitted
- Approved
- Rejected
- Completed
- Cancelled

---

## 10. Workflow

1. Create adjustment
2. Submit for approval (if required)
3. Approve/reject
4. Apply adjustment → stock updated
5. Record audit

---

## 11. Inventory Impact

- Increase → stock added
- Decrease → stock reduced
- must reflect in stock overview
- linked to inventory movement

---

## 12. Validation Rules

- physical qty must be entered
- adjustment qty must match logic
- cannot adjust non-existing stock (unless allowed)
- approval required for high value adjustments

---

## 13. API Dependencies

- GET /inventory-adjustments
- POST /inventory-adjustments
- GET /inventory-adjustments/{id}
- POST /inventory-adjustments/{id}/submit
- POST /inventory-adjustments/{id}/approve
- POST /inventory-adjustments/{id}/reject

---

## 14. UI Behavior

- highlight negative adjustments
- show before/after qty
- show reason clearly
- approval indicator

---

## 15. Audit Requirements

- who created
- who approved
- qty before/after
- reason
- timestamp

---

## 16. Edge Cases

- zero adjustment
- negative stock correction
- multiple adjustments on same item
- batch-specific corrections

---

## 17. Completion Criteria

- adjustment logic defined
- approval flow defined
- stock impact defined
- audit captured

---

## 18. Next Files

- page_stock_transfer.md
- txn_stock_adjustment.md
