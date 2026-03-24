# Step 6 – Wireframe Design (Banani)

## Objective
Convert the V2 ERP process flows, role model, and screen map into production-grade wireframe instructions for Banani so each module can be visualized screen by screen with clear layout, hierarchy, actions, states, and role-based behavior.

## Scope of Step 6
This step should define wireframes at page level, not just feature level. Every major screen should show:
- purpose of the page
- primary users / roles
- key data blocks
- filters and search
- main table / list / card areas
- primary and secondary CTAs
- status visibility
- approval / rejection / exception actions
- empty, loading, error, and no-permission states
- drawer / modal / side panel behavior
- navigation entry points and breadcrumbs
- linked actions to upstream / downstream modules

## Required Inputs
Before final wireframe execution, Banani should use these artifacts as source truth:
1. ERP Planning
2. Module Design
3. User Role Design
4. Process Flow Design
5. Screen Map

## Wireframe Design Principles
- Enterprise-first, operationally dense but visually controlled
- Minimize clicks for high-frequency user roles
- Keep approval and exception actions visible without clutter
- Tables for transaction-heavy workflows; cards only where summary is more useful
- Strong page hierarchy: header -> summary -> filters -> workspace -> details/actions
- Role-based visibility should change actions, sections, and editability
- Status should always be visible at record level and page level
- Use right-side drawers for quick review/edit where full-page transition is unnecessary
- Use full-page forms for creation flows with many dependencies
- Use sticky action bars for long forms and approval pages
- Show audit trail, comments, and attachments where business decisions happen

## Standard Wireframe Template Per Screen
For each screen, document the following:

### 1. Screen Name
Clear page name aligned with the screen map.

### 2. Module
Which module owns the screen.

### 3. Purpose
What business outcome this page supports.

### 4. Primary Roles
Who mainly uses it, who can approve, who can view only.

### 5. Entry Points
How users reach the page:
- left navigation
- dashboard widgets
- related record links
- notifications/tasks
- breadcrumbs/search/global actions

### 6. Layout Structure
Define page structure such as:
- global header
- page title + breadcrumb
- KPI strip / status summary
- filter/search section
- main list/table
- side detail panel / tabs / drawer
- footer / sticky action bar

### 7. Key Components
Specify components such as:
- filters
- segmented tabs
- data table columns
- summary cards
- forms
- accordions
- timeline
- comments
- attachments
- audit log
- approval panel

### 8. Actions
Break into:
- primary CTA
- row actions
- bulk actions
- approval actions
- exception handling actions
- export/print/share actions

### 9. States
Must include:
- draft
- submitted
- in review
- approved
- rejected
- cancelled
- archived
- empty
- error
- loading
- no access

### 10. Role Logic
Document what each role can:
- view
- create
- edit
- approve
- reject
- delete/cancel
- export
- comment

### 11. Business Rules Reflected in UI
Examples:
- fields locked after submission
- approver section visible only at review stage
- rejection requires reason
- downstream actions enabled only after approval
- partial completion warnings
- mandatory attachments for specific conditions

### 12. Linked Screens
What opens from this page and what returns to it.

## Global ERP Page Patterns for Banani

### A. List Page Pattern
Use for requests, orders, inventory records, invoices, employee records, tickets, etc.

**Recommended structure**
- breadcrumb + page title
- top-right primary CTA
- top summary strip with counts by status
- advanced filters row
- searchable/sortable data table
- row click to details page or drawer
- bulk action bar appears on selection
- export/download option

**Include**
- saved views
- quick filters by status/date/owner/location
- column management where needed
- badge-based status indicators

### B. Detail Page Pattern
Use for transactional records requiring review, action, or auditability.

**Recommended structure**
- header with record ID, title, current status, owner
- action cluster on top-right
- left/main content area with tabs or sections
- right panel with approval, comments, timeline, attachments, audit trail
- related records block near bottom

**Tabs / sections can include**
- overview
- line items/details
- approvals
- documents
- activity log

### C. Creation / Edit Form Pattern
Use for new requests, master data setup, configurations, submissions.

**Recommended structure**
- breadcrumb + page title
- multi-section form or stepper if complex
- sticky action footer with Save Draft / Submit / Cancel
- validation inline near fields
- contextual help for dependency-heavy fields

### D. Dashboard Pattern
Use for role homepages.

**Recommended structure**
- greeting + role context
- KPI summary cards
- pending tasks / approvals queue
- alerts / exceptions
- shortcuts to common actions
- charts only where decision-making benefits
- recent activity / reminders

### E. Approval Workspace Pattern
Use where approvers handle many records quickly.

**Recommended structure**
- queue list on left or full-width table
- preview panel on right
- approve/reject/return buttons always visible
- mandatory comment on rejection/return
- compare/inspect support for critical decisions

## Banani Prompt Structure
Use this prompt format for each module or screen cluster:

> Design low-fidelity enterprise wireframes for the **[Module Name]** module of a V2 ERP system. The target users are **[roles]**. The module supports **[business process summary]**. Create wireframes for the following screens: **[screen list]**. For each screen, define page layout, information hierarchy, navigation, filters, search, table/list structures, forms, tabs, drawers/modals, status indicators, approval actions, exception handling, empty/error/loading states, and role-based visibility. Ensure the UI reflects enterprise ERP behavior, auditability, and high-frequency operational efficiency. Prefer structured tables, sticky action bars, visible statuses, and approval side panels where appropriate.

## Production Prompt Template Per Screen

### Prompt Template
Design a low-fidelity wireframe for the ERP screen **[Screen Name]** under the **[Module Name]** module.

Business purpose: **[purpose]**  
Primary roles: **[roles]**  
Entry points: **[entry points]**  
Screen type: **List / Detail / Form / Dashboard / Approval Workspace / Settings**

The wireframe must include:
- top navigation context with breadcrumb and page title
- visible record/page status
- page-level summary or KPI strip if relevant
- search, filters, sort, and saved views if relevant
- data table/card/list/form layout as applicable
- primary CTA and secondary actions
- row-level or section-level actions
- comments, attachments, and audit trail where decisions happen
- approval/rejection actions if applicable
- empty/loading/error/no-access states
- role-based visibility and editability
- linked records or downstream/upstream module references

Design rules:
- optimize for enterprise efficiency, not marketing aesthetics
- use compact but readable layout
- minimize clicks for repeated actions
- keep critical actions and statuses above the fold
- show exception handling explicitly
- preserve consistency with other ERP modules

## Recommended Execution Order for Banani
1. Global design system assumptions
2. Dashboard wireframes by role
3. Transaction list pages
4. Transaction detail pages
5. Create/edit forms
6. Approval workspaces
7. Master data pages
8. Settings/configuration pages
9. Edge states and permission states

## Deliverables Expected from Step 6
- wireframe set for each module
- screen-by-screen wireframe notes
- layout rationale for complex screens
- role-based differences annotated
- action/state mapping visible in wireframes
- exceptions and alternate flows represented
- clear linkage to process flows and screen map

## Handoff Note
Once Banani completes these wireframes, the next step should be UI design / visual system application on top of approved wireframe logic, not before.

