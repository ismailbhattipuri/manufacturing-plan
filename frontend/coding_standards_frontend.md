# Coding Standards: Frontend

## 1. Purpose
This file defines the coding standards for frontend implementation of the ERP system.

It standardizes:
- naming conventions
- file organization discipline
- component design rules
- hook patterns
- service/API usage patterns
- state/query handling conventions
- error/loading handling
- permission and workflow logic placement
- review expectations

This file should be treated as the frontend engineering rulebook for the project.

---

## 2. Recommended Folder Location
Keep this file at:

`frontend/coding_standards_frontend.md`

---

## 3. Scope
These standards apply to:
- shared frontend code
- module code
- pages
- hooks
- services
- types
- route files
- utility functions
- workflow/action handling
- API integration

---

## 4. Core Engineering Principles

### Clarity over cleverness
Code should be easy to understand by another engineer without hidden logic.

### Predictability over improvisation
Use the defined folder structure, naming conventions, query patterns, and permission helpers consistently.

### Separate business logic from presentation
Do not bury workflow/business rules inside presentational UI components.

### Reuse before duplication
If the same UI or logic pattern appears in multiple places, extract it appropriately.

### Explicit workflow behavior
Approval, rejection, cancellation, QC, and stock-impact behavior must be represented clearly in code.

### Backend remains source of truth
Frontend should reflect permissions and workflow state, but should not invent authority beyond backend responses.

---

## 5. Naming Conventions

## 5.1 Files
Use clear PascalCase for React component files and camel/lowercase patterns for hooks/services/types as appropriate.

### Pages
- `PRListPage.tsx`
- `PRDetailPage.tsx`
- `PODetailPage.tsx`
- `GRNEntryPage.tsx`

### Components
- `PageHeader.tsx`
- `RecordStatusHeader.tsx`
- `POVendorCommercialSummary.tsx`

### Hooks
- `usePRDetail.ts`
- `usePOList.ts`
- `useIncomingQC.ts`

### Services
- `procurementService.ts`
- `inventoryService.ts`
- `qcService.ts`

### Types
- `pr.types.ts`
- `po.types.ts`
- `stock.types.ts`

### Utilities
- `formatCurrency.ts`
- `buildQueryString.ts`

---

## 5.2 Variables and Functions
- use descriptive names
- avoid cryptic abbreviations unless they are standard ERP/business abbreviations already used system-wide

Good:
- `selectedPurchaseOrderId`
- `isQCActionAllowed`
- `handleApprovePurchaseRequest`

Avoid:
- `selId`
- `doAct`
- `tmpRows`

---

## 5.3 Constants
Use UPPER_SNAKE_CASE for fixed values.

Examples:
- `DEFAULT_PAGE_SIZE`
- `PR_STATUS_APPROVED`
- `STOCK_STATUS_QC_HOLD`

Group module constants inside module constant files.

---

## 6. Component Standards

## 6.1 Component Types
Prefer these categories:

### Page Components
Own route-level composition and page state.

### Domain Components
Represent module/business concepts.

### Shared Presentational Components
Reusable UI building blocks.

### Workflow Components
Status/approval/linked-record controls.

---

## 6.2 Page Components Must
- orchestrate page-level queries
- own page-level local UI state
- wire actions and refresh behavior
- pass shaped props to child components
- not become giant unreadable files

### Rule
If a page exceeds manageable complexity, split into section components.

---

## 6.3 Shared Components Must
- remain reusable
- avoid embedding module-specific business assumptions unless explicitly designed as domain components
- accept props, not fetch business data directly unless clearly intended

---

## 6.4 Props Standards
- keep prop names explicit
- avoid prop overload
- prefer structured prop objects when a component naturally consumes a domain block
- do not pass unused props

Good:
- `recordStatus`
- `linkedRecords`
- `onApprove`
- `isEditable`

---

## 6.5 Component Size Guidance
If a component is:
- too large to scan comfortably
- mixing rendering and workflow logic heavily
- difficult to test in isolation

Then split it.

---

## 7. Hook Standards

## 7.1 Hook Purpose
Hooks should encapsulate:
- query wiring
- mutation wiring
- derived business UI state
- reusable page behavior

---

## 7.2 Hook Naming
Always start custom hooks with `use`.

