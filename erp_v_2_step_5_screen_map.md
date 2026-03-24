# ERP V2 – Step 5: Screen Map (Navigation + Page Structure)

## 1. Purpose
This document defines the full screen structure of the ERP system, including:
- Navigation hierarchy
- Page grouping by module
- Entry points for users
- Flow between screens

This acts as the bridge between process flows and UI wireframes.

---

## 2. Navigation Structure

### 2.1 Main Navigation (Sidebar)

- Dashboard
- Master Data
- Procurement
- Inventory
- Production
- Quality
- Sales
- Dispatch
- Finance
- Reports
- Admin / RBAC

---

## 3. Module-wise Screen Map

---

## 3.1 Dashboard

### Pages
- Overview Dashboard
- KPI Widgets

---

## 3.2 Master Data

### Pages
- Item Master
- BOM Master
- Vendor Master
- Customer Master
- Warehouse / Location / Bin Master
- UOM Master

---

## 3.3 Procurement

### Pages
- PR List
- Create PR
- PR Detail View

- PO List
- Create PO
- PO Detail View

- GRN List
- GRN Entry
- GRN Detail View

---

## 3.4 Inventory

### Pages
- Stock Overview
- Stock Ledger
- Stock Movement
- Stock Transfer
- Stock Adjustment

---

## 3.5 Production

### Pages
- Production Plan List
- Work Order List
- Work Order Detail
- Material Issue Screen
- Production Entry Screen

---

## 3.6 Quality

### Pages
- Incoming QC
- In-Process QC
- Final QC
- QC Reports

---

## 3.7 Sales

### Pages
- Sales Order List
- Create Sales Order
- Sales Order Detail

---

## 3.8 Dispatch

### Pages
- Dispatch Planning
- Delivery Note List
- Delivery Entry

---

## 3.9 Finance

### Pages
- Invoice List
- Invoice Entry
- Payment Entry

---

## 3.10 Reports

### Pages
- Inventory Reports
- Sales Reports
- Production Reports
- Financial Reports

---

## 3.11 Admin / RBAC

### Pages
- Module Master
- Permission Master
- Role Management
- User Management
- Role Access Matrix

---

## 4. Screen Types (Standardization)

Each module will use standard screen patterns:

### 4.1 List Screen
- Table view
- Filters / search
- Bulk actions
- Navigation to detail

### 4.2 Create / Edit Screen
- Form-based input
- Validation rules
- Save / Submit actions

### 4.3 Detail View Screen
- Full record view
- Status display
- Action buttons (approve, reject, edit)

---

## 5. Navigation Rules

- User sees only permitted modules (RBAC)
- Actions shown based on permissions
- Status controls available based on role

---

## 6. Flow Linking Example

Procurement:
PR List → PR Detail → Convert to PO → PO Detail → GRN Entry

Production:
Work Order → Issue Material → Production Entry → Completion

---

## 7. Next Step

Proceed to:
👉 Step 6 – Wireframe Design (Banani prompts with page-level detail)

