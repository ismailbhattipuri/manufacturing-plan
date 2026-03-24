# Step 8 – Backend API

## Objective
Define the backend API layer for the V2 ERP system so every approved module, workflow, role rule, screen action, and database entity is exposed through secure, predictable, auditable, and implementation-ready service contracts.

This step converts the data model and business workflows into executable backend interfaces. It should define what APIs exist, what each endpoint does, what inputs and outputs are expected, how authorization is enforced, how workflows and approvals are triggered, and how frontend screens interact with backend services consistently.

## Scope of Step 8
Step 8 should cover API design at the service-contract level, not just endpoint naming. It must define:
- module-wise API groups
- CRUD and transactional endpoints
- workflow and approval endpoints
- authentication and authorization rules
- list/query/filter/search behavior
- pagination and sorting patterns
- validation rules and error responses
- file upload/download endpoints
- comments, attachments, and audit endpoints
- dashboard and reporting endpoints
- integration/webhook endpoints where relevant
- status transition APIs
- bulk action APIs
- notification/task APIs where applicable
- versioning and backward-compatibility approach

## Required Inputs
Before backend API design begins, the following artifacts should be treated as source truth:
1. ERP Planning
2. Module Design
3. User Role Design
4. Process Flow Design
5. Screen Map
6. Wireframe Design
7. Database Design

## Core Backend API Design Principles
- Design APIs around business capabilities, not only tables
- Keep module boundaries explicit and stable
- Use predictable RESTful conventions unless a specific workflow requires action-style endpoints
- Separate list endpoints from detail endpoints clearly
- Expose workflow transitions intentionally; do not hide critical status changes behind generic updates
- Make authorization enforceable at endpoint and record-scope level
- Return audit-friendly metadata wherever business actions occur
- Validate aggressively at the API boundary
- Support idempotency for sensitive transaction-creation operations where needed
- Keep response structures consistent across modules
- Design bulk operations explicitly rather than overloading single-record endpoints
- Anticipate dashboard, approval queue, and search/filter needs from the start

## Expected Outputs of Step 8
This step should produce:
- API domain grouping
- module-wise endpoint inventory
- request/response contract structure
- authentication and authorization rules
- validation and error model
- workflow/action endpoint structure
- file/comment/audit endpoint design
- list/filter/pagination/search conventions
- webhook/integration endpoint notes
- backend implementation guidance for services/controllers

## API Architecture Layers

### 1. Public API Surface
The externally consumed service contracts used by frontend apps and integrations.
Examples:
- authentication endpoints
- procurement endpoints
- inventory endpoints
- HR endpoints
- dashboard endpoints
- reporting endpoints

### 2. Application Service Layer
Implements business use cases behind the API.
Examples:
- create purchase request
- submit purchase request
- approve purchase request
- create goods receipt
- transfer stock
- process invoice payment

### 3. Domain Logic Layer
Contains validation, workflow rules, policies, and transactional business behavior.
Examples:
- enforce approval policy
- validate stock availability
- restrict cross-branch data access
- lock records after approval

### 4. Persistence Layer
Maps service actions to the database design from Step 7.
Examples:
- repositories / ORM models / query services
- audit logging
- workflow state persistence
- attachment metadata storage

## Recommended API Domains
Group endpoints by business domain so implementation remains modular.

### A. Identity and Access API
Typical endpoints:
- login
- logout
- refresh token
- current user profile
- change password
- role list
- permission matrix
- user role assignment
- scoped access mapping

### B. Organization Structure API
Typical endpoints:
- companies
- branches
- departments
- teams
- designations
- locations
- cost centers

### C. HR / People API
Typical endpoints:
- employees
- employee documents
- leave requests
- attendance logs
- payroll runs

### D. Procurement API
Typical endpoints:
- vendors
- purchase requests
- quotations
- purchase orders
- goods receipts
- vendor comparisons

### E. Inventory API
Typical endpoints:
- products
- categories
- units of measure
- warehouses
- stock balances
- inventory movements
- stock adjustments
- stock transfers

### F. Sales / CRM API
If in scope.
Typical endpoints:
- customers
- sales orders
- shipments
- invoices
- collections

### G. Finance API
Typical endpoints:
- chart of accounts
- journal entries
- invoices
- payments
- receipts
- expense claims
- tax rules

### H. Workflow and Collaboration API
Cross-module support.
Typical endpoints:
- approvals queue
- workflow instances
- comments
- attachments
- notifications
- tasks
- audit logs

