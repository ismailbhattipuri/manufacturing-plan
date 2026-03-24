# Frontend Folder Structure Code Plan

## 1. Purpose
This file defines the actual code-level folder structure for frontend implementation of the ERP system.

It converts the planning documents into a scalable frontend code organization so development can begin cleanly.

This structure should support:
- module-based development
- reusable shared components
- route-based page organization
- service/API separation
- permission logic reuse
- scalable state management
- future expansion into more ERP modules without major restructuring

---

## 2. Recommended Folder Location
Keep this planning file at:

`frontend/frontend_folder_structure_code_plan.md`

Use the structure below as the actual frontend code organization blueprint.

---

## 3. Core Frontend Architecture Direction
Use a module-first frontend code structure.

### Main Rules
- group business pages by module
- group shared components centrally
- keep page code separate from reusable components
- keep API services separate from page rendering
- keep permission helpers centralized
- keep route definitions centralized but module-aware
- keep business-specific hooks close to their module when possible

---

## 4. Recommended High-Level Code Structure

```text
src/
  app/
    router/
    providers/
    layouts/
    guards/

  modules/
    procurement/
    inventory/
    quality/
    master-data/
    sales/
    dispatch/
    finance/
    reports/
    admin/

  shared/
    components/
    layout/
    workflow/
    collaboration/
    forms/
    tables/
    feedback/
    utils/
    constants/
    types/

  services/
    api/
    auth/
    files/
    dashboard/

  hooks/
  store/                  # optional, only for app-wide state if needed
  config/
  assets/
```

---

## 5. App Layer Structure

### `src/app/router/`
Purpose:
- central route registration
- route metadata
- module route composition
- query param schema helpers if used

Suggested files:
```text
src/app/router/
  index.ts
  routeMap.ts
  routeGuards.ts
  routeTypes.ts
```

### `src/app/providers/`
Purpose:
- app-wide providers
- query provider
- theme provider
- auth provider
- notification provider

Suggested files:
```text
src/app/providers/
  AppProviders.tsx
  QueryProvider.tsx
  AuthProvider.tsx
```

### `src/app/layouts/`
Purpose:
- top-level app shell layout
- authenticated layout
- minimal layout for login/error pages if needed

Suggested files:
```text
src/app/layouts/
  AppShell.tsx
  AuthenticatedLayout.tsx
```

### `src/app/guards/`
Purpose:
- role/permission route guards
- fallback / no-access handling

Suggested files:
```text
src/app/guards/
  RequireAuth.tsx
  RequirePermission.tsx
  RequireRole.tsx
```

---

## 6. Module Structure Standard
Each business module should follow a consistent internal structure.

### Standard Module Pattern
```text
src/modules/{module-name}/
  pages/
  components/
  hooks/
  services/
  routes/
  types/
  constants/
  utils/
```

This keeps module-specific logic together.

---

# 7. Procurement Module Code Structure

## Recommended Structure
```text
src/modules/procurement/
  pages/
    PRListPage.tsx
    PRDetailPage.tsx
    POListPage.tsx
    PODetailPage.tsx
    GRNEntryPage.tsx
    GRNDetailPage.tsx
    IncomingQCPage.tsx

  components/
    PR/
      PRSummarySection.tsx
      PRLineItemsTable.tsx
      PRLinkedPOPanel.tsx
    PO/
      POSourceReferenceCard.tsx
      POVendorCommercialSummary.tsx
      POTotalsSummary.tsx
      POReceiptProgressWidget.tsx
      POLinkedGRNPanel.tsx
    GRN/
      GRNReceiptSummary.tsx
      GRNQCPanel.tsx
      GRNStockImpactPanel.tsx
      GRNLinkedRecordsPanel.tsx
    QC/
      IncomingQCQueueTable.tsx
      InspectionActionPanel.tsx

  hooks/
    usePRList.ts
    usePRDetail.ts
    usePOList.ts
    usePODetail.ts
    useGRNEntry.ts
    useGRNDetail.ts
    useIncomingQC.ts

  services/
    procurementService.ts
    grnService.ts
    qcService.ts

  routes/
    procurementRoutes.tsx

  types/
    pr.types.ts
    po.types.ts
    grn.types.ts
    qc.types.ts

  constants/
    procurementStatus.ts
    procurementPermissions.ts

  utils/
    procurementMappers.ts
    procurementHelpers.ts
```

---

# 8. Inventory Module Code Structure

