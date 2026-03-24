# Frontend Module Bootstrap Checklist

## 1. Purpose
This file is the practical startup checklist for beginning frontend development on the ERP system.

It converts the planning package into a real implementation checklist so the team can start coding without missing foundational steps.

This checklist should be used:
- before frontend development starts
- when bootstrapping a new module
- before declaring a module ready for page implementation
- during lead review of engineering readiness

---

## 2. Recommended Folder Location
Keep this file at:

`frontend/frontend_module_bootstrap_checklist.md`

---

## 3. Scope Covered
This checklist supports:
- initial frontend app setup
- first ERP vertical slice bootstrap
- future module bootstrap reuse

Current active slice:
- Procurement
- Incoming QC
- Inventory Stock Overview

---

## 4. Bootstrap Principle
A module is not ready for actual page development until:
- route structure exists
- service layer exists
- permission logic exists
- query keys exist
- shared components needed by the module exist
- basic mock/live data contract is agreed
- page placeholders are registered
- loading/error/no-access states are accounted for

---

# 5. Global Frontend Bootstrap Checklist

## 5.1 Application Foundation
Confirm the following are ready:

- [ ] app shell created
- [ ] authenticated layout created
- [ ] sidebar navigation exists
- [ ] topbar exists
- [ ] breadcrumb support exists
- [ ] route provider configured
- [ ] query/data provider configured
- [ ] auth/session provider configured
- [ ] base API client configured
- [ ] global error handling pattern agreed
- [ ] no-access screen/component exists
- [ ] empty/loading/error feedback components exist

---

## 5.2 Shared Frontend Infrastructure
Confirm the following are ready:

- [ ] `PageHeader`
- [ ] `FilterBar`
- [ ] `DataTable`
- [ ] `StatusBadge`
- [ ] `ConfirmDialog`
- [ ] `RejectReasonModal`
- [ ] `RecordStatusHeader`
- [ ] `ActionBar`
- [ ] `CommentThreadPanel`
- [ ] `AttachmentPanel`
- [ ] `AuditTrailPanel`
- [ ] shared table pagination pattern
- [ ] shared query key naming pattern
- [ ] shared mutation error handling pattern
- [ ] permission helper baseline implemented

---

## 5.3 Routing Foundation
Confirm the following are ready:

- [ ] frontend route map exists
- [ ] central app router composes module routes
- [ ] route guards exist
- [ ] query param parsing strategy exists
- [ ] create/detail/list route pattern is standardized
- [ ] route metadata pattern exists for title/breadcrumb/permission

---

## 5.4 Backend Integration Foundation
Confirm the following are ready:

- [ ] base response envelope agreed
- [ ] validation error shape agreed
- [ ] auth/me endpoint available or mocked reliably
- [ ] comments API pattern agreed
- [ ] attachments API pattern agreed
- [ ] audit API pattern agreed
- [ ] page/API dependency matrix available
- [ ] frontend service stub structure created

---

# 6. Module Bootstrap Standard
Every new module should complete these steps before full page development starts.

## 6.1 Module Folder Created
- [ ] `src/modules/{module}/pages/`
- [ ] `src/modules/{module}/components/`
- [ ] `src/modules/{module}/hooks/`
- [ ] `src/modules/{module}/services/`
- [ ] `src/modules/{module}/routes/`
- [ ] `src/modules/{module}/types/`
- [ ] `src/modules/{module}/constants/`
- [ ] `src/modules/{module}/utils/`

## 6.2 Module Route File Created
- [ ] route file exists
- [ ] route file exported into app router
- [ ] placeholder page routes registered
- [ ] route guard requirements attached

## 6.3 Module Service Stubs Created
- [ ] base service file created
- [ ] list query functions stubbed
- [ ] detail query functions stubbed
- [ ] workflow mutation functions stubbed
- [ ] comments/attachments helper usage connected

## 6.4 Module Types Created
- [ ] list row type created
- [ ] detail payload type created
- [ ] mutation payload types created
- [ ] filter/query param type created
- [ ] shared status enum/constants created

## 6.5 Module Permission Constants Created
- [ ] page access constants defined
- [ ] action permission constants defined
- [ ] status-based action mapping reference created

## 6.6 Module Query Key Plan Created
- [ ] list query key
- [ ] summary query key if needed
- [ ] detail query key
- [ ] comments query key
- [ ] attachments query key
- [ ] audit query key
- [ ] linked-record query key

---

# 7. Procurement Module Bootstrap Checklist

## 7.1 Folder Structure
- [ ] `src/modules/procurement/pages/`
- [ ] `src/modules/procurement/components/PR/`
- [ ] `src/modules/procurement/components/PO/`
- [ ] `src/modules/procurement/components/GRN/`
- [ ] `src/modules/procurement/components/QC/`
- [ ] `src/modules/procurement/hooks/`
- [ ] `src/modules/procurement/services/`
- [ ] `src/modules/procurement/routes/`
- [ ] `src/modules/procurement/types/`
- [ ] `src/modules/procurement/constants/`
- [ ] `src/modules/procurement/utils/`

## 7.2 Pages Registered
- [ ] `PRListPage.tsx`
- [ ] `PRDetailPage.tsx`
- [ ] `POListPage.tsx`
- [ ] `PODetailPage.tsx`
- [ ] `GRNEntryPage.tsx`
- [ ] `GRNDetailPage.tsx`
- [ ] `IncomingQCPage.tsx`

## 7.3 Service Files Stubbed
- [ ] `procurementService.ts`
- [ ] `grnService.ts`
- [ ] `qcService.ts`

