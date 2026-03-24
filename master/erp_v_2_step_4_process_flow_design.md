# ERP V2 – Step 4: Process Flow Design (Transaction Flows + Status + Control)

## 1. Purpose
This document defines end-to-end process flows across modules, including:
- Transaction lifecycle
- Status transitions
- User roles involved
- Approval stages
- Exception and reverse flows

This is the most critical document for ensuring real-world ERP correctness.

---

## 2. Flow Design Standard
Each process will include:
- Process Name
- Actors (Roles)
- Steps
- Status Flow
- Approval Points
- Exception Scenarios
- System Impact

---

# 3. Core Business Flows

---

## 3.1 Procurement Flow (PR → PO → GRN)

### Actors
- Procurement Executive
- Procurement Manager
- Store User
- QA (optional)

### Steps
1. Create Purchase Requisition (PR)
2. PR Approval
3. Create Purchase Order (PO)
4. PO Approval
5. Receive Goods (GRN)
6. Quality Check (if applicable)
7. Stock Update

### Status Flow
- PR: Draft → Submitted → Approved → Rejected
- PO: Draft → Submitted → Approved → Closed
- GRN: Pending → Received → QC Pending → Accepted / Rejected

### Approval Points
- PR Approval (Manager)
- PO Approval (Manager / Finance)

### Exception Scenarios
- PR Rejected
- PO Modified after approval
- Partial GRN
- Rejected material

### System Impact
- Inventory increases on GRN acceptance

---

## 3.2 Inventory Movement Flow

### Actors
- Store User

### Steps
1. Create Stock Transfer
2. Approve Transfer (optional)
3. Execute Movement

### Status Flow
- Draft → Approved → Completed

### Exception Scenarios
- Insufficient stock
- Wrong location transfer

### System Impact
- Stock moves between warehouse/bin

---

## 3.3 Production Flow

### Actors
- Production Planner
- Production Supervisor
- Store User

### Steps
1. Create Production Plan
2. Create Work Order
3. Issue Raw Materials
4. Execute Production
5. Record Output

### Status Flow
- Work Order: Planned → Released → In Progress → Completed → Closed

### Exception Scenarios
- Material shortage
- Rework required
- Production variance

### System Impact
- Raw material decreases
- Finished goods increase

---

## 3.4 Quality Flow

### Actors
- QA Inspector
- QA Manager

### Steps
1. Perform Inspection
2. Record Result
3. Approve / Reject

### Status Flow
- Pending → Inspected → Approved / Rejected

### Exception Scenarios
- Reinspection
- Conditional approval

---

## 3.5 Sales → Dispatch Flow

### Actors
- Sales Executive
- Dispatch Team

### Steps
1. Create Sales Order (SO)
2. Approve SO
3. Allocate Stock
4. Dispatch Goods

### Status Flow
- SO: Draft → Approved → Allocated → Dispatched → Closed

### Exception Scenarios
- Partial dispatch
- Stock shortage

### System Impact
- Inventory decreases

---

## 3.6 Finance Flow

### Actors
- Finance Team

### Steps
1. Generate Invoice
2. Record Payment

### Status Flow
- Invoice: Draft → Issued → Paid / Partial / Overdue

### Exception Scenarios
- Payment mismatch
- Credit note

---

# 4. Cross-Module Controls

### 4.1 Approval Rules
- Based on role
- Can be extended to amount-based approval

### 4.2 Status Integrity
- Status must follow strict transition rules
- No skipping stages

### 4.3 Maker-Checker
- Creator ≠ Approver (recommended)

---

# 5. Missing Advanced Flows (To Add in Next Iteration)

- Purchase Return
- Sales Return
- Rework Flow
- Scrap Handling
- Subcontracting

---

## 6. Next Step

Proceed to:
👉 Step 5 – Screen Map (page-level navigation and structure)

