# Frontend Permission Logic Matrix

## 1. Purpose
This file defines how frontend permission logic should behave for the current ERP design slice.

It standardizes:
- page-level access
- action-level access
- status-based action gating
- role-based visibility rules
- downstream action visibility
- read-only vs editable behavior

This file is the bridge between:
- Step 3 (User Role Design)
- Step 4 (Process Flow Design)
- Step 9 (Frontend Development)
- actual frontend UI behavior

---

## 2. Recommended Folder Location
Keep this file at:

`frontend/frontend_permission_logic_matrix.md`

---

## 3. Scope Covered
This matrix currently covers:
- Procurement Executive
- Procurement Manager
- Store User
- QA Inspector

Across these pages:
- PR List
- PR Detail
- PO List
- PO Detail
- GRN Entry
- GRN Detail
- Incoming QC
- Stock Overview

---

## 4. Permission Logic Principles
### Backend is final authority
Frontend permission logic is for UX and guidance, not final security.

### Role + status both matter
A role may generally have access to a page, but action visibility still depends on record status.

### Hide or disable intentionally
- hide actions the user should never know about
- disable actions that exist conceptually but are not currently valid because of status/state

### Maker-checker must be visible in UI
The frontend must not present approval actions to a role that is acting only as maker in the current workflow.

---

## 5. Standard Permission Actions
The frontend should evaluate these action families:

- canViewPage
- canCreateRecord
- canEditDraft
- canSubmitRecord
- canApproveRecord
- canRejectRecord
- canCancelRecord
- canCloseRecord
- canExportRecord
- canOpenDownstreamAction
- canAddComment
- canUploadAttachment
- canViewAudit
- canViewLinkedRecords

---

# 6. Role Summary

## 6.1 Procurement Executive
Primary function:
- maker for PR and PO
- can create, edit draft, submit
- can convert approved PR to PO
- cannot approve PR/PO

## 6.2 Procurement Manager
Primary function:
- checker for PR and PO
- can approve/reject submitted PR/PO
- generally read-only on document content

## 6.3 Store User
Primary function:
- operational receiving role
- can create and process GRN against approved PO
- can record warehouse receipt context
- cannot approve procurement documents

## 6.4 QA Inspector
Primary function:
- QC execution role
- can inspect QC-required receipts
- can accept/reject QC disposition
- cannot edit procurement/commercial data

---

# 7. Page-Level Access Matrix

## 7.1 PR List
- Procurement Executive: View = Yes
- Procurement Manager: View = Yes
- Store User: View = No
- QA Inspector: View = No

### Create Button
- Procurement Executive: Yes
- Procurement Manager: Optional / usually No
- Store User: No
- QA Inspector: No

---

## 7.2 PR Detail
- Procurement Executive: View = Yes
- Procurement Manager: View = Yes
- Store User: No
- QA Inspector: No

---

## 7.3 PO List
- Procurement Executive: View = Yes
- Procurement Manager: View = Yes
- Store User: View = Yes (read-only context)
- QA Inspector: Optional limited view if policy allows, usually No list access

### Create Button
- Procurement Executive: Yes
- Procurement Manager: Optional / usually No
- Store User: No
- QA Inspector: No

---

## 7.4 PO Detail
- Procurement Executive: View = Yes
- Procurement Manager: View = Yes
- Store User: View = Yes
- QA Inspector: View = Yes (source context only if allowed)

---

## 7.5 GRN Entry
- Procurement Executive: No
- Procurement Manager: No
- Store User: Yes
- QA Inspector: No

---

## 7.6 GRN Detail
- Procurement Executive: View = Yes (traceability)
- Procurement Manager: View = Yes
- Store User: View = Yes
- QA Inspector: View = Yes

---

## 7.7 Incoming QC
- Procurement Executive: No
- Procurement Manager: No
- Store User: No
- QA Inspector: Yes

---

## 7.8 Stock Overview
- Procurement Executive: Yes (view)
- Procurement Manager: Yes (view)
- Store User: Yes
- QA Inspector: Yes (QC stock view)
- Additional future roles like Production Planner: Yes (view)

---

# 8. Action Logic Matrix by Document Status

