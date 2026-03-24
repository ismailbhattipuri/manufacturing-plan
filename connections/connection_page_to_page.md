# Connection: Page to Page Navigation Mapping

## 1. Purpose
Defines how users move between pages in the procurement and inbound inventory workflow.

This file translates document flow into actual UI navigation flow.

Main chain:
PR List → PR Detail → PO List / Create PO → PO Detail → GRN Entry → GRN Detail

It ensures:
- consistent routing
- clear button/page transitions
- proper deep linking
- role-aware navigation behavior
- upstream/downstream traceability on the UI side

---

## 2. Scope
This file currently covers the first completed ERP slice:

- PR List
- PR Detail
- PO List
- PO Detail
- GRN Entry
- GRN Detail

It also includes:
- dashboard entry points
- notification entry points
- linked-record navigation
- breadcrumb return logic

---

## 3. Folder Location
This file should be kept at:

`connections/connection_page_to_page.md`

This keeps navigation logic separate from:
- transaction logic
- role logic
- page UI specification

---

## 4. Core Navigation Chain
### Main Operational Flow
- PR List → PR Detail
- PR Detail → Create PO / PO Detail
- PO List → PO Detail
- PO Detail → GRN Entry
- GRN Entry → GRN Detail
- GRN Detail → PO Detail

### Supporting Navigation
- Dashboard → PR List
- Dashboard → PO List
- Dashboard → GRN Entry
- Notifications → relevant detail page
- Global Search → detail page directly

---

## 5. PR List Navigation Rules
### From PR List
Users can navigate to:
- Create PR page
- PR Detail page
- filtered list states by summary strip
- export flow

### Trigger Rules
#### Action: Create PR
- source page: PR List
- target page: Create PR
- method: primary CTA button
- route: `/procurement/purchase-requests/new`

#### Action: Open PR Detail
- source page: PR List
- target page: PR Detail
- method: row click or View action
- route: `/procurement/purchase-requests/{id}`

#### Action: Approve PR (from list)
- source page: PR List
- result: approval mutation on same page or open detail depending on design
- recommendation: allow quick action for simple approval, but open detail for full review if business requires

---

## 6. PR Detail Navigation Rules
### From PR Detail
Users can navigate to:
- PR List
- Create PO
- linked PO Detail
- print/export flow

### Trigger Rules
#### Action: Back to List
- source page: PR Detail
- target page: PR List
- method: breadcrumb or back action
- route: `/procurement/purchase-requests`

#### Action: Convert to PO
- source page: PR Detail
- target page: Create PO
- method: top-right action button
- condition: PR status = Approved
- route: `/procurement/purchase-orders/new?source_pr={id}`

#### Action: View Linked PO
- source page: PR Detail
- target page: PO Detail
- method: linked records section
- route: `/procurement/purchase-orders/{po_id}`

---

## 7. PO List Navigation Rules
### From PO List
Users can navigate to:
- Create PO
- PO Detail
- filtered list states by summary strip

### Trigger Rules
#### Action: Create PO
- source page: PO List
- target page: Create PO
- method: primary CTA button
- route: `/procurement/purchase-orders/new`

#### Action: Open PO Detail
- source page: PO List
- target page: PO Detail
- method: row click or View action
- route: `/procurement/purchase-orders/{id}`

---

## 8. PO Detail Navigation Rules
### From PO Detail
Users can navigate to:
- PO List
- source PR Detail
- GRN Entry
- linked GRN Detail

### Trigger Rules
#### Action: Back to PO List
- source page: PO Detail
- target page: PO List
- method: breadcrumb or back action
- route: `/procurement/purchase-orders`

#### Action: View Source PR
- source page: PO Detail
- target page: PR Detail
- method: source reference section
- route: `/procurement/purchase-requests/{pr_id}`

#### Action: Receive Goods / Create GRN
- source page: PO Detail
- target page: GRN Entry
- method: top-right action button
- condition: PO status = Approved
- route: `/procurement/grn/new?source_po={id}`

#### Action: View Linked GRN
- source page: PO Detail
- target page: GRN Detail
- method: linked records section
- route: `/procurement/grn/{grn_id}`

---

## 9. GRN Entry Navigation Rules
### From GRN Entry
Users can navigate to:
- PO Detail
- GRN Detail
- GRN List (future)
- cancel/back flow

### Trigger Rules
#### Action: Open Source PO
- source page: GRN Entry
- target page: PO Detail
- method: source PO reference block
- route: `/procurement/purchase-orders/{po_id}`

#### Action: Save / Mark Received
- source page: GRN Entry
- target page: GRN Detail
- method: submit/save completion action
- route: `/procurement/grn/{grn_id}`

#### Action: Cancel Entry
- source page: GRN Entry
- target page: PO Detail or GRN List
- method: cancel action
- recommendation: if opened from PO, return to PO Detail; otherwise return to GRN List

---

## 10. GRN Detail Navigation Rules
### From GRN Detail
Users can navigate to:
- source PO Detail
- linked QC record (future)
- linked stock movement (future)
- GRN List (future)

### Trigger Rules
#### Action: View Source PO
- source page: GRN Detail
- target page: PO Detail
- method: source reference section
- route: `/procurement/purchase-orders/{po_id}`

#### Action: View Linked QC Record
- source page: GRN Detail
- target page: QC Detail / Incoming QC page
- method: linked records section
- route: future quality route

#### Action: View Stock Impact
- source page: GRN Detail
- target page: inventory movement / stock detail page
- method: stock impact section
- route: future inventory route

---

