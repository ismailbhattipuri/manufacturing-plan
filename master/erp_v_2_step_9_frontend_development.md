# ERP V2 – Step 9: Frontend Development

## 1. Objective
This document defines the frontend development layer for the ERP V2 system. It translates the approved planning, module structure, roles, workflows, screen map, wireframe direction, database expectations, and backend API contracts into an implementation-ready frontend architecture.

Step 9 exists to ensure the ERP UI is not treated as only a set of screens, but as a structured application layer with:
- consistent page behavior
- role-aware rendering
- workflow-aware actions
- reusable UI patterns
- predictable API integration
- audit-friendly user interactions
- scalable module-based frontend organization

This step is the bridge between high-level ERP design and detailed page-wise frontend execution.

---

## 2. Position of Step 9 in the ERP Design Flow
The ERP planning sequence now becomes:

1. Step 1 – Planning (What, Why, Who)
2. Step 2 – Module Design
3. Step 3 – User Role Design
4. Step 4 – Process Flow Design
5. Step 5 – Screen Map
6. Step 6 – Wireframe Design
7. Step 7 – Database Design
8. Step 8 – Backend API
9. Step 9 – Frontend Development

Step 9 must consume the previous steps and turn them into a frontend system that is modular, connected, role-aware, and implementation-ready.

---

## 3. Purpose of This Document
This document defines:
- frontend architecture principles
- module-based UI implementation structure
- page rendering standards
- routing and navigation rules
- role and permission-based frontend behavior
- page-to-page and record-to-record connection behavior
- API integration rules using Step 8
- design system and reusable UI component strategy
- form, table, workflow, approval, dashboard, and reporting UI patterns
- frontend state, validation, and error-handling rules
- expected outputs before detailed page sub-files are created

This is a master frontend strategy document. It is not the replacement for page-level specs. Those detailed sub-files will be created after this step.

---

## 4. Source Inputs from Previous Steps
Step 9 depends on all previously approved files.

### 4.1 Step 1 – Planning
Provides:
- ERP scope
- business goals
- stakeholders and user groups
- overall module list
- end-to-end business cycle
- design principles

Frontend impact:
- determines the overall ERP shell
- determines which business areas must exist in navigation
- defines the major user personas the UI must support

### 4.2 Step 2 – Module Design
Provides:
- module objectives
- key entities
- page inventory
- high-level actions
- module dependencies

Frontend impact:
- determines module-wise route groups
- determines module dashboards, list screens, create screens, and detail screens
- determines what major actions must appear on the UI

### 4.3 Step 3 – User Role Design
Provides:
- access model
- role-module-permission relationships
- user-role assignment logic
- approval-oriented permission structure

Frontend impact:
- determines page visibility
- determines action button visibility
- determines section-level and field-level editability
- determines role-based dashboards and role-based navigation

### 4.4 Step 4 – Process Flow Design
Provides:
- transaction flow logic
- approval states
- state transitions
- dependencies and system impacts

Frontend impact:
- determines status badges and workflow timelines
- determines which actions appear at which status
- determines conversion actions between related documents
- determines exception actions and approval actions

### 4.5 Step 5 – Screen Map
Provides:
- page inventory
- high-level navigation and page grouping

Frontend impact:
- determines app navigation structure
- determines route naming and module organization
- determines cross-screen access paths

### 4.6 Step 6 – Wireframe Design
Provides:
- layout direction
- visual hierarchy expectations
- page composition strategy
- reusable section ideas

Frontend impact:
- determines how lists, forms, drawers, tabs, and detail layouts should be organized
- guides screen consistency across modules

### 4.7 Step 7 – Database Design
Provides:
- domain model direction
- entity structures
- relationships
- system-wide support structures

Frontend impact:
- informs how data is grouped and displayed
- informs master-detail relationships
- informs attachment, comment, audit, notification, and workflow displays

### 4.8 Step 8 – Backend API
Provides:
- endpoint domains
- request/response structures
- workflow endpoints
- auth and authorization model
- list/filter/search conventions
- error model

