# Transaction: Inventory Movement

## 1. Purpose
Defines all stock changes in the system. Every stock increase or decrease must be recorded as an inventory movement.

---

## 2. Source Alignment
- GRN (stock in)
- QC (accept/reject)
- Future: transfer, adjustment, sales

---

## 3. Movement Types
- GRN Receipt (IN)
- QC Accepted (IN → Available)
- QC Rejected (BLOCKED)
- Adjustment (future)
- Transfer (future)

---

## 4. Core Fields

### Header
- Movement ID
- Movement Type
- Source Document (GRN / PO)
- Date
- Warehouse

### Line
- Item
- Quantity
- From Location
- To Location
- Stock Type (Available / QC / Rejected)

---

## 5. Rules
- Every GRN Accepted → Inventory IN
- Rejected → not available stock
- No negative stock unless allowed

---

## 6. API Needs
- GET movements
- POST movement
- GET movement detail

---

## 7. Notes
- Must be audit-safe
- Must link to source documents
