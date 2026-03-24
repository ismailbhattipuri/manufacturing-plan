# Page: GRN Detail

## 1. Page Purpose
The GRN Detail page is the full record workspace for a single Goods Receipt Note (GRN). It allows authorized users to:
- review receipt details against the source Purchase Order (PO)
- see received, accepted, rejected, and pending QC quantities
- verify warehouse/bin, batch, and serial capture
- review discrepancy notes and quality outcome
- view stock impact, comments, attachments, and audit history

This page is the main operational review screen for inbound receipt after GRN creation.

---

## 2. Source Alignment
This page must remain aligned with:
- Step 2 – Procurement Module
- Step 3 – User Role Design
- Step 4 – Procurement Flow
- Step 5 – Screen Map
- Step 6 – Wireframe Design
- Step 8 – Backend API
- Step 9 – Frontend Development
- txn_goods_receipt_note.md
- txn_purchase_order.md
- page_po_detail.md
- page_grn_entry.md

---

## 3. Module
- Procurement / Inventory Inbound

---

## 4. Recommended Folder Location
This file should be kept at:

`pages/procurement/page_grn_detail.md`

### Why this location
Keep it beside:
- `page_po_detail.md`
- `page_grn_entry.md`

because your current working flow is organized as one continuous procurement chain:

PR → PO → GRN

This makes the full upstream/downstream document flow easy to track in one module folder.

---

## 5. Page Type
- Standard ERP Detail View Page

This page follows the detail-page pattern:
- breadcrumb + title
- receipt header with status
- top-right action cluster
- main receipt and line item content
- right-side or tabbed contextual panels for QC, comments, attachments, and audit
- linked stock / PO / quality references

---

## 6. Primary Roles
### Main Users
- Store User / Warehouse User
- Store Manager
- QA Inspector / QA Team

### Secondary Users
- Procurement Executive
- Procurement Manager
- Finance / Management (view only if permitted)
- Admin (audit/configuration support only)

---

## 7. Business Goal of This Page
This page supports the following business outcomes:
- complete review of what was physically received
- control of QC and acceptance/rejection decisions
- traceable PO-to-receipt linkage
- correct stock-impact visibility
- strong discrepancy and audit tracking

---

## 8. Entry Points
Users may reach this page from:
- GRN List row click
- GRN Entry completion redirect
- PO Detail linked GRN section
- inbound receipt queue
- QC pending queue
- notification link
- global search result

---

## 9. Route / Navigation Position
### Suggested Route
- `/procurement/grn/{id}`

### Related Routes
- `/procurement/grn`
- `/procurement/grn/new?source_po={id}`
- `/procurement/purchase-orders/{id}`

---

## 10. Breadcrumb
Recommended breadcrumb:
- Procurement / GRN / GRN-XXXXXX

Optional expanded path:
- Procurement / Purchase Orders / PO-XXXXXX / GRN-XXXXXX

Use the simpler one unless cross-document breadcrumb depth is intentionally supported.

---

## 11. Record Header Structure
The header area should contain:
- GRN Number
- current Status badge
- source PO Number
- vendor name
- GRN Date
- receiving warehouse
- received by
- QC status indicator
- receipt summary (received / accepted / rejected)
- top-right action cluster

### Header Purpose
The user should understand immediately:
- which receipt this is
- what order/vendor it belongs to
- what happened to the goods
- what actions remain

---

## 12. Top-Right Action Cluster
Actions should render based on role + status.

### Possible Actions
- Edit
- Mark Received
- Send to QC
- Accept
- Reject
- Cancel
- Print
- Export
- Add Comment
- View Source PO
- View Stock Impact

### Action Visibility Rules
#### Store User / Warehouse User
- Pending → Edit, Mark Received, Cancel
- Received → View, Send to QC, Accept (if QC not required), Comment
- QC Pending → View only, Comment
- Accepted / Rejected / Cancelled → View only

