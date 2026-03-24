# Transaction: Stock Adjustment

## 1. Business Purpose
Stock Adjustment is the transaction used to correct inventory discrepancies between system-recorded stock and physically observed stock.

This transaction ensures:
- inventory accuracy is maintained
- discrepancies are formally recorded and justified
- stock corrections are controlled and auditable
- financial and operational impact is traceable
- adjustment reasons are analyzed for process improvement

Stock Adjustment must never be used casually; it is a controlled correction mechanism.

---

## 2. Source Alignment
This transaction must remain aligned with:
- Step 2 – Inventory Module
- Step 4 – Inventory / Process Flow Design
- Step 8 – Backend API
- Step 9 – Frontend Development
- txn_inventory_movement.md
- page_stock_adjustment.md
- page_stock_overview.md
- page_stock_movement.md
- role_store_user.md

---

## 3. Trigger / When Stock Adjustment Is Created
A Stock Adjustment may be created when:
- physical stock count differs from system stock
- damage, expiry, or shrinkage is identified
- audit findings require correction
- system error or incorrect entry needs correction
- stock reconciliation is performed
- warehouse discrepancy is reported

---

## 4. Roles Involved
### Primary Roles
- Store User
- Store Manager
- Inventory Controller

### Secondary Roles
- Finance Officer
- Management (approval for high-value adjustments)

---

## 5. Role Responsibility Summary
### Store User
- identify discrepancy
- create adjustment record
- input physical quantity and reason

### Store Manager / Controller
- review and approve adjustment
- validate reason and quantity

### Finance Officer
- track financial impact
- audit adjustment trends

---

## 6. Transaction Type
- Category: Inventory Correction Transaction
- Module: Inventory
- Upstream Source: Physical count / discrepancy
- Downstream Target: Inventory movement correction

---

## 7. Core Business Outcome
Stock Adjustment ensures:
- system stock reflects actual stock
- discrepancies are explained and traceable
- inventory ledger remains consistent
- audit trail exists for every correction

---

## 8. Header Fields

### Identification
- Adjustment ID
- Adjustment Number
- Adjustment Date

### Context
- Warehouse
- Location / Bin
- Branch / Plant

### Ownership
- Created By
- Approved By

### Business Fields
- Adjustment Type (Increase / Decrease)
- Reason Code
- Notes

### Audit Fields
- Created At
- Updated At

---

## 9. Line Item Fields

### Item Info
- Item ID
- Item Code
- Item Name
- UOM

### Quantity Fields
- System Quantity
- Physical Quantity
- Adjustment Quantity (calculated = Physical - System)

### Stock Context
- Batch No
- Serial No
- Stock State

### Control
- Reason
- Notes

---

## 10. Status Model
- Draft
- Submitted
- Approved
- Rejected
- Completed
- Cancelled

---

## 11. Status Meaning

### Draft
Adjustment being prepared

### Submitted
Awaiting approval

### Approved
Approved for execution

### Rejected
Rejected adjustment

### Completed
Stock updated

### Cancelled
Withdrawn

---

## 12. Status Transition Rules

- Draft → Submitted
- Submitted → Approved
- Submitted → Rejected
- Approved → Completed
- Draft → Cancelled
- Approved → Cancelled (restricted)

### Guardrails
- cannot complete without approval (if required)
- cannot adjust without qty difference
- cannot adjust invalid item/location

---

## 13. Quantity Logic

### Formula
Adjustment Qty = Physical Qty - System Qty

### Rules
- positive → stock increase
- negative → stock decrease
- zero adjustment should be blocked or flagged

---

## 14. Eligibility Rules

Allowed:
- available stock
- batch/serial stock

Restricted:
- scrap stock
- blocked stock (unless correction flow)

---

## 15. Inventory Impact

### Increase
- stock increases at location

### Decrease
- stock reduces

### Movement Record
- must generate inventory movement record
- appear in stock movement ledger

---

## 16. Financial Impact

- stock valuation increase/decrease
- loss/gain recording
- audit impact

---

## 17. API Impact

Required endpoints:
- GET /inventory-adjustments
- POST /inventory-adjustments
- GET /inventory-adjustments/{id}
- POST /inventory-adjustments/{id}/submit
- POST /inventory-adjustments/{id}/approve
- POST /inventory-adjustments/{id}/reject
- POST /inventory-adjustments/{id}/complete

---

## 18. Validation Rules

- physical qty required
- system qty must be known
- reason mandatory
- approval required for large adjustments
- cannot exceed logical limits (configurable)

---

## 19. Audit Requirements

Track:
- who created
- who approved
- before/after qty
- adjustment qty
- reason
- timestamp

---

## 20. Edge Cases

- zero adjustment
- negative stock correction
- repeated adjustment
- batch-level correction
- incorrect adjustment reversal

---

## 21. Reporting Needs

- adjustment frequency
- adjustment value
- reason analysis
- warehouse discrepancy trend

---

## 22. Completion Criteria

- quantity logic defined
- status model defined
- inventory impact defined
- audit defined

---

## 23. Next Files

- txn_scrap.md
- txn_rework.md
