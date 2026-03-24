# First Vertical Slice Delivery Checklist

## 1. Purpose
This file is the final control checklist for delivering the first ERP vertical slice:

PR → PO → GRN → QC → Stock

It ensures:
- design completeness
- frontend completeness
- backend completeness
- integration correctness
- workflow correctness
- UAT readiness

This is the final “are we truly ready?” document before internal demo / pilot / UAT.

---

## 2. Recommended Folder Location
Keep this file at:

`delivery/first_vertical_slice_delivery_checklist.md`

(Create `delivery/` folder if not present)

---

## 3. Scope Covered

### Modules
- Procurement
- Quality (Incoming QC)
- Inventory

### Flow
- Purchase Requisition (PR)
- Purchase Order (PO)
- Goods Receipt Note (GRN)
- Incoming QC
- Stock Overview

---

# 4. Design Completion Checklist

## 4.1 Module Design
- [ ] procurement module defined
- [ ] inventory module defined
- [ ] QC/quality flow defined
- [ ] module boundaries are clear

## 4.2 Role Design
- [ ] Procurement Executive defined
- [ ] Procurement Manager defined
- [ ] Store User defined
- [ ] QA Inspector defined
- [ ] role responsibilities mapped to actions

## 4.3 Transaction Design
- [ ] PR transaction defined
- [ ] PO transaction defined
- [ ] GRN transaction defined
- [ ] QC transaction defined
- [ ] inventory movement defined

## 4.4 Page Design
- [ ] PR List + Detail
- [ ] PO List + Detail
- [ ] GRN Entry + Detail
- [ ] Incoming QC page
- [ ] Stock Overview page

## 4.5 Connection Design
- [ ] PR → PO connection
- [ ] PO → GRN connection
- [ ] GRN → QC connection
- [ ] QC → Stock connection
- [ ] linked records defined on all relevant pages

---

# 5. Frontend Completion Checklist

## 5.1 Routing
- [ ] all routes implemented
- [ ] route guards working
- [ ] deep links working
- [ ] breadcrumb correct

## 5.2 Shared Components
- [ ] PageHeader
- [ ] FilterBar
- [ ] DataTable
- [ ] StatusBadge
- [ ] ConfirmDialog
- [ ] RecordStatusHeader
- [ ] ActionBar
- [ ] CommentThreadPanel
- [ ] AttachmentPanel
- [ ] AuditTrailPanel

## 5.3 Procurement Pages
- [ ] PR List working
- [ ] PR Detail working
- [ ] PO List working
- [ ] PO Detail working

## 5.4 GRN Pages
- [ ] GRN Entry working
- [ ] GRN Detail working

## 5.5 QC Page
- [ ] Incoming QC queue working
- [ ] QC action panel working
- [ ] accept/reject logic working

## 5.6 Inventory Page
- [ ] Stock Overview working
- [ ] stock filters working
- [ ] drilldown working

## 5.7 State Management
- [ ] query keys used consistently
- [ ] mutation invalidation correct
- [ ] no stale data issues
- [ ] no unnecessary global state

## 5.8 Permission Logic
- [ ] role-based page access correct
- [ ] status-based action visibility correct
- [ ] maker-checker enforced
- [ ] QC restricted to QA role

## 5.9 UX States
- [ ] loading states implemented
- [ ] empty states implemented
- [ ] error states implemented
- [ ] no-access states implemented

---

# 6. Backend Completion Checklist

## 6.1 Shared APIs
- [ ] auth/me endpoint
- [ ] response envelope consistent
- [ ] error structure consistent
- [ ] comments API working
- [ ] attachments API working
- [ ] audit API working

## 6.2 PR APIs
- [ ] PR list
- [ ] PR summary
- [ ] PR detail
- [ ] submit/approve/reject/cancel
- [ ] create/update

## 6.3 PO APIs
- [ ] PO list
- [ ] PO summary
- [ ] PO detail
- [ ] submit/approve/reject/cancel/close
- [ ] PR → PO linkage

## 6.4 GRN APIs
- [ ] GRN prefill
- [ ] GRN create/update
- [ ] mark received
- [ ] send to QC
- [ ] accept/reject
- [ ] stock impact endpoint

## 6.5 QC APIs
- [ ] QC pending queue
- [ ] QC accept/reject
- [ ] QC evidence upload

## 6.6 Inventory APIs
- [ ] stock overview
- [ ] stock summary
- [ ] movement/drilldown

---

# 7. Integration Checklist

## 7.1 PR → PO
- [ ] approved PR can create PO
- [ ] linked PO visible in PR

## 7.2 PO → GRN
- [ ] approved PO allows GRN creation
- [ ] receipt progress updates
- [ ] linked GRN visible in PO

## 7.3 GRN → QC
- [ ] QC required items go to QC
- [ ] QC queue shows correct records

## 7.4 QC → Stock
- [ ] accepted stock increases inventory
- [ ] rejected stock not counted as available
- [ ] QC hold stock visible separately

---

# 8. Workflow Validation Checklist

- [ ] cannot approve draft PR
- [ ] cannot create GRN from unapproved PO
- [ ] cannot bypass QC if required
- [ ] cannot accept/reject GRN without correct role
- [ ] cannot mix accepted/rejected qty incorrectly
- [ ] cannot show invalid actions for roles

---

# 9. Data Consistency Checklist

- [ ] PR totals match line items
- [ ] PO totals match line items
- [ ] GRN qty matches PO remaining qty rules
- [ ] QC accepted + rejected = received
- [ ] stock reflects GRN/QC results correctly

---

# 10. Performance Checklist

- [ ] list pages load within acceptable time
- [ ] detail pages load efficiently
- [ ] no unnecessary refetch loops
- [ ] API payloads are not excessively large
- [ ] pagination works

---

# 11. UAT Readiness Checklist

- [ ] all flows demonstrable end-to-end
- [ ] roles tested separately
- [ ] edge cases tested
- [ ] error messages understandable
- [ ] navigation intuitive
- [ ] no major blockers

---

# 12. Demo Readiness Checklist

- [ ] demo script prepared
- [ ] sample data ready
- [ ] all flows tested with demo data
- [ ] fallback plan ready if API fails

---

# 13. Go-Live Readiness (Internal Pilot)

- [ ] logs/monitoring available
- [ ] error tracking available
- [ ] rollback strategy defined
- [ ] user onboarding/training material ready

---

# 14. Completion Criteria

This vertical slice is COMPLETE when:

- design is fully implemented
- frontend is stable
- backend is stable
- integration is correct
- workflow is correct
- roles behave correctly
- data is consistent
- UAT passes

---

# 15. Final Output

At this stage you have:

- a working ERP slice
- production-level architecture
- reusable system foundation
- ready-to-scale module structure

---

# 16. Next Step

After this:

- extend inventory (transfer, adjustment)
- extend procurement (RFQ, vendor mgmt)
- add sales module
- add finance module