Frontend impact:
- defines all frontend service integration patterns
- defines how forms submit data
- defines how tables fetch/filter/paginate data
- defines how workflow actions are triggered from the UI

---

## 5. Core Frontend Development Principles
The frontend must follow these principles:

1. **Module-first architecture**  
   The frontend should be organized by business module, not by isolated screen files only.

2. **Role-aware rendering**  
   The UI must respect module access, page access, action access, and future scope-based access.

3. **Workflow-aware interactions**  
   Buttons, forms, timelines, badges, and linked actions must reflect real process state.

4. **Consistent page patterns**  
   List pages, create pages, detail pages, approval pages, dashboards, and reports should follow repeatable structures.

5. **API-contract-driven implementation**  
   The frontend should align to Step 8 service contracts, not invent separate field logic.

6. **Reusable UI system**  
   Shared components must be used for tables, forms, filters, status chips, approval bars, timelines, attachments, comments, and record references.

7. **Business traceability**  
   Each transaction page should clearly show source documents, downstream documents, status history, comments, attachments, and audit references where applicable.

8. **Scalable maintainability**  
   The system must support future deep page files and role files without requiring major restructuring.

---

## 6. Frontend Scope of Step 9
Step 9 covers frontend planning and implementation structure for:
- ERP application shell
- authentication-aware app loading
- role-aware navigation
- module routing
- page types and page composition
- reusable UI components
- shared state patterns
- API integration patterns
- error/loading/empty state standards
- workflow action handling
- record linkage and cross-page behavior
- dashboard and reporting presentation structure
- approval and task queues
- audit, comments, attachments, and activity display

Step 9 does **not** yet produce the final page-by-page deep specification. That comes immediately after this step in sub-files.

---

## 7. Recommended Frontend Application Architecture

### 7.1 Application Layers
The frontend should be designed in the following logical layers:

#### A. App Shell Layer
Responsible for:
- login session handling
- app bootstrapping
- layout shell
- sidebar/topbar/footer patterns
- branch/plant/warehouse context selectors where applicable
- notification area
- global search entry point

#### B. Module Layer
Responsible for:
- grouping routes and screens by business module
- exposing module dashboard, list, create, detail, and action screens
- reusing shared components within a domain

#### C. Shared Business Component Layer
Responsible for reusable ERP-specific components such as:
- status badges
- approval bars
- audit panels
- comments panel
- attachment uploader/viewer
- document reference card
- line item grid
- stock summary widget
- workflow timeline
- KPI cards

#### D. Shared Technical Component Layer
Responsible for generic components such as:
- table
- form field wrappers
- modal
- drawer
- tabs
- filters
- paginator
- empty state
- error state
- skeleton loader
- toast/alert system

#### E. Service and Data Layer
Responsible for:
- API clients
- module service files
- query/mutation hooks
- request/response mapping
- auth token handling
- optimistic/local UI state where appropriate

---

## 8. Recommended Frontend Folder and Module Strategy
A module-based structure is recommended.

```text
src/
  app/
    router/
    providers/
    layouts/
  modules/
    master-data/
    procurement/
    inventory/
    production/
    quality/
    sales/
    dispatch/
    finance/
    reports/
    admin/
  shared/
    components/
    forms/
    tables/
    layout/
    feedback/
    workflow/
    files/
    charts/
  services/
    api/
    auth/
    permissions/
  hooks/
  utils/
  constants/
  types/
```

### Module-Level Internal Pattern
Each module should generally contain:
- routes
- pages
- components
- service hooks
- types/interfaces
- mappers/adapters
- constants
- permission helpers

Example:

```text
modules/procurement/
  routes/
  pages/
    pr-list/
    pr-create/
    pr-detail/
    po-list/
    po-create/
    po-detail/
    grn-entry/
  components/
  services/
  types/
  permissions/
```

---

## 9. Frontend Technology and Implementation Direction
The exact technology stack can be finalized by engineering, but the architecture should support:
- component-based SPA frontend
- route-driven module navigation
- form/state management for large transactional forms
- server-state data fetching and caching
- reusable design system implementation
- file upload support
- tabular and line-item heavy interfaces
- approval/action workflows

