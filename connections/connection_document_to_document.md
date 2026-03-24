# Connection: Document to Document Mapping

## 1. Purpose
Defines how core procurement documents are connected:
PR → PO → GRN → QC → Stock

Ensures traceability, quantity control, and workflow integrity across the system.

---

## 2. PR → PO Connection

### Rule
- PO can only be created from Approved PR

### Mapping
- PR Header → PO Header (context, department, reference)
- PR Line → PO Line (item, qty, UOM)

### Quantity Logic
- Ordered Qty ≤ PR Requested Qty
- Partial conversion allowed

### Status Impact
- PR Approved → Eligible for PO
- PR Partially Converted → track remaining
- PR Fully Converted → closed/complete

---

## 3. PO → GRN Connection

### Rule
- GRN can only be created from Approved PO

### Mapping
- PO Header → GRN Header (vendor, warehouse)
- PO Line → GRN Line (item, qty reference)

### Quantity Logic
- GRN Received Qty ≤ PO Remaining Qty
- Partial receipt allowed

### Status Impact
- PO Approved → Eligible for GRN
- PO Partially Received → track remaining
- PO Fully Received → ready to close

---

## 4. GRN → QC Connection

### Rule
- If item marked QC Required → must go through QC

### Flow
- GRN Received → QC Pending → Accepted / Rejected

### Quantity Logic
- Accepted + Rejected = Received

### Impact
- Accepted → moves to stock
- Rejected → blocked / return flow

---

## 5. GRN → Stock Impact

### Rule
- Only Accepted Qty impacts usable stock

### Flow
- Accepted → Inventory Increase
- Rejected → no stock or quarantine

---

## 6. Traceability Chain

Every document must store:

- source_document_id
- source_document_line_id
- downstream_document_ids

### Example
PR → PO → GRN → Inventory Movement

---

## 7. API Connection Rules

### Required APIs
- PR → Create PO
- PO → Create GRN
- GRN → Accept / Reject

### Validation
- enforce upstream status
- enforce quantity limits
- enforce role permissions

---

## 8. UI Connection Rules

### Navigation
- PR Detail → Convert to PO
- PO Detail → Receive Goods (GRN)
- GRN Detail → View PO

### Indicators
- show linked document count
- show status chain

---

## 9. Completion Criteria
Connection design is complete when:
- all document links are defined
- quantity rules are enforced
- status dependencies are clear
- frontend navigation paths are defined
- backend validation rules are aligned
