# ERP V2 – Step 1: Planning (What, Why, Who)

## 1. Purpose of This Document
This document defines the foundational vision of the ERP system. It establishes the scope, objectives, stakeholders, and business context before moving into detailed design.

---

## 2. What (System Definition)

### 2.1 ERP Scope
A centralized Manufacturing ERP system designed to manage end-to-end operations:
- Master Data Management
- Procurement
- Inventory Management
- Production Planning & Execution
- Quality Control
- Sales & Dispatch
- Finance & Accounting
- Reporting & Analytics
- Administration & Access Control

### 2.2 Business Coverage
- Multi-company (future-ready)
- Multi-plant / factory
- Multi-warehouse with bin/location tracking
- Batch / Serial tracking (configurable)

### 2.3 Core Objectives
- Single source of truth for all operations
- Real-time visibility of inventory, production, and orders
- Standardized workflows across departments
- Strong audit trail and traceability
- Scalable architecture for enterprise use

---

## 3. Why (Business Goals)

### 3.1 Problems Being Solved
- Data fragmentation across systems
- Manual and error-prone processes
- Lack of real-time visibility
- Weak tracking of inventory and production
- No structured approval or control systems

### 3.2 Expected Outcomes
- Improved operational efficiency
- Reduced errors and data inconsistencies
- Faster decision-making
- Better compliance and audit readiness
- Scalable system for business growth

---

## 4. Who (Stakeholders & Users)

### 4.1 User Groups
- Admin
- Management (CEO, COO, Finance Head)
- Procurement Team
- Store/Warehouse Team
- Production Team
- Quality Team
- Sales Team
- Accounts/Finance Team

### 4.2 Role Characteristics
- Role-based access control (RBAC)
- Maker-Checker approval structure
- Department-based permissions
- Future: Plant/Warehouse level access restrictions

---

## 5. ERP Modules (High-Level)

1. Master Data
2. Procurement
3. Inventory
4. Production
5. Quality
6. Sales
7. Dispatch
8. Finance
9. Reports
10. Admin / RBAC

---

## 6. End-to-End Business Cycle (Simplified)

1. Purchase Requisition (PR)
2. Purchase Order (PO)
3. Goods Receipt (GRN)
4. Inventory Update
5. Production Planning
6. Production Execution
7. Quality Check
8. Finished Goods Storage
9. Sales Order (SO)
10. Dispatch
11. Invoicing

---

## 7. Key Design Principles

- Modular architecture
- Clear separation of concerns
- Configurable workflows
- Strong validation and controls
- Auditability of all transactions
- API-first backend design
- Scalable database structure

---

## 8. Assumptions

- System will initially support a single company but designed for multi-company
- Users are trained and role-based
- Internet-based centralized system

---

## 9. Out of Scope (V2 Planning Stage)

- Advanced AI/ML forecasting
- External integrations (initial phase)
- Mobile-first UI (can be added later)

---

## 10. Next Step

Proceed to:
👉 Step 2 – Module Design (Detailed Functional Breakdown)