## Recommended Structure
```text
src/modules/inventory/
  pages/
    StockOverviewPage.tsx

  components/
    Stock/
      StockSummaryStrip.tsx
      StockOverviewTable.tsx
      StockDrilldownDrawer.tsx
      StockStatusBadge.tsx

  hooks/
    useStockOverview.ts
    useStockDrilldown.ts

  services/
    inventoryService.ts

  routes/
    inventoryRoutes.tsx

  types/
    stock.types.ts
    inventoryMovement.types.ts

  constants/
    stockStatus.ts

  utils/
    inventoryHelpers.ts
```

---

# 9. Shared Component Structure

## `src/shared/components/`
Purpose:
- generic reusable visual components

Suggested structure:
```text
src/shared/components/
  PageHeader/
    PageHeader.tsx
  ActionBar/
    ActionBar.tsx
  SummaryCardStrip/
    SummaryCardStrip.tsx
  ConfirmDialog/
    ConfirmDialog.tsx
  StatusBadge/
    StatusBadge.tsx
```

---

## `src/shared/layout/`
Purpose:
- shell and navigation related shared components

Suggested structure:
```text
src/shared/layout/
  SidebarNav.tsx
  Topbar.tsx
  Breadcrumbs.tsx
```

---

## `src/shared/workflow/`
Purpose:
- workflow/status/linked-record reusable pieces

Suggested structure:
```text
src/shared/workflow/
  RecordStatusHeader.tsx
  ApprovalTimeline.tsx
  LinkedRecordsPanel.tsx
  RejectReasonModal.tsx
```

---

## `src/shared/collaboration/`
Purpose:
- comments, attachments, audit, activity

Suggested structure:
```text
src/shared/collaboration/
  CommentThreadPanel.tsx
  AttachmentPanel.tsx
  AuditTrailPanel.tsx
```

---

## `src/shared/forms/`
Purpose:
- shared form wrappers and field helpers

Suggested structure:
```text
src/shared/forms/
  FormSection.tsx
  StickyActionBar.tsx
  FormErrorSummary.tsx
```

---

## `src/shared/tables/`
Purpose:
- reusable table/grid behavior

Suggested structure:
```text
src/shared/tables/
  DataTable.tsx
  RowActionMenu.tsx
  TableToolbar.tsx
```

---

## `src/shared/feedback/`
Purpose:
- empty, loading, error, no-access UI

Suggested structure:
```text
src/shared/feedback/
  EmptyState.tsx
  ErrorState.tsx
  LoadingSkeleton.tsx
  NoAccessState.tsx
```

---

## `src/shared/utils/`
Purpose:
- formatting and helper utilities shared across modules

Examples:
```text
src/shared/utils/
  formatDate.ts
  formatCurrency.ts
  formatNumber.ts
  buildQueryString.ts
```

---

## `src/shared/types/`
Purpose:
- truly cross-module shared types only

Examples:
```text
src/shared/types/
  pagination.types.ts
  api.types.ts
  auth.types.ts
  common-status.types.ts
```

---

# 10. Services Layer Structure

## `src/services/api/`
Purpose:
- base API client
- interceptors
- request helpers
- response normalization

Suggested files:
```text
src/services/api/
  apiClient.ts
  apiErrorHandler.ts
  requestHelpers.ts
```

## `src/services/auth/`
Purpose:
- authentication/session helpers

Suggested files:
```text
src/services/auth/
  authService.ts
  tokenStorage.ts
```

## `src/services/files/`
Purpose:
- shared attachment/file upload/download logic

Suggested files:
```text
src/services/files/
  fileService.ts
```

## `src/services/dashboard/`
Purpose:
- dashboard-level services if shared across modules later

---

# 11. Hooks Strategy

## Shared hooks in `src/hooks/`
Use only for broad reusable hooks such as:
```text
src/hooks/
  useDebounce.ts
  useQueryParams.ts
  usePermission.ts
  useConfirmDialog.ts
```

## Module-specific hooks
Prefer keeping business hooks close to modules, for example:
- `src/modules/procurement/hooks/usePODetail.ts`
- `src/modules/inventory/hooks/useStockOverview.ts`

### Rule
If a hook is business-domain-specific, keep it inside the module.

---

# 12. Types Strategy

## Module types stay inside module
Examples:
- PR payloads
- PO detail view models
- GRN entry forms
- QC queue rows
- stock row structures

## Shared types stay shared only if reused broadly
Examples:
- pagination metadata
- API error response
- current user session
- option/select item

### Important Rule
Do not move everything into one giant global `types/` folder.

---

# 13. Route File Strategy

## Each module owns its route declarations
Examples:
- `src/modules/procurement/routes/procurementRoutes.tsx`
- `src/modules/inventory/routes/inventoryRoutes.tsx`

