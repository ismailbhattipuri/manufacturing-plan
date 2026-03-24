# Step 7 – Database Design

## Objective
Define the full database architecture for the V2 ERP system so that every approved module, process flow, role rule, and screen interaction is backed by a clear, scalable, auditable, and implementation-ready data model.

This step translates business structure into data structure. It should specify what entities exist, how they relate, what core attributes they store, which records drive transactions, how approvals and auditability are captured, and how role-based access and reporting can be supported reliably.

## Scope of Step 7
Step 7 should cover database design at both conceptual and logical levels. It should not stop at table naming. It must define:
- master entities
- transactional entities
- relationship mappings
- status models
- approval data structures
- user / role / permission linkage
- audit trail structure
- attachment and comment structure
- configuration / settings tables
- reference data and lookup tables
- integration-ready identifiers
- soft delete / archival strategy
- reporting considerations
- multi-branch / multi-department / multi-location support where relevant

## Required Inputs
Before database design begins, the following artifacts should be treated as source truth:
1. ERP Planning
2. Module Design
3. User Role Design
4. Process Flow Design
5. Screen Map
6. Wireframe Design

## Core Database Design Principles
- Normalize core business entities, but avoid over-fragmentation that harms operational performance
- Separate master data from transactional data clearly
- Capture every business-critical status as structured data, not UI-only logic
- Store approval history as records, not overwritten fields
- Design for auditability from day one
- Use consistent primary/foreign key conventions across modules
- Support role-based data visibility through relational structure where possible
- Anticipate reporting and dashboard needs early
- Keep lookup/reference values configurable where the business may evolve
- Support soft deletion and archival rather than destructive deletion for critical records
- Prefer explicit relationship tables over ambiguous JSON blobs for core ERP logic
- Ensure every transaction can be traced to creator, updater, approver, timestamps, and source context

## Expected Outputs of Step 7
This step should produce:
- entity list by module
- database domain grouping
- ERD-ready relationship map
- table-by-table logical schema
- primary key / foreign key structure
- status and workflow model
- approval schema
- audit schema
- notes on indexing and reporting needs
- notes on optional partitioning / archival strategy
- integration and external ID strategy

## Database Design Layers

### 1. Conceptual Data Model
High-level business entities and their relationships.
Examples:
- User
- Role
- Employee
- Department
- Vendor
- Customer
- Product
- Warehouse
- Purchase Request
- Purchase Order
- Inventory Transaction
- Invoice
- Payment
- Approval Record
- Comment
- Attachment

### 2. Logical Data Model
Translate conceptual entities into implementation-oriented tables with key fields and relationships.
Examples:
- purchase_requests
- purchase_request_items
- purchase_request_approvals
- purchase_orders
- purchase_order_items
- inventory_movements
- invoice_headers
- invoice_line_items

### 3. Cross-Cutting System Tables
System-wide structures that multiple modules depend on.
Examples:
- users
- roles
- permissions
- user_role_assignments
- branches
- departments
- locations
- statuses
- comments
- attachments
- audit_logs
- notifications
- workflow_instances

## Recommended Database Domains
Organize the schema into domains so the system remains maintainable.

### A. Identity and Access Domain
Covers authentication-adjacent and authorization structures.
Typical tables:
- users
- roles
- permissions
- role_permissions
- user_roles
- user_branch_access
- user_department_access
- session_logs (optional)

### B. Organization Structure Domain
Represents the company structure.
Typical tables:
- companies
- branches
- departments
- teams
- designations
- cost_centers
- locations

### C. HR / People Domain
Typical tables:
- employees
- employee_profiles
- employee_documents
- employee_bank_accounts
- employee_leave_requests
- attendance_logs
- payroll_runs

### D. Procurement Domain
Typical tables:
- vendors
- vendor_contacts
- purchase_requests
- purchase_request_items
- purchase_request_approvals
- quotations
- quotation_items
- purchase_orders
- purchase_order_items
- goods_receipts
- goods_receipt_items

### E. Inventory Domain
Typical tables:
- products
- product_categories
- units_of_measure
- warehouses
- bins
- stock_balances
- inventory_movements
- stock_adjustments
- stock_transfers

### F. Sales / CRM Domain
If in scope.
Typical tables:
- customers
- customer_contacts
- sales_orders
- sales_order_items
- shipments
- shipment_items
- invoices
- invoice_items
- collections

### G. Finance Domain
Typical tables:
- chart_of_accounts
- journal_entries
- journal_entry_lines
- invoices
- payments
- receipts
- expense_claims
- tax_rules

### H. Workflow and Collaboration Domain
Cross-module action history and collaboration.
Typical tables:
- workflow_definitions
- workflow_steps
- workflow_instances
- workflow_instance_steps
- approvals
- comments
- attachments
- notifications
- tasks

### I. Platform / System Domain
Technical and operational support tables.
Typical tables:
- audit_logs
- status_definitions
- reference_values
- import_jobs
- export_jobs
- integration_events
- webhooks
- background_job_logs

