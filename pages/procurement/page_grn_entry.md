# Page: GRN Entry

## 1. Page Purpose
The GRN Entry page is used to record the receipt of goods against an approved Purchase Order (PO). It allows warehouse/store users to:
- capture received quantities
- assign warehouse/bin locations
- record batch/serial details if required
- note discrepancies
- initiate QC process if required

This page is the operational entry point for inbound inventory.

---

## 2. Source Alignment
Aligned with:
- txn_goods_receipt_note.md
- txn_purchase_order.md
- page_po_detail.md
- Step 4 procurement flow
- Step 9 frontend standards

---

## 3. Module
- Procurement / Inventory

---

## 4. Page Type
- Transaction Entry Page (Form + Line Item Grid)

---

## 5. Primary Roles
- Store User / Warehouse User
- Store Manager (review)
- QA Team (QC follow-up)

---

## 6. Entry Points
- PO Detail → “Receive Goods”
- Sidebar → GRN Entry
- Inbound queue/dashboard
- Notification

---

## 7. Route
- `/procurement/grn/new?source_po={id}`

---

## 8. Page Header
- Title: Create GRN
- Source PO reference
- Vendor name
- Expected delivery

---

## 9. Source PO Reference Block
Show:
- PO Number
- Vendor
- Ordered items summary
- Remaining quantity
- Quick link to PO

---

## 10. Form Sections

### A. Header Section
Fields:
- GRN Date
- Warehouse
- Location/Bin
- Delivery Note Number
- Received By
- Remarks

---

### B. Line Item Grid
Columns:
- Item
- Ordered Qty
- Received Qty (input)
- Accepted Qty
- Rejected Qty
- Remaining Qty
- Batch / Serial
- QC Required
- Remarks

---

## 11. Actions
- Save Draft
- Mark as Received
- Send to QC
- Cancel

---

## 12. Status Rules
- Draft → editable
- Received → locked
- QC Pending → QC action
- Accepted → stock update
- Rejected → blocked

---

## 13. Validation Rules
- PO must be approved
- Received Qty > 0
- Cannot exceed remaining PO qty
- Accepted + Rejected = Received
- Warehouse required

---

## 14. Role Behavior

### Store User
- enter quantities
- mark received

### QA
- accept/reject

### Manager
- monitor exceptions

---

## 15. UI Behavior
- line items pre-filled from PO
- editable qty fields
- validation inline
- highlight discrepancies
- sticky action bar

---

## 16. Data Requirements
- PO line items
- remaining qty
- warehouse list
- item master

---

## 17. API Dependencies
- GET PO details
- POST create GRN
- PATCH update GRN
- POST mark received
- POST accept/reject

---

## 18. States
- Loading
- Empty (no receivable items)
- Error
- No access

---

## 19. Notes
- Must prevent over-receipt
- Must support partial receipt
- Must connect to QC

---

## 20. Next
- page_grn_detail.md
