# Role File – System Admin

## 1. Role Name
**System Admin**

## 2. Role Code
`ROLE_SYSTEM_ADMIN`

## 3. Role Purpose
The System Admin is responsible for configuring, governing, and maintaining ERP platform access, master configuration, user-role relationships, and core administrative control functions across the ERP landscape.

This role is the primary owner of:
- user management
- role and permission management
- module access management
- configuration governance
- access auditability
- high-level platform support activities

This role is not intended to replace normal business approvers unless explicitly allowed by business policy.

---

## 4. Source Alignment
This role file is derived from and must remain aligned with:
- Step 1 – ERP scope, stakeholders, and control principles
- Step 2 – module inventory and Admin / RBAC module definition
- Step 3 – RBAC architecture, permissions, role/user assignment model, and System Admin example
- Step 4 – maker-checker, approval separation, and status integrity principles
- Step 5 – Admin / RBAC screens and navigation rules
- Step 6 – page-level wireframe expectations for role-based visibility and no-access states
- Step 8 – identity/access APIs, permission APIs, and authorization enforcement
- Step 9 – frontend role-aware navigation and permission rendering

---

## 5. Business Responsibility
The System Admin is responsible for:
- creating and maintaining modules in the RBAC structure
- creating and maintaining permission definitions
- creating and maintaining roles
- creating and maintaining users
- assigning roles to users
- assigning modules and permissions to roles
- activating/deactivating roles, users, modules, and permissions
- monitoring access structure consistency
- supporting auditability of access changes
- managing role-based access setup before users start operational work

The System Admin is **not automatically the operational approver** for procurement, production, quality, sales, dispatch, or finance workflows unless such authority is intentionally granted by business policy.

---

## 6. Primary Modules Accessible
### Full Administrative Ownership
- Admin / RBAC

### Broad System-Level Visibility
The System Admin may be granted broad visibility across all major business modules for support, troubleshooting, and configuration verification:
- Dashboard
- Master Data
- Procurement
- Inventory
- Production
- Quality
- Sales
- Dispatch
- Finance
- Reports

### Important Restriction
Operational access across business modules should be separated into two possible modes:

#### Mode A – Support Visibility Only (recommended default)
- Read access to business records
- Export where approved by policy
- No transactional approval authority by default
- No posting/cancellation authority by default

#### Mode B – Extended Administrative Control (exception mode)
- Used only for controlled super-admin environments
- Must be approved by business leadership
- Must be audit logged aggressively

Recommended default for production ERP:
**Admin has full RBAC/configuration control, but only controlled business-operation power.**

---

## 7. Pages Accessible

### 7.1 Admin / RBAC Pages
Full access:
- Module Master
- Permission Master
- Role Management
- User Management
- Role Access Matrix
- User Role Assignment
- Access Audit / Assignment History *(to be added if not yet created)*

### 7.2 Dashboard Pages
- Overview Dashboard
- KPI Widgets
- Admin-focused operational summary widgets *(future)*

### 7.3 Business Module Pages
Access depends on configured admin mode.

Recommended minimum visibility:
- list pages across modules
- detail pages across modules
- reports pages for validation/support

Recommended default restrictions:
- no direct approval queue ownership for business workflows
- no approval buttons unless explicit business permission exists
- no hidden bypass actions

---

## 8. Dashboard Visibility
The System Admin dashboard should focus on platform control rather than only business KPIs.

### 8.1 Core Dashboard Widgets
- active users count
- inactive users count
- active roles count
- active modules count
- permission assignment summary
- recent access changes
- failed login / locked account summary *(if available)*
- pending user setup tasks
- pending role assignment tasks

### 8.2 Optional Cross-System Widgets
- recently created transactions by module
- integration/notification failures *(future)*
- high-priority system alerts
- records blocked by missing access assignment

### 8.3 Quick Actions
- create user
- create role
- update role access matrix
- reset user access
- open audit history

---

## 9. Permission Profile

### 9.1 Standard Permission Coverage in Admin / RBAC
System Admin has full access to Admin / RBAC functions including:
- Read
- Create
- Update
- Delete *(soft delete or controlled deactivate preferred)*
- Approve *(where approval exists for administrative changes)*
- Reject *(if approval-controlled access change flow exists)*
- Export
- Print
- Upload
- Download
- Manage
- Configure
- Assign Access