## 7.4 Hooks Stubbed
- [ ] `usePRList.ts`
- [ ] `usePRDetail.ts`
- [ ] `usePOList.ts`
- [ ] `usePODetail.ts`
- [ ] `useGRNEntry.ts`
- [ ] `useGRNDetail.ts`
- [ ] `useIncomingQC.ts`

## 7.5 Constants Ready
- [ ] procurement statuses
- [ ] PR action map
- [ ] PO action map
- [ ] GRN action map
- [ ] QC action map

## 7.6 Core Components Ready
- [ ] `PRSummarySection`
- [ ] `PRLineItemsTable`
- [ ] `PRLinkedPOPanel`
- [ ] `POSourceReferenceCard`
- [ ] `POVendorCommercialSummary`
- [ ] `POTotalsSummary`
- [ ] `POReceiptProgressWidget`
- [ ] `POLinkedGRNPanel`
- [ ] `GRNReceiptSummary`
- [ ] `GRNQCPanel`
- [ ] `GRNStockImpactPanel`
- [ ] `InspectionActionPanel`

---

# 8. Inventory Module Bootstrap Checklist

## 8.1 Folder Structure
- [ ] `src/modules/inventory/pages/`
- [ ] `src/modules/inventory/components/Stock/`
- [ ] `src/modules/inventory/hooks/`
- [ ] `src/modules/inventory/services/`
- [ ] `src/modules/inventory/routes/`
- [ ] `src/modules/inventory/types/`
- [ ] `src/modules/inventory/constants/`
- [ ] `src/modules/inventory/utils/`

## 8.2 Pages Registered
- [ ] `StockOverviewPage.tsx`

## 8.3 Service Files Stubbed
- [ ] `inventoryService.ts`

## 8.4 Hooks Stubbed
- [ ] `useStockOverview.ts`
- [ ] `useStockDrilldown.ts`

## 8.5 Components Ready
- [ ] `StockSummaryStrip`
- [ ] `StockOverviewTable`
- [ ] `StockDrilldownDrawer`
- [ ] `StockStatusBadge`

---

# 9. Page Bootstrap Checklist
Before starting a specific page, confirm:

## 9.1 Design Inputs Exist
- [ ] page spec file exists
- [ ] related transaction file exists
- [ ] related role file exists
- [ ] connection/navigation rules exist
- [ ] API dependency is documented
- [ ] permission logic is documented

## 9.2 Route Exists
- [ ] page route registered
- [ ] breadcrumb/title metadata attached
- [ ] route guard attached if needed

## 9.3 Service Functions Exist
- [ ] list/detail fetch function exists
- [ ] required mutations exist
- [ ] comments/attachments/audit service calls exist if page needs them

## 9.4 Query Keys Exist
- [ ] query key names decided
- [ ] invalidation targets documented

## 9.5 Components Ready
- [ ] required shared components exist
- [ ] required domain components exist
- [ ] missing component list identified if not built yet

## 9.6 State Plan Exists
- [ ] server state identified
- [ ] local UI state identified
- [ ] derived UI state identified
- [ ] loading/error/no-access state identified

---

# 10. “Ready to Code” Checklist for a Page
A page is ready to start implementation when:

- [ ] route is defined
- [ ] page component file exists
- [ ] page service calls are stubbed or real
- [ ] page query keys are defined
- [ ] page permission rules are known
- [ ] required shared/domain components are available or planned
- [ ] response shape is agreed or mocked
- [ ] acceptance criteria are known

---

# 11. “Ready for Review” Checklist for a Page
A page is ready for internal engineering review when:

- [ ] role-based visibility works
- [ ] status-based actions work
- [ ] loading state exists
- [ ] empty state exists
- [ ] error state exists
- [ ] no-access state exists
- [ ] mutation refresh behavior works
- [ ] linked navigation works
- [ ] comments/attachments/audit work if applicable

---

# 12. “Ready for QA/UAT” Checklist for a Slice
The first vertical slice is ready for QA/UAT when:

- [ ] PR flow works end-to-end
- [ ] PO flow works end-to-end
- [ ] GRN flow works end-to-end
- [ ] QC flow works end-to-end
- [ ] Stock Overview reflects accepted/rejected/QC-hold outcomes
- [ ] role transitions behave correctly
- [ ] no critical stale-data issue remains
- [ ] no critical route/deep-link issue remains
- [ ] major workflow validation errors show correctly

---

# 13. Suggested Bootstrap Sequence (Immediate)
If engineering starts now, do this first:

1. create module folders
2. register routes
3. create service stubs
4. create types and constants
5. create query key constants
6. create page placeholders
7. create shared/domain component placeholders
8. wire basic permissions
9. start PR List
10. continue along the defined build sprint plan

---

# 14. Common Bootstrap Mistakes to Avoid
- starting page implementation before routes are registered
- building pages before service stubs exist
- missing query key conventions
- mixing module-specific code into shared folders too early
- skipping permission helper setup
- leaving comments/attachments/audit integration as “later” without placeholders
- not defining loading/error/no-access states from the start

---

# 15. Ownership Suggestion
## Frontend Lead
Should verify:
- folder structure
- route structure
- service pattern
- permission pattern
- shared component reuse

## Frontend Developers
Should own:
- page placeholders
- module-specific hooks/services
- page implementation
- component integration

## Backend Lead
Should verify:
- endpoint availability for active sprint
- payload shape readiness
- validation/error consistency

---

# 16. Completion Criteria
This file is complete when:
- bootstrap tasks are clearly itemized
- module startup steps are repeatable
- page readiness criteria are clear
- review/UAT readiness criteria are clear
- team can start coding without ambiguity

---

# 17. Next Related Files
Recommended next files:
1. coding_standards_frontend.md
2. first_vertical_slice_delivery_checklist.md
3. backend_validation_error_contract.md
4. frontend_dev_handoff_template.md