### I. Reporting / Dashboard API
Typical endpoints:
- KPI summaries
- role dashboards
- approval summaries
- branch performance summaries
- module analytics
- export endpoints

## API Design Style Guidance

### Preferred Pattern
Use resource-oriented REST for standard operations and action endpoints for explicit business transitions.

Examples:
- `GET /api/v1/purchase-requests`
- `POST /api/v1/purchase-requests`
- `GET /api/v1/purchase-requests/{id}`
- `PATCH /api/v1/purchase-requests/{id}`
- `POST /api/v1/purchase-requests/{id}/submit`
- `POST /api/v1/purchase-requests/{id}/approve`
- `POST /api/v1/purchase-requests/{id}/reject`
- `POST /api/v1/purchase-requests/{id}/cancel`

### Why
This preserves clarity between:
- data modification
- workflow transition
- approval action
- operational exception handling

## Standard Endpoint Design Template
For each endpoint, document the following:

### 1. Endpoint
Method + path.
Example:
- `GET /api/v1/purchase-orders`

### 2. Purpose
What business need this endpoint serves.

### 3. Module
Which module/domain owns it.

### 4. Access Roles
Who can call it and under what scope.
Examples:
- requester
- approver
- finance officer
- inventory manager
- admin
- read-only auditor

### 5. Request Parameters
Include:
- path params
- query params
- body fields
- file payload rules if applicable

### 6. Validation Rules
Examples:
- required fields
- field format
- quantity > 0
- status preconditions
- branch access required
- attachment required for specific transitions

### 7. Business Logic
Examples:
- generate business reference number
- create line items
- write audit logs
- initiate workflow instance
- notify approver
- lock editing after submission

### 8. Response Structure
Define success response shape with key metadata.
Include:
- record data
- status
- messages
- workflow metadata
- pagination metadata where relevant

### 9. Error Responses
Examples:
- 400 validation error
- 401 unauthenticated
- 403 unauthorized
- 404 not found
- 409 invalid state transition
- 422 business rule violation
- 500 unexpected server error

### 10. Audit / Side Effects
Specify what should be logged or triggered.
Examples:
- audit entry created
- notification sent
- workflow step advanced
- stock ledger updated
- integration event emitted

## Standard Response Envelope
Use a consistent response structure across APIs.

### Example Success Envelope
```json
{
  "success": true,
  "message": "Purchase request submitted successfully",
  "data": {
    "id": "uuid",
    "request_no": "PR-2026-00045",
    "status": "submitted"
  },
  "meta": {
    "workflow_step": "manager_review",
    "timestamp": "2026-03-23T10:15:00Z"
  }
}
```

### Example List Response
```json
{
  "success": true,
  "data": [
    {
      "id": "uuid-1",
      "request_no": "PR-2026-00045",
      "status": "submitted"
    }
  ],
  "meta": {
    "page": 1,
    "per_page": 20,
    "total": 145,
    "sort": "created_at:desc"
  }
}
```

### Example Error Response
```json
{
  "success": false,
  "message": "Invalid state transition",
  "errors": [
    {
      "field": "status",
      "code": "INVALID_TRANSITION",
      "detail": "Only submitted requests can be approved"
    }
  ]
}
```

## Pagination, Filtering, Search, and Sorting Conventions
List APIs should behave consistently.

### Recommended Query Parameters
- `page`
- `per_page`
- `sort`
- `search`
- `status`
- `branch_id`
- `department_id`
- `date_from`
- `date_to`
- `created_by`
- `assignee_id`

### Guidance
- use cursor pagination later only if scale requires it
- keep default list pagination predictable
- support saved filter combinations at application layer if needed
- document searchable fields per endpoint

## Authentication and Authorization Model

### Authentication
Typical options:
- JWT access + refresh tokens
- secure session-based auth for internal apps
- SSO / identity provider integration where required

### Authorization
Enforce at multiple levels:
- endpoint access
- module access
- action-level permission
- branch/department/location scope
- record ownership rules
- approval-stage permission rules

### Examples
- employee can view own leave requests only
- branch manager can approve requests within assigned branch only
- finance can post payments but not modify HR records
- admin can manage role mappings but not bypass audit logs

## Workflow and Approval Endpoint Design
Critical workflow actions should be explicit endpoints.

### Common Action Endpoints
- `POST /api/v1/{module}/{id}/submit`
- `POST /api/v1/{module}/{id}/approve`
- `POST /api/v1/{module}/{id}/reject`
- `POST /api/v1/{module}/{id}/return`
- `POST /api/v1/{module}/{id}/cancel`
- `POST /api/v1/{module}/{id}/close`
- `POST /api/v1/{module}/{id}/reopen`

