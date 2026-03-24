# Frontend Route Map

## 1. Purpose
This file defines the route structure for the current ERP frontend slice and provides the routing standard for future modules.

It standardizes:
- module route groups
- list/detail/create route patterns
- query parameter conventions
- source/downstream navigation rules
- role-aware route access expectations
- route naming consistency

This file is the routing reference for frontend implementation.

---

## 2. Recommended Folder Location
Keep this file at:

`frontend/frontend_route_map.md`

---

## 3. Scope Covered
This route map currently covers the first completed ERP vertical slice:

- Procurement
  - PR List
  - PR Detail
  - PO List
  - PO Detail
  - GRN Entry
  - GRN Detail
  - Incoming QC
- Inventory
  - Stock Overview

---

## 4. Route Design Principles
### Stable and readable
Routes should reflect business entities clearly.

### Module-first grouping
Group routes by business module, not by technical component type.

### Detail pages should be direct-linkable
Every main record detail page must support direct navigation.

### Query params for context, not identity
Use route params for record identity and query params for filters, source references, and prefill context.

### Guarded by role and permission
Routes must support frontend guard logic, while backend remains the final access authority.

---

## 5. Top-Level Route Groups
Recommended top-level frontend route groups:

- `/dashboard`
- `/master-data`
- `/procurement`
- `/inventory`
- `/quality`
- `/sales`
- `/dispatch`
- `/finance`
- `/reports`
- `/admin`

For the current slice, we are actively defining:
- `/procurement`
- `/inventory`

---

# 6. Procurement Route Map

## 6.1 Purchase Requisition (PR)

### PR List
Route:
`/procurement/purchase-requests`

Purpose:
- list all visible PRs
- search, filter, sort
- quick workflow actions where allowed

Primary roles:
- Procurement Executive
- Procurement Manager

### Create PR
Route:
`/procurement/purchase-requests/new`

Purpose:
- create new PR

Primary roles:
- Procurement Executive
- any maker role with create permission

### PR Detail
Route:
`/procurement/purchase-requests/:prId`

Purpose:
- full PR review and action page

Primary roles:
- Procurement Executive
- Procurement Manager

### Suggested Query Params for PR List
- `status`
- `search`
- `requested_by`
- `department_id`
- `branch_id`
- `priority`
- `date_from`
- `date_to`
- `page`
- `per_page`
- `sort`

Example:
`/procurement/purchase-requests?status=submitted&sort=created_at:desc`

---

## 6.2 Purchase Order (PO)

### PO List
Route:
`/procurement/purchase-orders`

Purpose:
- list all visible POs
- monitor approval and receipt readiness

Primary roles:
- Procurement Executive
- Procurement Manager
- Store User (view)

### Create PO
Route:
`/procurement/purchase-orders/new`

Purpose:
- create PO directly or from approved PR

Primary roles:
- Procurement Executive
- permitted procurement maker roles

### Create PO from PR Context
Route:
`/procurement/purchase-orders/new?source_pr=:prId`

Purpose:
- prefilled PO creation from approved PR

### PO Detail
Route:
`/procurement/purchase-orders/:poId`

Purpose:
- full PO review, approval, and GRN linkage

Primary roles:
- Procurement Executive
- Procurement Manager
- Store User
- QA Inspector (view context)

### Suggested Query Params for PO List
- `status`
- `search`
- `vendor_id`
- `buyer_id`
- `branch_id`
- `delivery_status`
- `date_from`
- `date_to`
- `page`
- `per_page`
- `sort`

Example:
`/procurement/purchase-orders?status=approved&delivery_status=open`

---

## 6.3 Goods Receipt Note (GRN)

### GRN List (future standard)
Route:
`/procurement/grn`

Purpose:
- list all goods receipts
- filter by receipt/QC status

Primary roles:
- Store User
- Store Manager
- QA Team
- Procurement Team (view)

### GRN Entry
Route:
`/procurement/grn/new`

Base purpose:
- create new GRN

### GRN Entry from PO Context
Route:
`/procurement/grn/new?source_po=:poId`

Purpose:
- create GRN against approved PO with preloaded context

Primary roles:
- Store User
- Store Manager

### GRN Detail
Route:
`/procurement/grn/:grnId`

Purpose:
- review receipt, QC, and stock impact

Primary roles:
- Store User
- Store Manager
- QA Inspector
- Procurement Team (view)

### Suggested Query Params for GRN List
- `status`
- `qc_status`
- `warehouse_id`
- `vendor_id`
- `source_po`
- `date_from`
- `date_to`
- `page`
- `per_page`
- `sort`

Example:
`/procurement/grn?qc_status=pending&warehouse_id=3`

---

## 6.4 Incoming QC

### Incoming QC Queue
Route:
`/procurement/incoming-qc`

Purpose:
- queue/page for QC-required incoming receipts

Primary roles:
- QA Inspector
- QA Team

### Suggested Query Params
- `status`
- `warehouse_id`
- `vendor_id`
- `date_from`
- `date_to`
- `source_grn`
- `page`
- `per_page`
- `sort`

Example:
`/procurement/incoming-qc?status=qc_pending`

### Optional Future Detail Pattern
If QC becomes a dedicated record:
`/quality/incoming-qc/:qcId`

For now, the queue/action model is sufficient.

---

# 7. Inventory Route Map