### 9.2 Recommended Business Module Permission Baseline
For non-admin modules, recommended default is:
- Read = Yes
- Create = No
- Update = No
- Delete = No
- Approve = No
- Reject = No
- Cancel = No
- Close = No
- Reopen = No
- Export = Policy-based
- Print = Policy-based

### 9.3 Exception-Based Extension
Where System Admin is intentionally empowered to perform corrective business actions, those permissions must be granted explicitly module-by-module and action-by-action.

---

## 10. Action Rights by Area

### 10.1 Module Master
Allowed:
- create module
- edit module name/code/status
- define parent-child structure
- activate/deactivate module
- view module usage impact

Restricted:
- deleting a module physically after it is used in live assignments should not be allowed

### 10.2 Permission Master
Allowed:
- create permission definitions
- edit permission name/code/description
- activate/deactivate permissions
- standardize reusable permissions

Restricted:
- deleting historically used permission definitions should not be allowed if already referenced

### 10.3 Role Management
Allowed:
- create role
- edit role metadata
- activate/deactivate role
- clone role *(recommended future action)*
- view role usage count

Restricted:
- deleting roles with active user assignments should not be allowed

### 10.4 Role Access Matrix
Allowed:
- assign modules to role
- assign permissions by module
- remove permissions by module
- save access matrix
- compare role versions *(future)*

### 10.5 User Management
Allowed:
- create user
- edit user profile fields relevant to access
- activate/deactivate user
- reset login access/password workflow
- assign or remove roles
- view effective access

Restricted:
- direct business-approval privilege should not be silently added through ad hoc user overrides outside role structure unless explicitly designed

### 10.6 System-Wide Administrative Actions
Allowed:
- view access audit logs
- troubleshoot permission issues
- verify page visibility rules
- verify module visibility rules
- inspect effective permissions by user

---

## 11. Approval Authority
### 11.1 Administrative Approval Authority
If access changes require approval in future enterprise design, the System Admin may:
- propose access changes
- review access configurations
- execute approved administrative changes

### 11.2 Business Approval Authority
By default, System Admin should **not** automatically approve:
- PR
- PO
- GRN acceptance
- stock adjustments
- work orders
- QC approval
- SO approval
- invoice approval
- payment posting

### 11.3 Separation Rule
To maintain maker-checker integrity, the System Admin should not bypass business approval chains unless:
- a super-admin emergency policy exists
- the action is explicitly logged
- the authority is approved organizationally

---

## 12. Record Scope Visibility

### 12.1 Admin / RBAC Records
- all roles
- all users
- all modules
- all permissions
- all access assignments
- all assignment history

### 12.2 Business Records
Recommended support visibility:
- all branches/warehouses/departments for diagnosis and setup validation
- read-only by default

### 12.3 Future Scope Controls
Even for admin users, future enterprise design may still require:
- legal entity restrictions
- branch segmentation
- environment-based access (production/test)
- privacy restrictions for sensitive employee or finance data

---

## 13. Field-Level Behavior

### 13.1 Editable Fields in Admin Pages
Editable by System Admin:
- module metadata
- permission metadata
- role metadata
- role-module mappings
- role-permission mappings
- user status
- user-role mappings

### 13.2 Read-Only / Protected Fields
Should usually be read-only or tightly controlled:
- system-generated IDs
- historical audit timestamps
- last login logs
- previously approved audit records
- immutable business reference numbers

### 13.3 Destructive Field Controls
Critical changes should require confirmation and audit logging for:
- deactivation of user
- deactivation of role
- permission removal from role
- removal of active module access

---

## 14. UI Behavior Rules

### 14.1 Sidebar Visibility
System Admin should see:
- Admin / RBAC module always
- all configured modules if granted support visibility
- hidden modules only if active and assigned in role definition

### 14.2 Page Behavior
On Admin pages, the UI should show:
- full create/edit controls
- matrix-based permission editors
- effective access previews
- status chips for active/inactive entities
- warnings when changes impact active users

### 14.3 Button Visibility Rules
Buttons visible to System Admin on admin screens:
- Create
- Edit
- Activate / Deactivate
- Assign Access
- Save Matrix
- Reset Access / Reset Password *(if included)*
- Export

Buttons usually hidden unless explicitly enabled on business screens:
- Approve business transaction
- Reject business transaction
- Close business transaction
- Cancel business transaction

### 14.4 No-Access / Restricted Behavior
If admin lacks extended business permission, business pages should display:
- readable detail if read is allowed
- action buttons hidden or disabled
- clear reason for restriction where necessary

---

## 15. Status-Based Rules