## Standard Table Design Template
For each table, document the following:

### 1. Table Name
Use consistent plural snake_case naming.

### 2. Purpose
What business object or event the table represents.

### 3. Category
Mark as:
- master data
- transaction header
- transaction line item
- mapping table
- workflow table
- system table
- reporting/helper table

### 4. Primary Key
Define the primary key field.
Example:
- id (UUID or bigint)

### 5. Foreign Keys
List linked parent tables and dependency rules.

### 6. Core Columns
Document business-critical fields only first, then optional fields.
Include:
- business identifier
- name/title
- status
- owner/requester/approver ids
- amounts/quantities/dates
- source references
- timestamps

### 7. Status Fields
Clarify lifecycle statuses.
Example:
- draft
- submitted
- in_review
- approved
- rejected
- cancelled
- completed
- archived

### 8. Audit Columns
Standard audit columns should exist broadly:
- created_at
- created_by
- updated_at
- updated_by
- deleted_at
- deleted_by
- version_no (optional for optimistic locking)

### 9. Constraints
Examples:
- unique business code
- non-null required fields
- quantity > 0
- amount >= 0
- end_date >= start_date

### 10. Indexing Notes
Document search, filter, and join-heavy fields.
Examples:
- status
- created_at
- requester_id
- vendor_id
- warehouse_id
- branch_id
- transaction_date

### 11. Business Rules
Examples:
- purchase order cannot exist without approved purchase request if policy requires
- invoice amount must equal sum of line items
- only one active primary contact per vendor

## Global Naming and Key Conventions
- Use singular business meaning but plural table names
- Use `id` as primary key name consistently
- Use `[entity]_id` for foreign keys
- Use machine-safe codes separately from display names
- Keep `status` standardized where possible, but allow module-specific status definitions when needed
- Prefer UUIDs for distributed-safe identifiers where integration and scale matter; use bigint where simplicity/performance is prioritized
- Maintain both internal PK and business-facing reference number where relevant

Example:
- `purchase_requests.id` -> internal key
- `purchase_requests.request_no` -> business reference

## Master Data vs Transaction Data Guidance

### Master Data
Relatively stable entities that support operations.
Examples:
- products
- vendors
- customers
- employees
- branches
- departments
- warehouses

### Transaction Data
Business events and records that change state over time.
Examples:
- purchase_requests
- purchase_orders
- goods_receipts
- sales_orders
- invoices
- payments
- leave_requests

### Rule
Do not mix master profile columns into transaction tables unless snapshotting is intentionally required.
For example:
- store `vendor_id` in purchase_orders
- optionally store vendor snapshot fields if legal or reporting requirements need historical preservation

## Transaction Modeling Pattern
Use header + line item structure for business transactions.

### Header Table
Stores overall document information.
Examples:
- request_no
- requester_id
- branch_id
- total_amount
- status
- submitted_at

### Line Item Table
Stores item-level detail.
Examples:
- product_id
- description
- quantity
- unit_price
- tax_amount
- line_total

### Benefits
- aligns with ERP document behavior
- supports multi-item records cleanly
- simplifies approval and reporting logic

## Approval and Workflow Modeling
Do not model approvals as a single `approved_by` field only.
Use structured approval history.

### Recommended Tables
- workflow_definitions
- workflow_steps
- workflow_instances
- workflow_instance_steps
- approvals

### Minimum Approval Record Fields
- id
- module_name
- record_id
- workflow_instance_id
- step_name
- approver_user_id
- action_status
- action_taken_at
- action_comment
- sequence_no
- delegated_from_user_id (optional)

### Why
This preserves:
- multi-step approvals
- reassignment/delegation
- rejection history
- return-for-correction actions
- full auditability

## Status Modeling Guidance
Statuses should be queryable and stable.

### Option A: Inline Status Column
Good for simple modules.
Example:
- purchase_requests.status

### Option B: Reference Status Table
Useful if status labels, colors, or transitions vary by module.
Example tables:
- status_definitions
- status_transitions

### Recommendation
Use a standard status column in core tables and a separate status definition/transition structure where workflow complexity requires configurability.

## Audit Trail Design
Critical ERP actions should be auditable.

### Audit Log Table Example
- id
- module_name
- record_id
- action_type
- field_name
- old_value
- new_value
- acted_by
- acted_at
- source_ip (optional)
- user_agent (optional)
- context_json (optional, non-core)

### Track at Minimum
- create
- update
- submit
- approve
- reject
- cancel
- delete/restore
- status change
- assignment change

## Comments and Attachments Design
These should be cross-module where possible.

### comments
- id
- module_name
- record_id
- comment_text
- comment_type
- created_by
- created_at
- parent_comment_id (optional)

### attachments
- id
- module_name
- record_id
- file_name
- file_path_or_url
- mime_type
- file_size
- uploaded_by
- uploaded_at
- document_type