#### QA Inspector / QA Team
- QC Pending → Accept, Reject, Comment
- Received + QC required → Send to QC / inspect workflow actions if policy uses staged action
- Accepted / Rejected → View only

#### Store Manager
- can view all
- may approve exceptional handling if policy applies
- may cancel before final stock impact where allowed

#### Procurement Team
- view receipt outcome
- no warehouse or QC mutation rights unless policy allows

---

## 13. Main Layout Structure
Recommended layout:

### Option A – Main Content + Right Context Panel
#### Left / Main Content
- source PO summary
- receipt summary
- line items with quantity breakdown
- discrepancy / delivery details
- linked stock impact section

#### Right Context Panel
- QC status and actions
- comments
- attachments
- audit summary

### Option B – Main Content with Tabs
Tabs:
- Overview
- Line Items
- QC / Disposition
- Comments
- Attachments
- Audit Log
- Linked Records
- Stock Impact

### Recommendation
Use a hybrid structure:
- receipt summary and line items visible by default
- QC, comments, attachments, and audit available in contextual side panel or secondary tabs

---

## 14. Source PO Summary Section
This section should clearly show the upstream PO relationship.

### Show
- Source PO Number
- PO Status
- Vendor
- Buyer / Procurement Owner
- Expected Delivery Date
- quick link to PO Detail

### Purpose
This preserves procurement traceability and helps review open-vs-received context.

---

## 15. Receipt Summary Section
This is the first main operational content block.

### Recommended Fields
- GRN Date
- Delivery Note Number
- Received By
- Receiving Warehouse
- Receiving Bin / Location
- Vehicle / Shipment Reference (optional)
- Gate Entry Reference (optional)
- Delivery Date
- Receipt Remark
- Discrepancy Flag
- QC Required Overall Flag

### Quantity Summary Cards
- Total Received
- Total Accepted
- Total Rejected
- QC Pending Quantity
- Receipt Completion %

---

## 16. Line Items Section
This section shows the received lines in detail.

### Recommended Grid Columns
- Line No
- Item Code
- Item Name / Description
- UOM
- Ordered Quantity
- Previously Received Quantity
- Current Received Quantity
- Accepted Quantity
- Rejected Quantity
- Remaining PO Quantity
- Warehouse / Bin
- Batch No
- Serial Reference
- QC Required
- Condition Note
- Line Remark

### Behavior
- editable only while Pending
- read-only after receipt is finalized
- quantity reconciliation must be visually clear
- discrepancy rows should be highlighted

---

## 17. QC / Disposition Section
This section controls quality handling.

### Must Show
- whether QC is required
- current QC status
- inspector / inspected by
- inspection date/time
- accepted / rejected result
- rejection reason
- quarantine / hold information if applicable

### Actions
- Send to QC
- Accept
- Reject

### Rule
QC-required goods should not bypass this section.

---

## 18. Stock Impact Section
This is one of the most important GRN detail sections.

### Show
- whether stock has been posted
- posting date/time
- warehouse/location impacted
- quantity moved to unrestricted stock
- quantity moved to QC-hold/quarantine stock
- linked inventory movement reference

### Purpose
Users should know exactly whether the GRN has impacted stock, and how.

---

## 19. Linked Records Section
This section connects GRN to related records.

### Show
- Source PO
- linked QC inspection record(s)
- linked inventory movement(s)
- future return-to-vendor reference
- future invoice matching reference

### Quick Links
- View PO
- View QC Record
- View Inventory Movement

---

## 20. Comments Section
Support structured discussion on the receipt.

### Recommended Behavior
- show author, timestamp, comment text
- distinguish receiving note vs QC note vs rejection note
- allow authorized users to add comments
- maintain history traceability

---

## 21. Attachments Section
Support documents and evidence for the receipt.

### Typical Attachments
- delivery note
- challan
- packing slip
- receipt image/photo
- damage evidence
- QC evidence
- rejection support document