### Approval Queue Endpoints
- `GET /api/v1/approvals/pending`
- `GET /api/v1/approvals/{module}/{id}/history`
- `POST /api/v1/approvals/{module}/{id}/delegate`

### Request Body Example for Rejection
```json
{
  "reason": "Budget limit exceeded",
  "comment": "Please revise quantity and resubmit"
}
```

### Rule
Rejection, return, cancellation, and delegation should require structured reason/comment fields where business policy demands it.

## CRUD vs Transaction Endpoints
Not all business objects should allow unrestricted update/delete.

### Master Data Endpoints
Usually support standard CRUD.
Examples:
- vendors
- products
- departments
- warehouses

### Transaction Endpoints
Should be more controlled.
Examples:
- create draft
- update draft
- submit
- approve/reject
- cancel
- view history

### Guidance
After submission or approval:
- restrict edits
- move correction through explicit return/revise flows
- preserve auditability

## Bulk Operation APIs
Enterprise screens often require bulk actions.

### Example Endpoints
- `POST /api/v1/purchase-requests/bulk-submit`
- `POST /api/v1/stock-adjustments/bulk-approve`
- `POST /api/v1/employees/bulk-import`
- `POST /api/v1/products/bulk-update-status`

### Rules
Bulk APIs should define:
- max batch size
- partial success behavior
- per-record error reporting
- async handling if volume is high

## File, Comment, and Audit APIs
These are cross-cutting APIs.

### Attachments
- `POST /api/v1/attachments`
- `GET /api/v1/attachments/{id}`
- `DELETE /api/v1/attachments/{id}`
- `GET /api/v1/{module}/{id}/attachments`

### Comments
- `GET /api/v1/{module}/{id}/comments`
- `POST /api/v1/{module}/{id}/comments`
- `PATCH /api/v1/comments/{comment_id}`

### Audit Logs
- `GET /api/v1/{module}/{id}/audit-log`

### Guidance
Always tie comments and attachments to:
- module name
- record id
- user id
- timestamp
- optional visibility type/internal flag where needed

## Dashboard and Reporting APIs
Dashboards should not depend on raw table fetching from the frontend.

### Example Endpoints
- `GET /api/v1/dashboard/me`
- `GET /api/v1/dashboard/approvals-summary`
- `GET /api/v1/reports/procurement-summary`
- `GET /api/v1/reports/inventory-aging`
- `GET /api/v1/reports/finance/cash-flow`
- `POST /api/v1/reports/export`

### Guidance
- provide aggregated, role-scoped data
- keep report filters explicit
- support export generation with tracked job status if heavy

## Integration and Webhook APIs
If third-party systems are expected, define integration-safe contracts.

### Possible Endpoints
- `POST /api/v1/integrations/webhooks/{source}`
- `GET /api/v1/integrations/events`
- `POST /api/v1/integrations/sync/vendors`
- `POST /api/v1/integrations/sync/attendance`

### Include
- signature verification where needed
- retry-safe processing
- event logging
- sync status responses

## API Versioning Strategy
Use explicit versioning from the beginning.

### Recommendation
- `/api/v1/...`

### Why
This makes later contract evolution manageable without breaking existing frontend or integration clients.

## Validation and Error Model
Validation should happen consistently at the API boundary.

### Categories
- schema validation
- business rule validation
- permission validation
- workflow/state validation
- integration validation

### Common Error Codes
- `VALIDATION_ERROR`
- `UNAUTHENTICATED`
- `FORBIDDEN`
- `NOT_FOUND`
- `INVALID_TRANSITION`
- `DUPLICATE_REFERENCE`
- `CONFLICT`
- `ATTACHMENT_REQUIRED`
- `OUT_OF_SCOPE_ACCESS`
- `INSUFFICIENT_STOCK`

## Idempotency and Concurrency Guidance
For sensitive endpoints, plan for duplicate submission protection.

### Consider idempotency for
- payment posting
- bulk imports
- webhook receipt
- purchase order creation from approved source documents

### Consider concurrency protection for
- stock updates
- approval actions
- edit-after-fetch conflicts

Possible mechanisms:
- idempotency keys
- record version number
- optimistic locking
- transactional service methods

## Security and Operational Concerns
Backend API design should note:
- rate limiting for auth/integration endpoints
- file upload validation
- secure download authorization
- PII-sensitive field masking where needed
- audit logging of privileged actions
- structured logs and request tracing

