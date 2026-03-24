# Page: Stock Transfer

## 1. Page Purpose
The Stock Transfer page is used to move inventory between warehouses, locations, or bins within the organization.

It ensures:
- internal stock movement is controlled and traceable
- stock availability across locations is balanced
- warehouse operations are coordinated
- transfer history is auditable

---

## 2. Module
- Inventory

---

## 3. Recommended Folder Location
`pages/inventory/page_stock_transfer.md`

---

## 4. Page Type
- Transactional Entry + List

Supports:
- create transfer
- track transfer status
- view transfer history

---

## 5. Primary Roles
- Store User
- Store Manager
- Inventory Controller
- Logistics Team

---

## 6. Business Goal
- move stock between locations
- support multi-warehouse operations
- balance inventory
- support production and dispatch needs

---

## 7. Entry Points
- Inventory → Stock Transfer
- Stock Overview → Transfer action
- Low stock / replenishment workflows

---

## 8. Page Structure

### Header
- title: Stock Transfer
- create button
- export (optional)

---

### Section 1: Transfer Form

Fields:
- Transfer ID
- Date
- From Warehouse
- To Warehouse
- From Location
- To Location
- Item
- Batch/Serial (optional)
- Available Qty
- Transfer Qty
- Reason
- Notes

---

### Section 2: Transfer List

Columns:
- Transfer ID
- Date
- From Warehouse
- To Warehouse
- Item
- Qty
- Status
- Created By
- Actions

---

## 9. Status Model
- Draft
- Submitted
- Approved
- In Transit
- Received
- Completed
- Cancelled

---

## 10. Workflow

1. Create transfer
2. Submit (if approval required)
3. Approve transfer
4. Dispatch from source
5. Receive at destination
6. Update stock
7. Close transfer

---

## 11. Inventory Impact

- reduce stock at source
- increase stock at destination
- movement recorded in inventory movement

---

## 12. Validation Rules

- transfer qty ≤ available qty
- source and destination must differ
- cannot transfer blocked/scrap stock
- approval required for large transfers

---

## 13. API Dependencies

- GET /stock-transfers
- POST /stock-transfers
- GET /stock-transfers/{id}
- POST /stock-transfers/{id}/submit
- POST /stock-transfers/{id}/approve
- POST /stock-transfers/{id}/dispatch
- POST /stock-transfers/{id}/receive

---

## 14. UI Behavior

- show transfer direction clearly
- highlight pending transfers
- show in-transit status
- allow tracking

---

## 15. Audit Requirements

- who created
- who approved
- dispatch and receive timestamps
- qty moved
- source/destination

---

## 16. Edge Cases

- partial transfer
- transfer failure
- lost/damaged during transfer
- multi-location transfer

---

## 17. Completion Criteria

- transfer workflow defined
- inventory impact defined
- audit defined

---

## 18. Next Files

- txn_stock_transfer.md
- txn_stock_adjustment.md