# 8.1 Purchase Requisition (PR)

## Draft PR
### Procurement Executive
- View: Yes
- Edit: Yes
- Submit: Yes
- Cancel: Yes
- Approve: No
- Reject: No
- Convert to PO: No
- Add Comment: Yes
- Upload Attachment: Yes

### Procurement Manager
- View: Yes
- Edit: No
- Submit: No
- Cancel: No
- Approve: No
- Reject: No

## Submitted PR
### Procurement Executive
- View: Yes
- Edit: No
- Submit: No
- Cancel: Usually No / policy-based
- Approve: No
- Reject: No
- Convert to PO: No
- Add Comment: Yes
- Upload Attachment: Restricted / policy-based

### Procurement Manager
- View: Yes
- Approve: Yes
- Reject: Yes
- Edit: No
- Submit: No
- Cancel: Optional if policy allows
- Add Comment: Yes
- View Audit: Yes

## Approved PR
### Procurement Executive
- View: Yes
- Convert to PO: Yes
- View Linked PO: Yes
- Edit: No
- Approve: No

### Procurement Manager
- View: Yes
- Approve: No further action
- Reject: No further action
- Edit: No

## Rejected PR
### Procurement Executive
- View: Yes
- Revise/Resubmit: If revision flow enabled
- Cancel: Yes / policy-based
- View Rejection Reason: Yes

### Procurement Manager
- View: Yes
- Edit: No
- Approve: No
- Reject: No

---

# 8.2 Purchase Order (PO)

## Draft PO
### Procurement Executive
- View: Yes
- Edit: Yes
- Submit: Yes
- Cancel: Yes
- Approve: No
- Reject: No
- Create GRN: No
- Add Comment: Yes
- Upload Attachment: Yes

### Procurement Manager
- View: Yes
- Edit: No
- Approve: No
- Reject: No

## Submitted PO
### Procurement Executive
- View: Yes
- Edit: No
- Submit: No
- Approve: No
- Reject: No
- Create GRN: No

### Procurement Manager
- View: Yes
- Approve: Yes
- Reject: Yes
- Add Comment: Yes
- View Audit: Yes

## Approved PO
### Procurement Executive
- View: Yes
- Create GRN: Optional visible as downstream support, not primary executor
- View Linked GRN: Yes
- Edit: No

### Procurement Manager
- View: Yes
- Close: Optional if policy allows
- Edit: No

### Store User
- View: Yes
- Create GRN / Receive Goods: Yes
- View Linked GRN: Yes
- Edit commercial terms: No

### QA Inspector
- View: Yes if allowed
- No commercial mutations
- No GRN create action

## Rejected PO
### Procurement Executive
- View: Yes
- Revise/Resubmit: If revision flow enabled
- Cancel: Policy-based

### Procurement Manager
- View: Yes
- No further workflow action unless returned flow exists

---

# 8.3 Goods Receipt Note (GRN)

## Pending GRN
### Store User
- View: Yes
- Edit: Yes
- Mark Received: Yes
- Send to QC: If policy allows at this stage
- Cancel: Yes
- Accept: No
- Reject: No
- Add Comment: Yes
- Upload Attachment: Yes

### QA Inspector
- View: Usually No until QC stage, or limited if linked context is opened
- Edit: No
- Accept/Reject: No at Pending stage

## Received GRN
### Store User
- View: Yes
- Edit: No or very restricted
- Send to QC: Yes if QC required
- Accept: Yes only if no QC required and policy permits warehouse acceptance
- Reject: Yes only if operational rejection model allows
- Cancel: Restricted

### QA Inspector
- View: Yes if QC required
- Send to QC: N/A
- Accept: No unless status moved into QC Pending or combined workflow allows
- Reject: No unless in QC flow state

## QC Pending GRN
### Store User
- View: Yes
- Edit: No
- Accept: No
- Reject: No
- Comment: Yes

### QA Inspector
- View: Yes
- Accept: Yes
- Reject: Yes
- Edit receipt quantity: No
- Add Comment: Yes
- Upload QC Evidence: Yes

## Accepted GRN
### Store User
- View: Yes
- View Stock Impact: Yes
- Edit: No

