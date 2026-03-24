# Page: PO List

## 1. Page Purpose
The PO List page is the main operational listing screen for Purchase Orders (PO). It allows users to:
- view all purchase orders
- monitor approval and fulfillment status
- track vendor commitments
- identify pending approvals and receipts
- navigate to PO detail pages
- analyze procurement activity

This page is the primary entry point into the Purchase Order workflow.

---

## 2. Source Alignment
Aligned with:
- txn_purchase_order.md
- role_procurement_executive.md
- role_procurement_manager.md
- Step 4 procurement flow (PR → PO → GRN)
- Step 9 frontend structure

---

## 3. Module
- Procurement

---

## 4. Page Type
- Standard ERP List Page

---

## 5. Primary Roles
- Procurement Executive
- Procurement Manager
- Store/Warehouse (view)
- Finance (view/report)

---

## 6. Entry Points
- Sidebar → Procurement → PO List
- PR Detail → Convert to PO → redirect
- Dashboard widgets
- Global search
- Notifications

---

## 7. Route
- `/procurement/purchase-orders`

---

## 8. Page Header
Actions:
- Create PO (if allowed)
- Export
- Refresh

---

## 9. KPI Summary Strip
- Total POs
- Draft
- Submitted
- Approved
- Partially Received
- Closed

---

## 10. Filters & Search
Search:
- PO Number
- Vendor
- PR Reference

Filters:
- Status
- Date range
- Vendor
- Buyer
- Branch
- Delivery status

---

## 11. Table Columns
- PO Number
- PO Date
- Vendor
- Source PR
- Buyer
- Status
- Total Amount
- Expected Delivery
- Received %
- Last Updated
- Actions

---

## 12. Row Actions

### Procurement Executive
- Draft → Edit, Submit, Cancel
- Approved → View, Track GRN

### Procurement Manager
- Submitted → Approve, Reject

### General
- View
- Print
- Export

---

## 13. Primary CTA
- Create PO

---

## 14. Status UI Rules
- Draft → editable
- Submitted → pending approval
- Approved → ready for GRN
- Closed → completed
- Cancelled → inactive

---

## 15. Role UI Rules
Executive:
- create/edit PO
Manager:
- approve/reject
Store:
- view for GRN

---

## 16. Data Requirements
- PO list
- summary counts
- vendor data
- receipt progress

---

## 17. API Dependencies
- GET /purchase-orders
- POST /purchase-orders
- POST /purchase-orders/{id}/submit
- POST /purchase-orders/{id}/approve
- POST /purchase-orders/{id}/reject

---

## 18. States
- Loading
- Empty
- Error
- No access

---

## 19. Notes
- Must highlight GRN readiness
- Must show vendor clearly
- Must connect to PR

---

## 20. Next
- page_po_detail.md
