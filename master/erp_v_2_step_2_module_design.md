# ERP V2 – Step 2: Module Design (Detailed Functional Breakdown)

## 1. Purpose
This document defines each ERP module in detail, including pages, actions, and functional responsibilities. This is the foundation for process flows, UI design, and backend logic.

---

## 2. Module Structure Standard
Each module will be defined using the following structure:
- Module Objective
- Key Entities
- Pages/Screens
- Actions
- Status Flow (high level)
- Dependencies

---

# 3. Modules

---

## 3.1 Master Data Module

### Objective
Manage all core business entities used across the system.

### Key Entities
- Item / Product
- BOM (Bill of Materials)
- Vendor
- Customer
- Warehouse / Location / Bin
- Unit of Measure (UOM)

### Pages
- Item Master
- BOM Master
- Vendor Master
- Customer Master
- Warehouse Master

### Actions
- Create / Edit / View / Deactivate
- Versioning (BOM)

### Dependencies
- Used by all modules

---

## 3.2 Procurement Module

### Objective
Manage purchasing process from requisition to receipt.

### Key Entities
- Purchase Requisition (PR)
- Purchase Order (PO)
- Goods Receipt Note (GRN)

### Pages
- PR List / Create
- PO List / Create
- GRN Entry

### Actions
- Create PR
- Approve PR
- Convert PR → PO
- Approve PO
- Receive Goods (GRN)

### Status Flow (High Level)
PR → Approved → PO → Approved → GRN → Completed

### Dependencies
- Master Data
- Inventory

---

## 3.3 Inventory Module

### Objective
Track stock across warehouses and locations.

### Key Entities
- Stock Ledger
- Stock Movement
- Batch / Serial

### Pages
- Stock Overview
- Stock Movement Entry
- Stock Adjustment

### Actions
- Transfer Stock
- Adjust Stock
- View Ledger

### Dependencies
- Procurement
- Production
- Sales

---

## 3.4 Production Module

### Objective
Manage manufacturing operations.

### Key Entities
- Production Plan
- Work Order
- Material Issue
- Production Output

### Pages
- Production Planning
- Work Order List
- Issue Materials
- Production Entry

### Actions
- Create Work Order
- Issue Materials
- Record Production

### Dependencies
- Inventory
- BOM

---

## 3.5 Quality Module

### Objective
Ensure quality compliance of materials and products.

### Key Entities
- Quality Inspection
- Rejection / Approval

### Pages
- Incoming QC
- In-Process QC
- Final QC

### Actions
- Inspect Material
- Approve / Reject

### Dependencies
- Procurement
- Production

---

## 3.6 Sales Module

### Objective
Manage customer orders.

### Key Entities
- Sales Order (SO)

### Pages
- SO List / Create

### Actions
- Create SO
- Approve SO

### Dependencies
- Inventory

---

## 3.7 Dispatch Module

### Objective
Handle shipment of goods.

### Key Entities
- Delivery Note
- Dispatch

### Pages
- Dispatch Planning
- Delivery Entry

### Actions
- Allocate Stock
- Dispatch Goods

### Dependencies
- Sales
- Inventory

---

## 3.8 Finance Module

### Objective
Manage financial transactions.

### Key Entities
- Invoice
- Payment

### Pages
- Invoice Entry
- Payment Entry

### Actions
- Generate Invoice
- Record Payment

### Dependencies
- Procurement
- Sales

---

## 3.9 Reports Module

### Objective
Provide insights and analytics.

### Pages
- Inventory Reports
- Sales Reports
- Production Reports

---

## 3.10 Admin / RBAC Module

### Objective
Manage users and permissions.

### Key Entities
- User
- Role
- Permission

### Pages
- User Management
- Role Management

### Actions
- Assign Roles
- Configure Permissions

---

## 4. Next Step

Proceed to:
👉 Step 3 – User Role Design (Detailed RBAC + Permissions)