## Multi-Entity Access Design
If the ERP supports multi-branch, multi-location, or multi-department operations, design for it explicitly.

### Recommended Fields on Transaction Tables
- company_id
- branch_id
- department_id
- location_id

### User Scope Tables
- user_branch_access
- user_department_access
- user_location_access

This supports filtered visibility and authorization checks.

## Reporting and Analytics Considerations
Database design should anticipate reporting without corrupting transactional integrity.

### Include reporting-supportive fields such as
- date dimensions on transactions
- denormalized reference codes where justified
- status timestamps
- branch/department ownership
- created/submitted/approved/completed timestamps

### Consider helper structures for later
- materialized views
- summary tables
- reporting schema / warehouse export

But do not replace core normalized transactional modeling with reporting shortcuts too early.

## Archival and Soft Delete Strategy
For business-critical records:
- avoid hard delete by default
- use `deleted_at` and `deleted_by`
- use `is_active` for master data where needed
- archive closed/high-volume records by policy if scale demands it

Examples:
- inactive vendors remain referenced historically
- cancelled transactions remain queryable
- old audit logs may move to archive partitions after retention thresholds

## Integration Readiness
Prepare tables for external integrations from the beginning.

### Include where appropriate
- external_system_id
- external_reference_no
- sync_status
- synced_at
- source_system

### Useful for
- accounting integrations
- payment gateway sync
- biometric attendance sync
- e-commerce sync
- CRM sync

## Recommended Deliverable Structure for Step 7

### Part A. Domain-Wise Entity Inventory
List all entities by module/domain.

### Part B. Relationship Map
Describe one-to-many, many-to-many, and dependent relationships.

### Part C. Table Specifications
One section per table using the standard template.

### Part D. Workflow / Approval Data Model
Cross-module approval and workflow tables.

### Part E. Audit / Attachment / Comment Model
Shared support tables.

### Part F. Access Control Data Model
Users, roles, permissions, and scope mappings.

### Part G. Reporting / Archive / Integration Notes
Operational design notes for engineering.

## Example Table Set: Procurement

### purchase_requests
- purpose: header record for procurement initiation
- key fields: id, request_no, requester_id, branch_id, department_id, request_date, priority, status, justification, submitted_at

### purchase_request_items
- purpose: line items requested under one purchase request
- key fields: id, purchase_request_id, product_id, description, quantity, estimated_unit_cost, estimated_total_cost, required_date

### purchase_request_approvals / approvals
- purpose: approval history for request lifecycle
- key fields: id, module_name, record_id, approver_user_id, action_status, sequence_no, action_taken_at, action_comment

### purchase_orders
- purpose: approved procurement order issued to vendor
- key fields: id, po_no, purchase_request_id, vendor_id, order_date, currency_id, subtotal, tax_amount, total_amount, status

### purchase_order_items
- purpose: ordered items tied to purchase order
- key fields: id, purchase_order_id, product_id, quantity, unit_price, tax_amount, line_total

## Example Table Set: Inventory

### inventory_movements
- purpose: immutable stock movement ledger
- key fields: id, movement_no, product_id, warehouse_id, bin_id, movement_type, source_module, source_record_id, quantity_in, quantity_out, movement_date, status

### stock_balances
- purpose: current on-hand snapshot by product/location
- key fields: id, product_id, warehouse_id, bin_id, on_hand_qty, reserved_qty, available_qty, updated_at

## Example Table Set: Access Control

### users
- id, username, email, password_hash or auth_provider_ref, employee_id, status, last_login_at

### roles
- id, role_name, role_code, description, is_system_role

### permissions
- id, permission_code, permission_name, module_name, action_name

### role_permissions
- id, role_id, permission_id

### user_roles
- id, user_id, role_id, branch_id (optional scoped assignment)

## Database Design Prompt Template
Use this prompt format when converting module design into data model output:

> Design the logical database schema for the **[Module Name]** module of a V2 ERP system. The module supports **[business process summary]** and is used by **[roles]**. Based on the process flow, screen map, and wireframe behavior, identify all master tables, transaction header tables, line item tables, workflow/approval tables, and support tables required. For each table, define purpose, key fields, primary key, foreign keys, statuses, audit columns, and major business rules. Also define relationships with organization, user/role, comments, attachments, and audit logs. Ensure the model supports enterprise auditability, reporting, and role-based operational control.

## Completion Criteria for Step 7
Step 7 is complete when:
- every module has a defined entity set
- all critical relationships are mapped
- transaction documents have header/line-item structure where needed
- role and approval structures are modeled, not implied
- auditability and attachments/comments are covered
- multi-branch/department/location requirements are addressed
- engineering can translate the schema into ERD and migrations without major business ambiguity

## Handoff Note
Once Step 7 is approved, the next step should typically move to API design, backend architecture, or detailed ERD / schema specification depending on the project workflow.

