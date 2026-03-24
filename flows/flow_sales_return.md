# Flow: Sales Return

## 1. Business Purpose
Sales Return is the process where goods sold to a customer are returned back to the organization due to defects, damage, wrong delivery, excess supply, or customer dissatisfaction.

This flow ensures:
- returned goods are tracked and validated
- inventory is updated correctly
- financial impact (credit note/refund) is handled
- quality and customer issues are captured
- traceability to original sales order/invoice is maintained

---

## 2. Trigger Events
Sales Return may be triggered when:
- customer receives damaged goods
- wrong item delivered
- excess quantity delivered
- product defect found after delivery
- customer rejects goods during delivery
- warranty/return policy allows return

---

## 3. Roles Involved
- Sales Executive
- Dispatch User
- Store User
- QA Inspector (if quality validation needed)
- Finance Officer

---

## 4. Preconditions
- original Sales Order (SO) exists
- goods were delivered (dispatch/invoice exists)
- return is within allowed return policy
- quantity requested ≤ delivered quantity (minus previous returns)

---

## 5. Source References
- Sales Order
- Delivery/Dispatch record
- Invoice
- Inventory movement
- QC inspection (if applicable)

---

## 6. Flow Overview
1. Customer initiates return
2. Return request is recorded
3. Return is validated
4. Goods are received back
5. QC inspection (if needed)
6. Inventory updated
7. Finance adjustment processed
8. Return closed

---

## 7. Detailed Steps

### Step 1 – Return Request
- initiated by sales or customer support
- capture:
  - customer
  - item
  - quantity
  - reason

### Step 2 – Validate Eligibility
- check delivered qty vs return qty
- verify policy window
- check if item already consumed/used

### Step 3 – Create Sales Return Record
- reference SO and invoice
- define return qty and reason
- status = Draft → Submitted

### Step 4 – Approval (if required)
- manager approves or rejects

### Step 5 – Receive Goods
- warehouse receives returned goods
- record condition and qty

### Step 6 – QC Inspection (optional)
- inspect returned goods
- classify:
  - reusable
  - rework
  - scrap

### Step 7 – Inventory Update
- increase stock (if reusable)
- move to QC/rework/scrap if needed

### Step 8 – Finance Adjustment
- create credit note or refund
- adjust customer balance

### Step 9 – Close Return
- after stock + finance reconciliation

---

## 8. Status Model
- Draft
- Submitted
- Approved
- Rejected
- Pending Receipt
- Received
- QC Pending
- Completed
- Closed
- Cancelled

---

## 9. Quantity Logic
- Return Qty ≤ Delivered Qty - Already Returned Qty
- Partial returns allowed
- Multiple returns allowed per SO line

---

## 10. Inventory Impact
- reusable stock → increase available stock
- defective → QC hold or scrap
- wrong item → restock correctly

---

## 11. QC Impact
- optional QC inspection
- classification:
  - usable
  - rework
  - scrap

---

## 12. Finance Impact
- credit note
- refund
- invoice adjustment

---

## 13. UI Impact
Future pages:
- Sales Return List
- Sales Return Detail
- Return Receipt Screen

---

## 14. API Impact
- create sales return
- get return detail
- approve/reject
- receive return
- update QC result
- process refund

---

## 15. Audit Requirements
- return request
- qty changes
- approvals
- receipt confirmation
- QC result
- finance adjustment

---

## 16. Edge Cases
- partial return
- damaged return
- expired return
- no-invoice return (manual case)
- multiple returns

---

## 17. Completion Criteria
Flow is complete when:
- linked to SO/invoice
- qty validated
- inventory updated
- finance adjusted
- audit trail exists

---

## 18. Next Files
- flow_rework.md
- flow_scrap_handling.md
- flow_subcontracting.md