### Recommended Technical Direction
- modern component-driven frontend framework
- centralized route system
- shared UI component library
- server-state query layer
- schema-aware form validation layer
- role and permission utility layer

This document intentionally focuses on the architectural standard rather than locking to one framework prematurely.

---

## 10. ERP Application Shell Design

### 10.1 Main Layout Regions
The ERP frontend should support the following regions:
- left sidebar navigation
- top header bar
- breadcrumb area
- page title + action bar
- page body area
- optional right-side contextual panel for comments, activity, or linked records

### 10.2 Header Responsibilities
Header may include:
- current company/branch/plant context
- global search
- task/approval notifications
- user profile menu
- quick create actions (role-permitted only)

### 10.3 Sidebar Responsibilities
Sidebar should:
- show only modules visible to the current user role
- support grouped menu sections
- support child pages when needed
- highlight active module and current page

### 10.4 Breadcrumb Standard
All major transactional and master pages should use breadcrumb navigation.

Example:
- Procurement / Purchase Requisition / PR-000145
- Inventory / Stock Transfer / ST-000089
- Production / Work Orders / WO-000221

---

## 11. Navigation and Route Design

### 11.1 Route Philosophy
Routes must be stable, readable, and aligned to Step 5 screen map.

Examples:
- `/master-data/items`
- `/master-data/items/:id`
- `/procurement/purchase-requests`
- `/procurement/purchase-requests/new`
- `/procurement/purchase-requests/:id`
- `/production/work-orders`
- `/production/work-orders/:id`
- `/quality/incoming-qc/:id`

### 11.2 Route Types
Each module should support route classes such as:
- dashboard route
- list route
- create route
- detail route
- edit route where allowed
- task/approval queue route
- report route

### 11.3 Record Conversion Routes
Where one document creates another, the route system should preserve source context.

Examples:
- PR Detail → Convert to PO
- PO Detail → Receive Goods (GRN)
- Work Order Detail → Issue Materials
- SO Detail → Dispatch Planning

The frontend should pass source record context intentionally and display source references clearly.

---

## 12. Role and Permission Behavior in the Frontend
The frontend must consume Step 3 access logic and Step 8 auth/access APIs.

### 12.1 UI Enforcement Areas
Permission handling should affect:
- visible modules
- visible pages
- visible action buttons
- editable fields
- bulk action availability
- export/print availability
- approval/reject buttons
- comments/attachment permissions if restricted

### 12.2 Permission Levels to Support
The frontend should be capable of handling:
- module access
- page access
- action access
- status-based action availability
- future branch/department/warehouse data scope limitations

### 12.3 Important Rule
Frontend permission checks improve UX, but backend authorization remains the final source of truth.

---

## 13. Standard Page Types
The ERP frontend should standardize page types.

### 13.1 Dashboard Page
Contains:
- KPI cards
- pending approvals/tasks
- shortcuts
- recent transactions
- exception alerts
- chart/report widgets

### 13.2 List Page
Contains:
- page title and primary actions
- filters/search
- table/grid
- pagination
- row actions
- bulk actions if allowed
- export option where permitted

### 13.3 Create Page
Contains:
- structured entry form
- header fields
- line item section if relevant
- validation messages
- draft/save/submit actions
- attachment section if required

### 13.4 Detail Page
Contains:
- summary header
- key metadata
- status badge
- available actions by status and role
- tabs/sections for line items, comments, attachments, audit, linked records
- workflow timeline/history

### 13.5 Approval Queue Page
Contains:
- assigned/pending items
- filters by module, date, priority, status
- quick review details
- approve/reject/escalate actions where allowed

### 13.6 Report Page
Contains:
- filter panel
- result grid/chart
- export options
- drill-down links to source records where useful

---