Examples:
- `usePRList`
- `usePODetail`
- `useStockOverview`

---

## 7.3 Hook Placement
- module-specific hooks stay in the module
- broadly reusable hooks go in `src/hooks/`

Examples:
- `src/modules/procurement/hooks/usePODetail.ts`
- `src/hooks/useDebounce.ts`

---

## 7.4 Hook Rules
Hooks should:
- return clear, structured outputs
- avoid hidden side effects where possible
- not contain presentation markup
- not mutate server data manually outside intended mutation flows

Good outputs:
- `data`
- `isLoading`
- `isError`
- `actions`
- `derivedFlags`

---

## 8. Service Layer Standards

## 8.1 Service Responsibility
Service files should:
- call API endpoints
- normalize request/response handling where useful
- keep endpoint paths centralized
- not contain page rendering logic

---

## 8.2 Service Naming
Use domain-specific service names:
- `procurementService`
- `inventoryService`
- `qcService`

---

## 8.3 Service Function Naming
Use clear verb-based names:
- `getPRList`
- `getPRDetail`
- `submitPR`
- `approvePO`
- `createGRN`
- `getStockOverview`

Avoid vague names:
- `fetchData`
- `runAction`
- `updateThing`

---

## 8.4 Service Function Boundaries
A service function should map closely to a real endpoint or a small related endpoint group.

Do not create giant “god service” files with all APIs mixed together.

---

## 9. Query and Server-State Standards

## 9.1 Query Keys
Use structured query keys consistently.

Examples:
- `['pr', 'list', filters]`
- `['pr', 'detail', prId]`
- `['po', 'detail', poId]`
- `['grn', 'detail', grnId]`
- `['stock', 'overview', filters]`

---

## 9.2 Query Ownership
Pages or module hooks should own the queries for their screens.

Shared components should not silently fetch major business data unless designed for that purpose.

---

## 9.3 Invalidation Rules
After mutations:
- invalidate exact related query keys
- refetch current detail/list data as needed
- avoid global refetch storms

Good example:
After GRN accept:
- invalidate GRN detail
- invalidate PO detail receipt progress
- invalidate stock overview

---

## 9.4 Strict vs Optimistic Updates
Use strict refresh for:
- workflow transitions
- approval/rejection
- QC results
- stock-impact actions

Use optimistic behavior only where business risk is low:
- comments
- local UI preferences
- minor non-critical UX state

---

## 10. Local UI State Standards

## 10.1 Keep UI State Close to Use
Examples:
- modals
- selected rows
- active tabs
- draft form state
- filter state

Keep these in page/component state unless there is a strong reason to lift them.

---

## 10.2 Do Not Abuse Global Store
Do not put into global store:
- page filters
- record detail payloads
- per-page modal state
- comments list for one page

Reserve global state for truly app-wide concerns.

---

## 11. Permission and Workflow Logic Standards

## 11.1 Use Permission Helpers
All role/status-based action visibility should go through clear helper functions or selectors.

Examples:
- `canApprovePR(user, record)`
- `canCreateGRN(user, poRecord)`
- `canRejectGRN(user, grnRecord)`

---

## 11.2 Do Not Scatter Permission Logic Randomly
Avoid repeating inline logic across many components like:
- `role === 'manager' && status === 'submitted' && ...`

Instead centralize in helper utilities.

---

## 11.3 Maker-Checker Logic
Workflow-sensitive roles must be enforced clearly in UI behavior.

Examples:
- maker should not see approve action
- QA should not see PO approval action
- store user should not see procurement commercial edit controls

---

## 12. Error Handling Standards

## 12.1 Error Types to Handle
- page load error
- section load error
- action/mutation error
- validation error
- no-access error

---

## 12.2 Error Display Rules
- page-level fetch errors → `ErrorState`
- section-level failures → local section error block if page can still function
- mutation errors → action-level alert/message
- validation errors → inline field or form summary

---

## 12.3 Do Not Swallow Errors
Never hide backend failures silently. Always surface them appropriately.

---

## 13. Loading and Empty State Standards

## 13.1 Loading
Support:
- full page loading
- section loading
- action loading
- table loading

Use skeletons where useful.

---