### 15.1 User Status Management
System Admin can manage:
- Active
- Inactive
- Locked *(if auth model supports it)*
- Pending Setup *(future)*

### 15.2 Role Status Management
System Admin can manage:
- Active
- Inactive

### 15.3 Module Status Management
System Admin can manage:
- Active
- Inactive

### 15.4 Permission Status Management
System Admin can manage:
- Active
- Inactive

### 15.5 Business Record Status Handling
For ordinary business records, System Admin should typically only observe status unless explicitly granted business-action permission.

---

## 16. Notifications and Tasks
System Admin should receive notifications for:
- new user creation tasks
- role assignment tasks
- failed access assignment save attempts
- deactivated role impact warnings
- permission conflicts *(future)*
- suspicious access activity *(future)*

Optional admin queue:
- pending access changes
- users without roles
- roles without modules
- inactive modules still referenced by active roles

---

## 17. Cross-Module Touchpoints
The System Admin role touches every module indirectly through access and control.

### Examples
- Procurement: validates whether Procurement Executive / Manager roles can see PR/PO pages correctly
- Inventory: validates stock pages are visible only to intended store roles
- Production: verifies Work Order and Material Issue access mapping
- Quality: verifies QC approval roles
- Sales/Dispatch: verifies order and dispatch role separation
- Finance: verifies invoice/payment access control
- Reports: controls who can export and access analytics

---

## 18. Backend Authorization Notes
Backend must enforce all admin actions regardless of frontend UI visibility.

### Required backend controls
- verify admin role before admin endpoints
- verify specific permission before module/role/user assignment mutation
- block inactive role/module/permission usage
- log all access-control changes in audit trail
- prevent invalid assignment combinations
- prevent deletion where historical dependency exists
- support effective access evaluation endpoint

### Recommended admin-related APIs
- `GET /api/v1/users`
- `POST /api/v1/users`
- `PATCH /api/v1/users/{id}`
- `PUT /api/v1/users/{id}/roles`
- `GET /api/v1/roles`
- `POST /api/v1/roles`
- `PUT /api/v1/roles/{id}/permissions`
- `GET /api/v1/permissions`
- `GET /api/v1/auth/me`

---

## 19. Frontend Build Notes
Frontend should implement a strong admin workspace for this role.

### Key UI needs
- matrix editor for role-permission mapping
- user detail drawer/modal or page
- effective access preview
- active/inactive chips
- confirmation dialogs for deactivation/removal
- audit history tab or side panel
- search/filter for users, roles, permissions, modules

### Important UX guidance
- do not mix business transaction actions into admin workspace unless explicitly granted
- show impact warnings before access changes
- use sticky save areas for large access matrices
- surface inactive/dependent records clearly

---

## 20. Audit Requirements
The following actions must always create audit entries:
- user created
- user updated
- user activated/deactivated
- role created
- role updated
- role activated/deactivated
- module created/updated/activated/deactivated
- permission created/updated/activated/deactivated
- role-permission matrix changed
- user-role assignment changed
- password reset or login access reset *(if supported)*

Audit entry should capture at minimum:
- actor user id
- action type
- target entity type
- target entity id
- old value summary
- new value summary
- timestamp
- source context

---

## 21. Risks and Controls
### Risks
- overpowered admin role bypassing business control
- uncontrolled access changes
- silent permission escalation
- role sprawl
- weak auditability

### Controls
- least privilege by default
- explicit module/action assignment
- strong audit logging
- inactive object enforcement
- confirmation dialogs for sensitive changes
- optional approval for privileged access changes *(future)*

---

## 22. Open Design Notes for Later Deepening
The following may be added in future iterations:
- admin delegation model
- temporary access elevation
- environment-specific admin roles
- segregation of duties conflict engine
- approval limits on access changes
- break-glass emergency access policy
- branch/plant-restricted admin variants

---

## 23. Reusable Role-File Pattern
This file should act as the template reference for all future role files.

Future files should follow the same structure for roles such as:
- Procurement Executive
- Procurement Manager
- Store User
- Store Manager
- Production Planner
- Production Supervisor
- QA Inspector
- QA Manager
- Sales Executive
- Dispatch User
- Finance Officer

---

## 24. Conclusion
The System Admin role is the ERP control role for platform configuration, access governance, and administrative supervision. It must have full administrative authority inside Admin / RBAC, but business-operational powers outside that module should remain explicit, limited, and audit controlled.

This separation protects:
- maker-checker integrity
- business approval chains
- system auditability
- maintainable enterprise governance