## 14. Standard Page Composition Rules
Every page should define the following when detailed sub-files are created:
- page purpose
- route
- primary users/roles
- entry points
- page type
- primary actions
- data sources
- layout sections
- field groups
- table columns
- validation rules
- status behavior
- role behavior
- linked pages
- error/loading/empty states

This document defines the standard, while later sub-files will instantiate it per page.

---

## 15. Reusable UI Component System
The frontend should build a reusable ERP component library.

### 15.1 Core Shared Components
- AppShell
- SidebarNav
- Topbar
- Breadcrumbs
- PageHeader
- ActionBar
- FilterBar
- DataTable
- FormSection
- SummaryCard
- StatusBadge
- EmptyState
- ErrorState
- LoadingSkeleton
- ConfirmDialog
- Drawer
- Modal
- Tabs
- Timeline

### 15.2 ERP-Specific Business Components
- ApprovalActionBar
- RecordStatusHeader
- DocumentReferencePanel
- LinkedRecordsPanel
- CommentThreadPanel
- AttachmentPanel
- AuditTrailPanel
- WorkflowTimeline
- LineItemEditorGrid
- StockAvailabilityWidget
- BOMSummaryBlock
- QCDecisionPanel
- PaymentSummaryBlock
- DispatchAllocationPanel

### 15.3 Why This Matters
Without a reusable component system, the ERP will become inconsistent across modules and much harder to maintain once sub-files are expanded.

---

## 16. Form Design Standards
ERP transactions are form-heavy. Forms must follow strict standards.

### 16.1 Form Types
- simple master data forms
- transactional header + line item forms
- approval decision forms
- stock movement forms
- upload/comment forms
- filter/search forms

### 16.2 Form Standards
Forms should support:
- required field indication
- inline validation and submit-time validation
- draft save where applicable
- controlled editability by status and role
- dependent fields and derived values
- attachment requirement where needed
- server-side validation error mapping

### 16.3 Line Item Form Rules
Transaction pages with line items should support:
- row add/remove where allowed
- product lookup/selectors
- quantity/amount calculations
- row-level validation
- totals/footer summary
- source-linked line item mapping when converted from another document

---

## 17. Table and Grid Standards
List pages and line-item-heavy screens will depend on strong table behavior.

### 17.1 List Table Requirements
- configurable visible columns
- sorting where valid
- server-driven pagination
- filters and search
- row click to detail page
- row action menu
- bulk selection where permitted
- export where permitted

### 17.2 Transaction Line Item Grid Requirements
- inline cell editing where appropriate
- row totals and aggregate totals
- validation at cell and row level
- linked item references
- read-only mode when status is locked

### 17.3 Status and Reference Columns
Tables should prominently support:
- document number
- status
- requester/owner/customer/vendor
- date
- amount/quantity
- branch/warehouse/plant
- linked reference count or quick link if relevant

---

## 18. Workflow and Approval UX Standards
Step 4 process flow must be visible in the frontend.

### 18.1 Workflow Visibility
Detail pages should clearly display:
- current status
- next possible actions
- who created the record
- who approved/rejected it
- key timestamps
- workflow history

### 18.2 Action Rendering
Buttons should change by role and status.
Examples:
- Save Draft
- Submit
- Approve
- Reject
- Cancel
- Convert to PO
- Receive Goods
- Issue Materials
- Complete Production
- Mark QC Pass/Fail

### 18.3 Confirmation and Guardrails
Sensitive actions should require:
- confirmation dialog
- optional/required remarks
- attachment requirement if business rule demands it
- server-response-based refresh of workflow state

---

## 19. Record Connection and Cross-Page Behavior
This is one of the critical missing execution layers that Step 9 must prepare for.

### 19.1 Record Linkage Principle
The frontend must treat ERP records as connected, not isolated.

Examples:
- PR links to PO(s)
- PO links to GRN(s)
- GRN links to inventory movement
- Work Order links to material issue and production output
- SO links to dispatch and invoice

### 19.2 UI Requirements for Linked Records
Detail pages should support:
- source record reference block
- downstream records section
- quick navigation links
- counts and statuses of linked records
- conversion/action shortcuts where allowed