## 13.2 Empty States
Every list/detail-supporting section should define:
- no records
- no linked records
- no comments
- no attachments
- no stock rows for current filter

---

## 14. Form Standards

## 14.1 Validation
- validate before submit where possible
- map backend validation errors clearly
- disable unsafe submit when obvious blocking validation fails

---

## 14.2 Form Mode Rules
- editable only when role + status allow it
- read-only detail mode must be visually clear
- draft form and review mode should not feel identical

---

## 14.3 Long Forms
Use:
- section grouping
- sticky action bars
- unsaved changes warning
- inline and summary validation messaging

---

## 15. Comments, Attachments, and Audit Standards

## 15.1 Comments
- comment panel should be consistent across pages
- distinguish general comment vs workflow note where applicable
- preserve comment timestamps and authors

## 15.2 Attachments
- attachment behavior should be consistent across pages
- file metadata should display clearly
- do not hide upload failures

## 15.3 Audit
- audit panel should show event, actor, time, description
- use same rendering pattern across modules

---

## 16. Routing Standards

## 16.1 Route-Page Alignment
Each route should map to one page component clearly.

## 16.2 Query Params
Use query params for:
- filters
- source context
- pagination
- sort
- workflow-specific list views

## 16.3 Guarded Routes
Page routes must use appropriate guard rules.

---

## 17. Type Safety Standards

## 17.1 Define Types Explicitly
Use explicit interfaces/types for:
- API responses
- list rows
- detail payloads
- mutation payloads
- form state
- filter params

## 17.2 Avoid `any`
Do not use `any` unless there is a genuinely temporary reason and it is tracked for cleanup.

## 17.3 Separate Raw API Types from View Models When Needed
If backend payload shape is not ideal for UI composition, map it cleanly rather than spreading transformation logic everywhere.

---

## 18. Utility Standards

## 18.1 Utilities Should Be Pure
Formatting/build helpers should avoid side effects.

## 18.2 Good Utility Candidates
- date formatting
- currency formatting
- query string building
- status label mapping
- quantity formatting

## 18.3 Avoid Domain Logic Creep
Do not turn utility files into hidden business-rule containers.

---

## 19. Review Standards

## 19.1 Every PR/MR Should Be Reviewed For
- naming clarity
- folder placement correctness
- duplication avoidance
- permission logic correctness
- query invalidation correctness
- loading/error/empty state coverage
- role/status action correctness

## 19.2 Reviewer Should Ask
- Is this logic in the right layer?
- Is this reusable enough?
- Is workflow logic centralized?
- Will another engineer understand this in 3 months?
- Does this respect the defined module structure?

---

## 20. Testing Standards

## 20.1 Prioritize Testing For
- permission helper logic
- action visibility rules
- workflow state transitions in UI
- query invalidation behavior
- critical form validation
- deep-link route handling

## 20.2 Manual QA Must Cover
- maker/checker separation
- PO → GRN flow
- GRN → QC flow
- QC → stock visibility flow
- stale-data edge cases after mutation

---

## 21. Documentation Standards
When adding a major page or domain behavior:
- keep related planning docs aligned if architecture changes
- update route map if route changes
- update API dependency matrix if page/API dependency changes
- update permission matrix if role/action behavior changes

---

## 22. Anti-Patterns to Avoid
- giant page components with everything inline
- duplicated permission conditions everywhere
- mixing API calls into presentational child components carelessly
- using global store for page-local state
- hiding workflow assumptions in UI labels only
- hardcoding status/action strings in many places
- skipping error/loading/empty states
- building one-off components for patterns already repeated elsewhere

---

## 23. Definition of Done for Frontend Code
A frontend feature is not done until:
- code is in the correct folder
- naming follows standards
- role/status logic works
- loading/error/empty/no-access states exist
- queries/mutations are wired correctly
- refresh behavior works
- review passes
- related planning docs are updated if implementation changed architecture

---

## 24. Completion Criteria
This file is complete when:
- coding rules are explicit
- component/hook/service/query standards are clear
- review expectations are defined
- anti-patterns are documented
- definition of done is clear

---

## 25. Next Related Files
Recommended next files:
1. first_vertical_slice_delivery_checklist.md
2. backend_validation_error_contract.md
3. frontend_dev_handoff_template.md
4. backend_dev_handoff_template.md