## App router composes them centrally
Example:
```text
src/app/router/index.ts
```

This file imports route groups from modules and assembles the final route tree.

---

# 14. Permission Logic Placement

## Shared permission helper layer
Recommended locations:
```text
src/hooks/usePermission.ts
src/shared/utils/permissionHelpers.ts
src/modules/procurement/constants/procurementPermissions.ts
```

### Suggested split
- generic permission evaluation helpers in shared
- module-specific action matrices/constants near the module

### Example
- shared helper checks role + action + record status
- module constant lists the supported action names

---

# 15. Query / Mutation Placement

## Query hooks should live near modules
Examples:
- `usePRList.ts`
- `usePODetail.ts`
- `useGRNDetail.ts`
- `useIncomingQC.ts`
- `useStockOverview.ts`

## Base client stays shared
Examples:
- `apiClient.ts`
- shared error handler
- token handling

### Important Rule
Do not place all business queries into one huge global file.

---

# 16. Suggested File Naming Rules
Use clear, predictable naming.

### Pages
- `PRListPage.tsx`
- `PODetailPage.tsx`
- `GRNEntryPage.tsx`

### Hooks
- `usePRDetail.ts`
- `useStockOverview.ts`

### Components
- `RecordStatusHeader.tsx`
- `POVendorCommercialSummary.tsx`
- `GRNStockImpactPanel.tsx`

### Services
- `procurementService.ts`
- `inventoryService.ts`

### Types
- `pr.types.ts`
- `stock.types.ts`

---

# 17. Suggested Colocation Rules

## Keep together
- module page
- module components
- module hooks
- module service
- module types

## Keep separate
- generic shared visual components
- base API client
- app router shell
- auth/session infrastructure

### Good example
`PODetailPage.tsx` should live near:
- `POVendorCommercialSummary.tsx`
- `usePODetail.ts`
- `po.types.ts`

---

# 18. Optional Store Layer Guidance
Use `src/store/` only for true global state, such as:
- authenticated user session snapshot
- app-wide notification counts
- global UI preferences
- maybe current organization context later

### Do not put into global store unnecessarily:
- page filters
- record details
- workflow page data
- comments/attachments lists

Those should remain in query/page state.

---

# 19. Example Full Slice Structure

```text
src/
  app/
    router/
      index.ts
      routeMap.ts
    providers/
      AppProviders.tsx
      QueryProvider.tsx
      AuthProvider.tsx
    layouts/
      AppShell.tsx
    guards/
      RequireAuth.tsx
      RequirePermission.tsx

  modules/
    procurement/
      pages/
        PRListPage.tsx
        PRDetailPage.tsx
        POListPage.tsx
        PODetailPage.tsx
        GRNEntryPage.tsx
        GRNDetailPage.tsx
        IncomingQCPage.tsx
      components/
        PR/
        PO/
        GRN/
        QC/
      hooks/
      services/
      routes/
      types/
      constants/
      utils/

    inventory/
      pages/
        StockOverviewPage.tsx
      components/
        Stock/
      hooks/
      services/
      routes/
      types/
      constants/
      utils/

  shared/
    components/
    layout/
    workflow/
    collaboration/
    forms/
    tables/
    feedback/
    utils/
    constants/
    types/

  services/
    api/
    auth/
    files/

  hooks/
  config/
  assets/
```

---

# 20. Build Order Recommendation for Codebase Setup

## Phase 1
- app shell
- router
- providers
- API client
- auth/session
- shared page/table/dialog components

## Phase 2
- procurement module folder scaffolding
- inventory module folder scaffolding
- route files
- service files
- types/constants/utils

## Phase 3
- PR pages
- PO pages
- GRN pages
- QC page
- Stock Overview page

## Phase 4
- shared workflow/collaboration panels
- drilldowns
- polish / refactor / consistency enforcement

---

# 21. Anti-Patterns to Avoid
- dumping all pages into a single `pages/` root folder
- putting all hooks in one global folder
- putting all API calls in one giant service file
- mixing shared and domain-specific components randomly
- putting workflow logic inside generic presentational components
- using global store for everything

---

# 22. Completion Criteria
This file is complete when:
- frontend code folders are defined
- module and shared boundaries are clear
- route/service/hook/component placement is clear
- naming strategy is stable
- initial codebase setup order is clear

---

# 23. Next Related Files
Recommended next files:
1. frontend_build_sequence_sprint_plan.md
2. backend_endpoint_priority_plan.md
3. frontend_module_bootstrap_checklist.md
4. coding_standards_frontend.md
