# Flow: Subcontracting

## 1. Business Purpose
Subcontracting is the business process where certain manufacturing or processing operations are outsourced to an external vendor while ownership of raw materials or semi-finished goods remains with the company.

This flow ensures:
- materials sent to vendor are tracked
- processing done externally is controlled
- received finished/semi-finished goods are validated
- stock movement across locations (company ↔ vendor) is traceable
- cost and vendor performance are monitored

Subcontracting is a hybrid flow involving procurement, production, inventory, and quality.

---

## 2. Trigger Events
Subcontracting may be triggered when:
- in-house capacity is insufficient
- specialized processing is required (coating, machining, etc.)
- cost optimization decision
- bulk production outsourcing
- seasonal demand spikes

---

## 3. Roles Involved
- Production Planner
- Procurement Executive
- Store User
- QA Inspector
- Vendor (external)
- Finance Officer

---

## 4. Preconditions
- subcontracting BOM or process defined
- vendor selected and approved
- raw material available in stock
- subcontracting PO created

---

## 5. Source References
- Subcontracting Purchase Order
- BOM / Process definition
- Inventory movement
- QC inspection
- Vendor record

---

## 6. Flow Overview
1. Subcontracting PO created
2. Raw materials issued to vendor
3. Vendor processes material
4. Finished goods received
5. QC inspection performed
6. Stock updated
7. Vendor billing processed

---

## 7. Detailed Steps

### Step 1 – Create Subcontracting PO
- define vendor
- define item to be processed
- define quantity
- define process cost

### Step 2 – Issue Material to Vendor
- stock moved from warehouse → vendor location
- track quantity issued
- create inventory movement

### Step 3 – Vendor Processing
- vendor performs required operations
- no direct system control, but tracked via PO

### Step 4 – Receive Processed Goods
- GRN created for subcontracting
- link to PO and issued materials

### Step 5 – QC Inspection
- validate quality of processed goods
- accept/reject/rework decision

### Step 6 – Inventory Update
- accepted → usable stock
- rejected → rework/scrap

### Step 7 – Vendor Billing
- process service invoice
- update payable

---

## 8. Status Model
- Draft
- Submitted
- Approved
- Material Issued
- In Process
- Received
- QC Pending
- Completed
- Closed
- Cancelled

---

## 9. Inventory Impact
- raw material reduced from warehouse
- moved to vendor stock
- finished goods added on receipt

---

## 10. QC Impact
- inspection on receipt
- defect tracking
- vendor quality performance

---

## 11. Finance Impact
- service cost booking
- vendor payable
- cost added to product

---

## 12. UI Impact
Future pages:
- Subcontracting PO
- Material Issue to Vendor
- Subcontracting GRN
- Vendor Performance Dashboard

---

## 13. API Impact
- create subcontract PO
- issue material
- receive goods
- QC result
- vendor billing

---

## 14. Audit Requirements
- material issued
- qty processed
- receipt qty
- QC result
- cost recorded

---

## 15. Edge Cases
- partial receipt
- material loss at vendor
- rejected goods
- vendor delay
- excess processing

---

## 16. Completion Criteria
Flow is complete when:
- material issued tracked
- goods received and validated
- inventory updated
- vendor cost processed

---

## 17. Next Files
- transactions/txn_subcontracting.md