### Behavior
- show file name, uploaded by, uploaded at, type
- upload allowed in Pending / Received / QC Pending based on policy
- delete restricted by role/status

---

## 22. Audit Log Section
This section should show major GRN activity.

### Recommended Events
- created
- edited
- marked received
- sent to QC
- accepted
- rejected
- cancelled
- quantity changes
- warehouse/bin changes
- stock posting
- linked QC action

### Recommended Fields
- event type
- acted by
- acted at
- short description
- old/new value summary for critical changes where available

---

## 23. Status-Based UI Rules
### Pending
- editable by receiving role
- Mark Received visible
- Cancel allowed
- all receipt fields open

### Received
- content mostly locked
- Accept / Send to QC / Reject visible based on policy
- receipt summary emphasized

### QC Pending
- QC controls visible
- warehouse editing blocked
- quality outcome pending

### Accepted
- read-only
- stock impact visible
- linked inventory movement emphasized

### Rejected
- rejection reason prominently visible
- stock impact blocked or isolated
- view only for most users

### Cancelled
- read-only
- no operational actions

---

## 24. Role-Based UI Rules
### Store User / Warehouse User
Should see:
- editable pending receipt
- Mark Received
- warehouse/bin and quantity details
- comments and attachments
- source PO summary

Should not see:
- business approval controls unrelated to receipt
- unrestricted QC override rights if policy separates them

### QA Inspector / QA Team
Should see:
- QC pending receipts
- disposition controls
- inspection note fields
- accepted/rejected summary

Should not see:
- procurement commercial edit rights
- unrelated warehouse mutation controls after receipt

### Procurement Team
Should see:
- receipt progress
- discrepancy and rejection outcome
- source/downstream traceability

---

## 25. Confirmation and Guardrail Behavior
### Mark Received
- confirmation optional
- backend validation mandatory
- quantity reconciliation required

### Send to QC
- only when QC-required logic applies
- should lock operational receipt quantities

### Accept
- allowed only when quality/disposition rules pass
- triggers stock posting according to policy

### Reject
- reason required
- rejected quantity must be explicit
- stock posting must be blocked or handled correctly

### Cancel
- confirmation required
- allowed only before final impact if policy allows

---

## 26. Empty / Missing Data States
### Missing Optional Data
Show graceful placeholders like:
- “No attachment uploaded”
- “No comments yet”
- “No QC record linked”
- “No inventory movement linked yet”

### Missing Record
If record not found:
- show record-not-found state
- provide back-to-list action

---

## 27. Loading State
Use:
- page header skeleton
- summary skeleton
- line item table skeleton
- contextual panel/tabs skeleton
- stock impact section loading placeholder

---

## 28. Error State
Possible errors:
- record fetch failed
- QC/disposition action failed
- comments/attachments fetch failed
- stock impact fetch failed
- linked record fetch failed

Display:
- clear message
- retry action
- partial section recovery where possible

---

## 29. No-Access State
If the user lacks permission:
- show restricted-access message
- hide receipt and stock details
- prevent unauthorized action visibility

---

## 30. Data Requirements
This page needs:
- GRN header detail
- GRN line items
- source PO summary
- role-based action permissions
- QC status / result
- comments
- attachments
- audit history
- linked inventory movement summary
- linked QC summary
- stock impact summary

---

## 31. API Dependencies
### Core APIs
- Get GRN detail
- Update pending GRN
- Mark GRN received
- Send GRN to QC
- Accept GRN
- Reject GRN
- Cancel GRN
- Get GRN comments
- Add GRN comment
- Get GRN attachments
- Upload attachment
- Get GRN audit log
- Get linked inventory movement
- Get linked QC record
- Get source PO summary

