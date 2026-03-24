# Role: Procurement Manager

## 1. Role Purpose
Responsible for reviewing, approving, and controlling procurement activities including PR and PO approvals, vendor decisions, and procurement governance.

## 2. Modules Accessible
- Procurement
- Vendor Master
- Reports

## 3. Pages Accessible
- PR List / Detail
- PO List / Detail
- Vendor Master
- Reports Dashboard

## 4. Dashboard Visibility
- Pending PR approvals
- Pending PO approvals
- Procurement summary KPIs

## 5. Actions Allowed
- Approve PR
- Reject PR
- Approve PO
- Reject PO
- View and override (with audit)
- Export reports

## 6. Actions Not Allowed
- Direct creation of PR/PO (recommended restricted)
- Editing submitted PR/PO (must return instead)

## 7. Approval Authority
- PR Approval
- PO Approval
- Conditional approval based on business rules

## 8. Field-Level Rules
- Read-only for submitted records
- Editable only when returned for correction

## 9. Record Scope Visibility
- Department-wide or organization-wide (based on role scope)

## 10. UI Behavior
- Show Approve/Reject buttons
- Show approval queue
- Hide create buttons (optional)
- Show audit/history panel prominently

## 11. Backend Authorization Notes
- Allow approval endpoints
- Enforce status-based transitions
- Log all approval actions

## 12. Workflow Notes
- Operates in Submitted stage
- Controls transition to Approved/Rejected