### 19.3 Page Entry Point Awareness
Pages may be entered from:
- sidebar navigation
- dashboard widgets
- approval queue
- notification links
- related record links
- report drill-down
- global search results

The frontend should maintain context wherever useful.

---

## 20. Dashboard and Work Queue Design
Each major role should eventually receive role-aware dashboards.

### 20.1 Dashboard Content Types
- KPI summary cards
- pending approvals
- pending tasks
- stock alerts
- overdue items
- recent transactions
- performance charts
- shortcut actions

### 20.2 Work Queue Types
- approvals queue
- my draft records
- pending my action
- escalated exceptions
- today’s receipts/issues/dispatches

Step 9 defines the dashboard framework. Detailed per-role dashboards will be defined in later role sub-files.

---

## 21. Comments, Attachments, and Audit UI
The ERP must expose collaboration and traceability features consistently.

### 21.1 Comments Panel
Should support:
- timestamped comments
- author display
- system comment vs user comment distinction
- optional workflow-related remarks

### 21.2 Attachment Panel
Should support:
- upload
- preview/download metadata
- attachment category if needed
- visibility rules where applicable

### 21.3 Audit Trail Panel
Should support:
- created by / updated by
- approval/rejection events
- status change history
- major field-change logs if exposed

---

## 22. Error, Loading, Empty, and No-Access State Standards
A production ERP frontend must define state behavior clearly.

### 22.1 Loading State
Use skeletons or loaders for:
- page load
- table load
- detail panel load
- workflow refresh after mutation

### 22.2 Empty State
Use structured empty states for:
- no records found
- no linked records
- no comments
- no attachments
- no dashboard data

### 22.3 Error State
Error states should distinguish:
- validation errors
- authorization errors
- missing record / deleted record
- server failure
- network failure

### 22.4 No-Access State
If a user reaches a route without permission, the UI must show a clear restricted-access state and avoid exposing unavailable actions.

---

## 23. Frontend API Integration Standards
The frontend must align directly with Step 8.

### 23.1 Service Layer Rules
For each module, frontend integration should define:
- list fetch methods
- detail fetch methods
- create/update methods
- workflow action methods
- attachment/comment methods
- export/report methods where needed

### 23.2 Query and Mutation Strategy
The frontend should support:
- cached list/detail retrieval where appropriate
- explicit refresh after important mutations
- invalidation of related records after workflow actions
- optimistic UI only where business risk is low

### 23.3 Error Mapping
API validation and business-rule errors should be mapped to:
- field-level form errors
- page-level alerts
- action-level feedback dialogs

### 23.4 Authorization Failure Handling
When the API returns forbidden/unauthorized responses, the frontend must:
- stop the action
- show a meaningful message
- refresh or redirect only when appropriate

---

## 24. Search, Filter, Pagination, and Export Standards
These patterns must be consistent across list and report pages.

### 24.1 Search
Support:
- document number search
- keyword search for relevant entity fields
- fast clear/reset behavior

### 24.2 Filters
Common filters may include:
- status
- date range
- branch/warehouse/plant
- requester/owner/approver
- vendor/customer/item

### 24.3 Pagination
Pagination should be server-driven for large ERP tables.

### 24.4 Export
Where permission allows, export should respect current filters and scope restrictions.

---

## 25. Module-Wise Frontend Deliverables
Step 9 should guide frontend deliverables for each module.

### 25.1 Master Data Frontend
Should support:
- master list screens
- create/edit/detail pages
- activation/deactivation flows
- lookup and selector support for transactional modules

### 25.2 Procurement Frontend
Should support:
- PR list/create/detail
- PO list/create/detail
- GRN entry/detail
- approval actions
- source-to-target conversion views

### 25.3 Inventory Frontend
Should support:
- stock overview
- stock movement detail
- stock adjustment
- stock transfer
- ledger-style history views

### 25.4 Production Frontend
Should support:
- production planning
- work order list/detail
- material issue screens
- production entry/output pages
- consumption and output summaries

