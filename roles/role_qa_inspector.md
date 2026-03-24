# Role: QA Inspector

## 1. Role Purpose
Responsible for quality verification of received materials and products that require inspection before final acceptance into usable stock or before release to the next operational stage. This role is a control role in the ERP flow and ensures that acceptance, rejection, and disposition decisions are recorded clearly and traceably.

---

## 2. Modules Accessible
- Quality
- Procurement (limited visibility for QC-linked receipts)
- Inventory (limited visibility for stock disposition context)

---

## 3. Pages Accessible
- GRN Detail
- Incoming QC
- In-Process QC (future)
- Final QC (future)
- QC Reports (view as permitted)
- PO Detail (view only where needed for source review)

---

## 4. Core Responsibilities
- review QC-required receipts
- inspect incoming materials against defined quality criteria
- record inspection findings
- accept or reject materials based on inspection outcome
- document defects, conditional observations, and rejection reasons
- ensure non-conforming items do not move into unrestricted usable stock
- support auditability of all quality decisions

---

## 5. Actions Allowed
- View GRN linked for QC
- Open QC-required receipt/work item
- Record inspection result
- Accept received items after quality pass
- Reject received items after quality failure
- Add QC comments
- Attach QC evidence/documents
- View linked source PO and receipt context

---

## 6. Actions Not Allowed
- Create PR
- Approve PR
- Create PO
- Approve PO
- Modify PO commercial terms
- Create financial entries
- Change warehouse receipt quantities after operational receipt is finalized unless exception policy explicitly allows it

---

## 7. Approval / Disposition Authority
- Can approve or reject quality disposition for assigned QC-required receipt items
- Can move receipt from QC Pending to Accepted or Rejected based on policy
- May require QA Manager escalation for conditional approval, high-risk rejection, or exceptional cases in future design

---

## 8. Field-Level Rules
### Editable by QA Inspector
- inspection result
- inspection remarks
- accepted quantity
- rejected quantity
- defect classification
- QC evidence attachments
- rejection reason
- QC status fields assigned to inspection action

### Read-Only for QA Inspector
- source PO commercial details
- receipt quantities already finalized by warehouse, except where inspection disposition fields are explicitly separate
- procurement approval fields
- finance-related fields

---

## 9. Record Scope Visibility
Typical scope:
- QC-required receipts assigned to the QA team
- incoming materials pending quality decision
- historical QC decisions for audit and traceability
- location/plant/warehouse scope based on assignment policy

---

## 10. UI Behavior
The UI should:
- show QC Pending items prominently
- highlight inspection-required lines
- provide clear Accept / Reject actions
- require reason on rejection
- show source receipt and PO context in read-only form
- show defect/discrepancy indicators strongly
- support evidence attachment and QC comments
- hide procurement approval buttons
- hide warehouse receipt-entry controls not relevant to QA role

---

## 11. Dashboard / Queue Visibility
Recommended QA dashboard visibility:
- Pending Incoming QC count
- Today’s inspections
- Rejected receipt count
- Accepted vs Rejected summary
- Urgent or aged QC-pending receipts
- Shortcuts to Incoming QC queue

---

## 12. Workflow Notes
This role operates mainly after warehouse receipt and before unrestricted stock release where QC is required.

Typical flow participation:
- GRN marked Received
- status moves to QC Pending
- QA Inspector reviews line(s)
- QA Inspector accepts or rejects
- system updates final stock disposition and audit trail

This role is therefore critical for:
- QC gate enforcement
- prevention of bad stock release
- procurement quality feedback
- traceable material disposition

---

## 13. Backend Authorization Notes
Backend should:
- allow QA Inspector to access QC-required receipt records
- allow inspection result submission
- allow accept/reject actions only on valid QC statuses
- enforce rejection reason when required
- block unauthorized warehouse/procurement data mutation
- log every QC disposition action with timestamp, actor, and result
- connect QC decision to stock-impact rules

---

## 14. Frontend Behavior Notes
Frontend should:
- render Accept / Reject only when record is QC Pending or eligible for QC action
- show GRN and PO context in read-only sections
- expose inspection form fields clearly
- enforce comment/reason requirements in the UI before submit where possible
- refresh linked stock/QC status after decision
- display historical QC decisions and evidence cleanly

---

## 15. Integration / Dependency Notes
This role depends on:
- GRN transaction and GRN Detail page
- QC status and disposition logic
- stock-impact rules after acceptance/rejection
- attachment/comment support
- audit logging
- future QA Manager escalation flow if implemented

---

## 16. Example Role Summary
QA Inspector is a **quality control execution role**, not a procurement or finance approval role.

In practice:
- warehouse receives the material
- QA inspects the material
- system records pass/fail disposition
- accepted stock moves forward
- rejected stock is isolated or blocked

---

## 17. Completion Notes
This role file is complete when:
- QC actions are defined
- limits of authority are clear
- UI behavior is separated from warehouse/procurement roles
- backend permission implications are explicit
- GRN/QC linkage is fully understood