## 7.1 Stock Overview

### Stock Overview
Route:
`/inventory/stock-overview`

Purpose:
- view stock by item/location/state

Primary roles:
- Store User
- Store Manager
- Procurement Team (view)
- QA Team (view)
- Production Planner (view)

### Optional Item Drilldown
Route:
`/inventory/stock-overview/:itemId`

Purpose:
- open item-level stock detail/drilldown page if needed later

### Suggested Query Params
- `search`
- `warehouse_id`
- `location_id`
- `item_id`
- `category_id`
- `stock_status`
- `low_stock`
- `hide_zero`
- `page`
- `per_page`
- `sort`

Example:
`/inventory/stock-overview?stock_status=qc_hold&warehouse_id=2`

---

# 8. Standard Route Patterns

## List Page Pattern
`/{module}/{resource}`

Examples:
- `/procurement/purchase-requests`
- `/procurement/purchase-orders`
- `/inventory/stock-overview`

## Create Page Pattern
`/{module}/{resource}/new`

Examples:
- `/procurement/purchase-requests/new`
- `/procurement/purchase-orders/new`
- `/procurement/grn/new`

## Detail Page Pattern
`/{module}/{resource}/:id`

Examples:
- `/procurement/purchase-requests/:prId`
- `/procurement/purchase-orders/:poId`
- `/procurement/grn/:grnId`

## Context Prefill Pattern
Use query params:
- `?source_pr=:prId`
- `?source_po=:poId`

---

# 9. Query Parameter Conventions

## Use query params for:
- filters
- sort
- pagination
- source context
- saved views (future)
- dashboard deep-link state

## Avoid query params for:
- primary record identity
- core route ownership

## Standard Common Params
- `page`
- `per_page`
- `sort`
- `search`
- `status`
- `date_from`
- `date_to`

## Source Context Params
- `source_pr`
- `source_po`
- `source_grn`

## Workflow Context Params (optional)
- `qc_status`
- `delivery_status`
- `view`
- `tab`

---

# 10. Route Guard Expectations

## Procurement Executive
Should access:
- PR list/create/detail
- PO list/create/detail
- linked document detail views

Should not access:
- approval-only workspaces unless explicitly permitted
- QC execution routes unless role explicitly expanded

## Procurement Manager
Should access:
- PR list/detail
- PO list/detail
- filtered approval queue views if implemented

Should not need:
- create routes unless business policy allows dual role

## Store User
Should access:
- PO detail (view context)
- GRN entry
- GRN detail
- stock overview

Should not access:
- procurement approval routes
- PR/PO create/edit routes

## QA Inspector
Should access:
- GRN detail (for QC context)
- incoming QC route
- stock overview (QC hold context if allowed)

Should not access:
- procurement create/approve routes
- inventory mutation routes not related to QC

---

# 11. Navigation Consistency Rules
### Detail routes must always be direct-linkable
Every record page should load fully from URL alone.

### Upstream/downstream links should use record detail routes
Examples:
- PR Detail → PO Detail
- PO Detail → GRN Detail
- GRN Detail → PO Detail

### Do not encode workflow state only in frontend memory
Use query params or server data when route state matters.

---

# 12. Deep Link Examples
### Notification → PR awaiting approval
`/procurement/purchase-requests/145`

### Dashboard → approved POs awaiting receipt
`/procurement/purchase-orders?status=approved&delivery_status=open`

### PO Detail → create GRN from PO
`/procurement/grn/new?source_po=232`

### Dashboard → QC pending queue
`/procurement/incoming-qc?status=qc_pending`

### Inventory → QC hold stock only
`/inventory/stock-overview?stock_status=qc_hold`

---

# 13. Return/Back Behavior Rules
### From list → detail → back
Return to previous filtered list state where practical.

### From create → successful save
Redirect to created record detail page.

### From detail workflow action
Stay on same detail route and refresh state, unless action explicitly creates downstream record and design redirects there.

### From context-created downstream page
Example:
- PO Detail → GRN Entry (`source_po`)
After save/submit:
- redirect to GRN Detail
After cancel:
- return to source PO Detail

---

# 14. Breadcrumb Alignment Rules
Breadcrumb should reflect route hierarchy, not temporary entry history.

Examples:
- Procurement / Purchase Requisitions
- Procurement / Purchase Requisitions / PR-000145
- Procurement / Purchase Orders / PO-000232
- Procurement / GRN / GRN-000078
- Inventory / Stock Overview

Source/downstream relationships should be shown in page body, not overloaded into breadcrumb.

---

# 15. Frontend Implementation Notes
Frontend route config should define for each route:
- path
- page component
- module
- permission guard
- optional title/breadcrumb metadata
- optional query param schema

### Example route config fields
- `path`
- `component`
- `requiredPermissions`
- `allowedRoles`
- `breadcrumbLabel`
- `useQuerySchema`

---

# 16. Completion Criteria
This file is complete when:
- core module route groups are defined
- list/create/detail patterns are standardized
- query param conventions are documented
- guard expectations are clear
- deep-link examples exist
- return/back behavior is defined

---

# 17. Next Related Files
Recommended next files:
1. frontend_permission_logic_matrix.md
2. api_page_dependency_matrix.md
3. frontend_state_management_plan.md
4. frontend_folder_structure_code_plan.md
