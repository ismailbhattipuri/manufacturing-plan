# Page: Incoming QC (Quality Control)

## 1. Page Purpose
This page is used by QA Inspectors to review and process Goods Receipt Notes (GRN) that require quality inspection.

It allows:
- viewing QC pending items
- inspecting received goods
- marking items as Accepted or Rejected
- recording QC notes and evidence

---

## 2. Module
- Procurement / Quality

---

## 3. Route
- /procurement/incoming-qc

---

## 4. Entry Points
- GRN Detail → Send to QC
- Dashboard → QC Pending
- Sidebar → Incoming QC

---

## 5. Page Layout

### A. QC List (Table)
Columns:
- GRN Number
- PO Number
- Vendor
- Item
- Received Qty
- QC Status
- Date

---

### B. QC Action Panel
For selected row:

Fields:
- Accepted Qty
- Rejected Qty
- Inspection Notes
- Attach Evidence

---

## 6. Actions
- Accept
- Reject
- Save QC Result

---

## 7. Rules
- Accepted + Rejected = Received
- QC required items must go through this page
- Only QA role can perform QC actions

---

## 8. Output
- Accepted → moves to stock
- Rejected → blocked / return

---

## 9. API Needs
- GET QC pending list
- POST QC result
- GET GRN detail

---

## 10. Notes
- Must be fast for operational use
- Should support bulk QC in future