## Recommended Deliverable Structure for Step 8

### Part A. API Domain Inventory
List all API groups by module.

### Part B. Endpoint Catalog
Module-wise endpoint list with method/path/purpose.

### Part C. Request / Response Contracts
Document payloads and envelopes.

### Part D. Authorization Matrix
Map endpoints to roles and scope.

### Part E. Workflow / Approval Actions
Explicit transition APIs.

### Part F. Shared APIs
Comments, attachments, audit logs, notifications, dashboards.

### Part G. Validation / Error / Versioning Notes
Implementation rules for backend engineers.

## Example Endpoint Set: Procurement

### Purchase Requests
- `GET /api/v1/purchase-requests`
- `POST /api/v1/purchase-requests`
- `GET /api/v1/purchase-requests/{id}`
- `PATCH /api/v1/purchase-requests/{id}`
- `POST /api/v1/purchase-requests/{id}/submit`
- `POST /api/v1/purchase-requests/{id}/approve`
- `POST /api/v1/purchase-requests/{id}/reject`
- `POST /api/v1/purchase-requests/{id}/cancel`
- `GET /api/v1/purchase-requests/{id}/history`

### Purchase Orders
- `GET /api/v1/purchase-orders`
- `POST /api/v1/purchase-orders`
- `GET /api/v1/purchase-orders/{id}`
- `PATCH /api/v1/purchase-orders/{id}`
- `POST /api/v1/purchase-orders/{id}/issue`
- `POST /api/v1/purchase-orders/{id}/close`

### Vendors
- `GET /api/v1/vendors`
- `POST /api/v1/vendors`
- `GET /api/v1/vendors/{id}`
- `PATCH /api/v1/vendors/{id}`
- `POST /api/v1/vendors/{id}/activate`
- `POST /api/v1/vendors/{id}/deactivate`

## Example Endpoint Set: Inventory

### Products
- `GET /api/v1/products`
- `POST /api/v1/products`
- `GET /api/v1/products/{id}`
- `PATCH /api/v1/products/{id}`

### Stock Transfers
- `GET /api/v1/stock-transfers`
- `POST /api/v1/stock-transfers`
- `GET /api/v1/stock-transfers/{id}`
- `POST /api/v1/stock-transfers/{id}/submit`
- `POST /api/v1/stock-transfers/{id}/approve`
- `POST /api/v1/stock-transfers/{id}/receive`

### Stock Balances
- `GET /api/v1/stock-balances`
- `GET /api/v1/stock-balances/{product_id}`
- `GET /api/v1/inventory-movements`

## Example Endpoint Set: Access Control

### Auth and Users
- `POST /api/v1/auth/login`
- `POST /api/v1/auth/refresh`
- `POST /api/v1/auth/logout`
- `GET /api/v1/auth/me`
- `GET /api/v1/users`
- `POST /api/v1/users`
- `GET /api/v1/users/{id}`
- `PATCH /api/v1/users/{id}`

### Roles and Permissions
- `GET /api/v1/roles`
- `POST /api/v1/roles`
- `GET /api/v1/permissions`
- `PUT /api/v1/roles/{id}/permissions`
- `PUT /api/v1/users/{id}/roles`

## Backend API Prompt Template
Use this prompt format when converting module design into API output:

> Design the backend API contract for the **[Module Name]** module of a V2 ERP system. The module supports **[business process summary]** and is used by **[roles]**. Based on the process flow, screen map, wireframe behavior, and database schema, define all required endpoints for listing, detail view, creation, draft update, workflow transitions, approvals, comments, attachments, audit logs, dashboard summaries, and bulk actions where relevant. For each endpoint, specify method, path, purpose, access roles, request payload, response shape, validation rules, business logic, and major side effects. Ensure the API design supports enterprise security, auditability, role-based access control, predictable frontend integration, and explicit workflow transitions.

## Completion Criteria for Step 8
Step 8 is complete when:
- every module has a defined endpoint set
- CRUD and workflow actions are separated clearly
- auth and authorization rules are documented
- list/filter/search/pagination conventions are standardized
- comments, attachments, audit, and dashboard APIs are covered
- validation and error handling are defined consistently
- backend engineers can translate the contracts into controllers/services without major business ambiguity

## Handoff Note
Once Step 8 is approved, the next step should typically move to service architecture, detailed endpoint specifications by module, OpenAPI/Swagger documentation, or frontend-backend integration planning depending on the project workflow.

