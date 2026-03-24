# Role: Procurement Executive

## 1. Role Purpose
Responsible for managing procurement operations including creating purchase requisitions (PR), converting approved PRs into purchase orders (PO), and coordinating with vendors.

## 2. Modules Accessible
- Procurement
- Vendor Master
- Reports (limited)

## 3. Pages Accessible
- PR List / Create / Detail
- PO List / Create / Detail
- Vendor Master (view/create)

## 4. Dashboard Visibility
- My PRs
- Pending PR submissions
- Recent POs

## 5. Actions Allowed
- Create PR
- Edit PR (Draft only)
- Submit PR
- Convert PR → PO (only after approval)
- Create PO
- Edit PO (Draft only)

## 6. Actions Not Allowed
- Approve PR
- Approve PO
- Finalize GRN

## 7. Approval Authority
- None (Maker role only)

## 8. Field-Level Rules
- Editable in Draft
- Read-only after submission

## 9. Record Scope Visibility
- Own records or assigned department

## 10. UI Behavior
- Show Create/Submit buttons
- Hide Approve buttons
- Disable fields after submission

## 11. Backend Authorization Notes
- Allow create/update endpoints
- Deny approval endpoints

## 12. Workflow Notes
- Works only in Draft and Submitted stages