### 25.5 Quality Frontend
Should support:
- incoming QC
- in-process QC
- final QC
- approve/reject decisions
- defect/rejection visibility

### 25.6 Sales Frontend
Should support:
- sales order list/create/detail
- stock allocation visibility
- customer-centric order tracking

### 25.7 Dispatch Frontend
Should support:
- dispatch planning
- delivery detail
- stock allocation and dispatch confirmation views

### 25.8 Finance Frontend
Should support:
- invoice entry/detail
- payment entry/detail
- transactional reference linking back to source docs

### 25.9 Reports Frontend
Should support:
- operational report pages
- drill-down capability
- export
- chart/grid hybrid layouts where appropriate

### 25.10 Admin / RBAC Frontend
Should support:
- user management
- role management
- module assignment
- role-permission matrix views
- audit-friendly assignment history where required

---

## 26. Frontend Traceability Matrix to Existing Steps
Step 9 should explicitly align with earlier steps.

### Step Mapping Summary
- **Step 1** → overall ERP shell, persona coverage, business context
- **Step 2** → module route groups, major entities, primary actions
- **Step 3** → permission rendering, role dashboards, action visibility
- **Step 4** → workflow actions, status rendering, timeline logic
- **Step 5** → route map and navigation structure
- **Step 6** → page composition and UI layout consistency
- **Step 7** → entity display structure and relational UI blocks
- **Step 8** → service contracts, forms, list fetching, action APIs

This ensures Step 9 is not standalone. It is connected directly to all previous steps.

---

## 27. Expected Outputs of Step 9
After Step 9 is approved, the ERP design package should have a clear frontend implementation direction with:
- application shell standard
- frontend module architecture
- routing strategy
- page type standards
- UI component strategy
- permission-aware rendering approach
- API integration rules
- record linkage rules
- dashboard and queue framework
- loading/error/empty/no-access standards
- module-wise frontend deliverable list

---

## 28. What Comes Immediately After Step 9
Once this document is approved, the next work should be creation of deep sub-files.

### Priority Order After Step 9
1. Role deep files
2. Transaction deep files
3. Page UI deep files
4. Page connection matrix
5. Missing advanced flow files
6. DB deep design files
7. API deep design files

### Why Step 9 Comes Before Sub-Files
Step 9 gives the frontend implementation standard so the later deep files can follow one consistent structure.

Without Step 9:
- role files may define UI behavior inconsistently
- page files may follow different patterns
- action placement may vary across modules
- connection logic may be undocumented
- API usage may become uneven

---

## 29. Recommended Next Sub-File Categories After This Step
After approval of Step 9, create the following sub-file groups:

### A. Role Frontend Files
Examples:
- role_admin_frontend.md
- role_procurement_executive_frontend.md
- role_store_manager_frontend.md
- role_production_supervisor_frontend.md

### B. Transaction UI Files
Examples:
- transaction_purchase_requisition_ui.md
- transaction_purchase_order_ui.md
- transaction_grn_ui.md
- transaction_work_order_ui.md
- transaction_sales_order_ui.md

### C. Page UI Files
Examples:
- page_pr_list_ui.md
- page_pr_detail_ui.md
- page_po_create_ui.md
- page_grn_entry_ui.md
- page_work_order_detail_ui.md

### D. Connection Files
Examples:
- page_connection_matrix.md
- document_connection_matrix.md
- workflow_action_visibility_matrix.md

---

## 30. Final Conclusion
Step 9 – Frontend Development formalizes how the ERP will be built on the UI side.

The earlier steps define:
- what the ERP is
- what modules exist
- who uses it
- how workflows move
- which screens exist
- how data is modeled
- how backend services are exposed

Step 9 now defines:
- how all of that becomes a real frontend application
- how the pages behave consistently
- how users experience workflows and approvals
- how records connect across the ERP
- how future deep page and role files should be structured

This document should be treated as the frontend master strategy for ERP V2.

---

## 31. Next Step
Proceed to:
👉 Detailed frontend sub-files for roles, transactions, pages, and page connections.