## 11. Dashboard Entry Rules
### Procurement Dashboard Widgets
Recommended widget links:
- My PRs → PR List with pre-applied filter
- Pending PR Approvals → PR List or PR Detail queue
- Approved PR Awaiting PO → PR List filtered or PR Detail deep link
- Pending PO Approvals → PO List filtered
- Approved PO Awaiting GRN → PO List filtered
- Pending GRN / QC → GRN Entry / GRN Detail queue

### Route Pattern
Prefer query params for pre-filtering, for example:
- `/procurement/purchase-requests?status=submitted`
- `/procurement/purchase-orders?status=approved`
- `/procurement/grn?qc_status=pending`

---

## 12. Notification Deep Link Rules
Notifications should open the most actionable page.

### Examples
- PR submitted for approval → PR Detail
- PR approved → PR Detail
- PO submitted for approval → PO Detail
- PO approved and ready for receipt → PO Detail
- GRN pending QC → GRN Detail
- GRN accepted/rejected → GRN Detail

### Rule
Notification links should land on the detail page, not only the list page, unless the notification refers to a queue-type workload.

---

## 13. Global Search Navigation Rules
Global search results should route users directly to:
- PR Detail
- PO Detail
- GRN Detail

Search result cards/rows should show:
- document number
- document type
- status
- primary reference (vendor/requester/source PO)
- deep-link target

---

## 14. Breadcrumb Rules
### Standard Rules
- list page breadcrumb ends at module/list
- detail page breadcrumb includes record identifier
- create page breadcrumb includes “New” state
- linked source/downstream pages should not break breadcrumb hierarchy unexpectedly

### Examples
- Procurement / Purchase Requisitions
- Procurement / Purchase Requisitions / PR-000145
- Procurement / Purchase Orders / PO-000232
- Procurement / GRN / GRN-000078

### Important Rule
Breadcrumb should reflect current page hierarchy, while source/downstream links should be shown inside the page body.

---

## 15. Role-Based Navigation Rules
### Procurement Executive
Should navigate easily between:
- PR List ↔ PR Detail
- PR Detail → Create PO
- PO List ↔ PO Detail
- linked PO view

### Procurement Manager
Should navigate mainly between:
- PR approval queue / PR Detail
- PO approval queue / PO Detail
- filtered lists and detail review pages

### Store User
Should navigate mainly between:
- PO Detail → GRN Entry
- GRN Entry → GRN Detail
- GRN Detail ↔ source PO

### QA Inspector
Should navigate mainly between:
- QC queue (future) → GRN Detail
- GRN Detail → linked QC / stock reference
- GRN Detail ↔ source PO for context

---

## 16. Page Transition Conditions
Transitions must be state-aware.

### Examples
- PR Detail → Create PO only if PR is Approved
- PO Detail → GRN Entry only if PO is Approved
- GRN Detail → QC page only if QC required / linked QC exists
- GRN Detail → stock impact page only if stock posting exists

### Rule
Do not expose navigation to downstream pages unless transition eligibility exists.

---

## 17. Data Passed Between Pages
### PR Detail → Create PO
Pass:
- source_pr_id
- optional selected line ids in future
- source context for prefill

### PO Detail → GRN Entry
Pass:
- source_po_id
- receiving context defaults if available
- source lines for prefill

### GRN Detail → future inventory/QC pages
Pass:
- grn_id
- line references if needed
- linked movement / qc record id

### Recommendation
Use route params for identity and query params for prefill/context.

---

## 18. Page Return Rules
### After Create / Save / Submit
- Create PR → PR Detail
- Create PO → PO Detail
- Create GRN / Mark Received → GRN Detail

### After Cancel
- PR cancel from create/detail → PR List or PR Detail read-only depending on state
- PO cancel from create/detail → PO List or PO Detail
- GRN cancel from entry → return to PO Detail if launched from PO, otherwise GRN List

### After Approval/Rejection
Stay on current detail page and refresh state, unless business wants to return to queue after action.

---

## 19. UI Control Rules for Navigation
Navigation triggers may appear as:
- primary CTA buttons
- row click
- row action menus
- linked record cards
- breadcrumbs
- summary card filters
- notifications
- search results

### Important Rule
The same transition should be consistent across the product.
Example:
- “View Linked PO” should always open PO Detail, not sometimes a drawer and sometimes a page, unless explicitly designed.

---

## 20. Future Expansion Notes
This navigation map can later expand to include:
- GRN List
- Incoming QC list/detail
- Inventory movement detail
- stock overview
- return-to-vendor flow
- finance invoice matching flow
- approval queue workspaces

---

## 21. Frontend Notes
Frontend should:
- centralize routes in a route map
- support deep linking to all detail pages
- preserve filter/query state when returning to lists where practical
- enforce route guards using role and permission checks
- keep linked-record navigation visible inside detail pages

---

## 22. Backend Notes
Backend should:
- provide enough linked-record metadata for navigation blocks
- expose source/downstream references in detail payloads
- support summary counts and filter parameters for list-entry routes
- ensure unauthorized detail routes return proper access errors

---

## 23. Completion Criteria
Page-to-page navigation design is complete when:
- list/detail/create transitions are defined
- upstream/downstream page links are defined
- dashboard/search/notification entry routes are defined
- role-based navigation expectations are clear
- route patterns are stable
- state-based navigation restrictions are documented

---

## 24. Next Related Files
Recommended next files:
1. connection_navigation_and_deep_links.md
2. page_incoming_qc.md
3. inventory/page_stock_overview.md
4. finance/page_invoice_entry.md