### QA Inspector
- View: Yes
- Accept/Reject again: No
- View QC Result: Yes

### Procurement Executive / Manager
- View: Yes
- Traceability only

## Rejected GRN
### Store User
- View: Yes
- Edit: No
- View rejection details: Yes

### QA Inspector
- View: Yes
- View rejection details: Yes
- Modify result: No unless exception flow exists

---

# 9. Incoming QC Page Logic

## QA Inspector
- View queue: Yes
- Select receipt/item: Yes
- Enter accepted qty: Yes
- Enter rejected qty: Yes
- Add inspection notes: Yes
- Upload evidence: Yes
- Save QC Result: Yes
- Accept: Yes
- Reject: Yes

## All Other Current Roles
- No access

### Important Rule
Only QA roles should see the actionable QC controls.

---

# 10. Stock Overview Logic

## Procurement Executive
- View stock list: Yes
- Filter/search: Yes
- Export: If permitted
- Stock mutation actions: No

## Procurement Manager
- View stock list: Yes
- Filter/search: Yes
- Export: If permitted
- Stock mutation actions: No

## Store User
- View stock list: Yes
- Filter/search: Yes
- Export: Policy-based
- Future transfer/adjust actions: permission-based only

## QA Inspector
- View stock list: Yes
- QC Hold and rejected stock visibility: Yes
- Stock mutation actions: No

---

# 11. Downstream Action Visibility Rules

## PR Detail → Convert to PO
Show only if:
- role can create PO or open downstream procurement action
- PR status = Approved
- user has visibility to procurement downstream flow

## PO Detail → Create GRN
Show only if:
- role = Store User or authorized receiving role
- PO status = Approved
- remaining receivable quantity > 0

## GRN Detail → View Stock Impact
Show only if:
- stock posting or stock-impact summary exists
- user has inventory visibility

## GRN Detail / Incoming QC → Accept / Reject
Show only if:
- user is QA role
- record/line is QC actionable
- status is Received / QC Pending as defined by workflow

---

# 12. Read-Only vs Editable Rules

## Editable means:
- input fields enabled
- save/submit action possible
- draft mutation allowed

## Read-only means:
- fields visible but disabled/static
- workflow context still visible
- comments/attachments may still be partially allowed

### Standard Rule
After submission/approval/acceptance, pages should move from editable to review-oriented UI.

---

# 13. Hide vs Disable Guidance

## Hide action when:
- role should never perform the action
- action is out of user’s responsibility domain

Examples:
- QA Inspector should not see Approve PO
- Store User should not see Approve PR

## Disable action when:
- role conceptually can perform the action, but current record state does not allow it

Examples:
- Procurement Manager can approve PR, but Approve button should be disabled/hidden if PR is Draft
- Store User can mark GRN received, but button should be disabled if qty reconciliation fails

---

# 14. Shared Permission Helper Suggestions
Frontend should implement helpers like:

- `canViewPR(user, record?)`
- `canEditPR(user, record)`
- `canApprovePR(user, record)`
- `canConvertPRToPO(user, record)`
- `canCreateGRN(user, poRecord)`
- `canAcceptGRN(user, grnRecord)`
- `canRejectGRN(user, grnRecord)`
- `canViewStockOverview(user)`

### Helper Inputs
Each helper may use:
- assigned roles
- permissions
- record status
- record ownership / maker-checker context
- scope information (future branch/warehouse restrictions)

---

# 15. Future Extension Notes
This matrix can later expand to include:
- QA Manager
- Store Manager advanced controls
- Finance Officer
- Production Planner
- amount-based approval
- warehouse/branch scoped permissions
- delegated approval
- bulk-action permissions

---

# 16. Completion Criteria
This file is complete when:
- page-level visibility is defined
- role/status-based actions are defined
- downstream action visibility is defined
- hide vs disable rules are clear
- helper function direction is clear

---

# 17. Next Related Files
Recommended next files:
1. api_page_dependency_matrix.md
2. frontend_state_management_plan.md
3. frontend_folder_structure_code_plan.md
4. frontend_build_sequence_sprint_plan.md
