# Connection: Navigation & Deep Links

## 1. Purpose
Defines how deep links, notifications, dashboard shortcuts, and filtered navigation behave across the ERP system.

This ensures:
- consistent entry into pages
- correct context loading
- smooth user experience across workflows
- predictable back/return behavior

---

## 2. Folder Location
connections/connection_navigation_and_deep_links.md

---

## 3. Deep Link Principles
- Always open the most actionable page (usually Detail page)
- Preserve context using query params
- Avoid redirect loops
- Ensure role-based access control before rendering

---

## 4. Notification Deep Links

### Rules
- PR approval → PR Detail
- PO approval → PO Detail
- GRN pending QC → GRN Detail
- GRN accepted/rejected → GRN Detail

### Example
`/procurement/purchase-orders/123`
`/procurement/grn/456`

---

## 5. Dashboard Deep Links

### Example Widgets
- Pending PR → `/procurement/purchase-requests?status=submitted`
- Approved PO → `/procurement/purchase-orders?status=approved`
- QC Pending → `/procurement/grn?qc_status=pending`

---

## 6. Query Param Rules
Use query params for:
- filters
- prefill context
- workflow states

### Examples
- `?status=submitted`
- `?source_po=123`
- `?qc_status=pending`

---

## 7. Return Navigation Rules
- After create → go to Detail page
- After approve/reject → stay on same page
- After cancel → return to list or parent
- Preserve filters when returning to list

---

## 8. Role-Based Routing
- Block unauthorized routes
- Redirect to safe page if access denied
- Hide navigation triggers in UI

---

## 9. Error Handling
- Invalid ID → show “Record not found”
- No permission → show “Access denied”
- Missing context → fallback to list page

---

## 10. Completion Criteria
- all deep link entry points defined
- query param usage standardized
- return navigation defined
- role-based routing enforced