### Example Endpoint Needs
- `GET /api/v1/goods-receipts/{id}`
- `PATCH /api/v1/goods-receipts/{id}`
- `POST /api/v1/goods-receipts/{id}/mark-received`
- `POST /api/v1/goods-receipts/{id}/send-to-qc`
- `POST /api/v1/goods-receipts/{id}/accept`
- `POST /api/v1/goods-receipts/{id}/reject`
- `POST /api/v1/goods-receipts/{id}/cancel`
- `GET /api/v1/goods-receipts/{id}/comments`
- `POST /api/v1/goods-receipts/{id}/comments`
- `GET /api/v1/goods-receipts/{id}/attachments`
- `GET /api/v1/goods-receipts/{id}/audit-log`
- `GET /api/v1/goods-receipts/{id}/linked-inventory-movements`
- `GET /api/v1/goods-receipts/{id}/linked-qc-records`

---

## 32. Validation / Action Rules on This Page
### Edit
- only allowed in Pending by permitted receiving role

### Mark Received
- record must be Pending
- source PO must be Approved
- quantities must be valid
- warehouse/location must be valid

### Send to QC
- record must be Received
- at least one line must require QC or policy must allow full-QC routing

### Accept
- status must be Received or QC Pending, depending on design
- quantity reconciliation must pass
- QC conditions must pass if applicable

### Reject
- reason must be provided
- rejected quantity must be explicit
- stock posting restrictions must pass

### Cancel
- only before irreversible stock impact if policy requires

---

## 33. Suggested UI Components
Recommended reusable components:
- RecordStatusHeader
- SourceReferenceCard
- ReceiptSummaryCard
- LineItemGrid
- QCDispositionPanel
- StockImpactPanel
- LinkedRecordsPanel
- CommentThreadPanel
- AttachmentPanel
- AuditTrailPanel
- ActionBar
- ConfirmDialog
- RejectReasonModal

---

## 34. Export / Print Behavior
### Print
Useful for receipt confirmation and warehouse documentation.

### Export
Allowed only for authorized roles.

### Rule
Printed/exported GRN should reflect final recorded receipt state and respect access rules.

---

## 35. Notification / Deep Link Behavior
This page should support direct opening from:
- receipt-created notification
- QC pending notification
- GRN accepted/rejected notification
- stock-posted notification
- PO linked receipt navigation

Deep links should preserve workflow context where useful.

---

## 36. Wireframe Notes
For wireframe execution:
- receipt status and quantity summary should be above the fold
- line items should dominate the main workspace
- QC and stock impact should be visible without excessive clicks
- discrepancy indicators should stand out
- source PO and linked records should remain easy to access

---

## 37. Frontend Build Notes
### State Handling Needed
- detail fetch state
- pending edit state
- role-aware action rendering
- mutation refresh state
- QC/stock impact refresh
- comment/attachment refresh
- linked record refresh

### Edit Mode Suggestion
- editable only in Pending
- auto-lock after receipt is finalized
- preserve unsaved-change warning

---

## 38. Backend Build Notes
Backend should ensure:
- detail payload includes receipt, QC, and stock-impact context
- quantity reconciliation is enforced centrally
- linked stock and QC references are efficient
- comments/attachments are scoped properly
- mark-received/accept/reject/cancel enforce lifecycle rules
- audit log captures all operationally important changes

---

## 39. Dependencies
This page depends on:
- GRN transaction design
- PO transaction linkage
- store and QA role definitions
- QC support flow
- inventory posting logic
- comments/attachments/audit support
- Step 9 frontend page standards

---

## 40. Completion Criteria
GRN Detail page design is complete when:
- header and action cluster are fixed
- source PO, receipt summary, line items, QC, and stock impact sections are clear
- role-based and status-based actions are defined
- comments/attachments/audit/linked records are defined
- API dependencies are listed
- PO → GRN → stock/QC path is explicit

---

## 41. Next Related Files
Recommended next files:
1. role_store_user.md
2. role_qa_inspector.md
3. connection_document_to_document.md
4. connection_page_to_page.md
