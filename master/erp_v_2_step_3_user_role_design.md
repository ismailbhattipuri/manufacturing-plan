# ERP V2 – Step 3: User Role Design (RBAC + Access Control)

## 1. Purpose
This document defines how users, roles, modules, permissions, and access assignment work in the ERP system.

The access model must be scalable, easy to manage, and suitable for enterprise use.

---

## 2. Core Access Design

The ERP access structure follows this hierarchy:

1. Create Modules
2. Create Permissions
3. Create Roles
4. Create Users
5. Assign Modules to Roles
6. Assign Permissions to Role-Module combinations
7. Assign Roles to Users

This means:
- A **user** gets access through a **role**
- A **role** gets access to selected **modules**
- Each selected **module** for that role gets specific **permissions**
- Permissions define allowed actions such as read, create, update, delete, approve, export, print, and manage

---

## 3. Main Entities

### 3.1 Module
Represents a major functional area of the ERP.

Examples:
- Master Data
- Procurement
- Inventory
- Production
- Quality
- Sales
- Dispatch
- Finance
- Reports
- Admin / RBAC

### 3.2 Permission
Represents an allowed action inside a module.

Standard permissions:
- Read
- Create
- Update
- Delete
- Approve
- Reject
- Export
- Print
- Manage

### 3.3 Role
Represents a bundle of module access and permissions.

Examples:
- Procurement Executive
- Store Manager
- Production Supervisor
- Finance Officer
- QA Manager
- System Admin

### 3.4 User
Represents an actual system user.

A user may:
- Have one or more roles
- Inherit module access from assigned role(s)
- Have status: Active / Inactive

---

## 4. Access Logic

### 4.1 Module Assignment to Role
A role does not automatically get all modules.
Modules must be explicitly assigned to the role.

Example:
- Role: Procurement Executive
- Assigned Modules:
  - Procurement
  - Vendor Master
  - Reports

### 4.2 Permission Assignment to Role + Module
After a module is assigned to a role, permissions are configured for that module.

Example:
- Role: Procurement Executive
- Module: Procurement
- Permissions:
  - Read = Yes
  - Create = Yes
  - Update = Yes
  - Delete = No
  - Approve = No
  - Export = Yes

### 4.3 User Assignment
A role is assigned to a user.
The user gets all allowed module access and permissions from the assigned role.

Example:
- User: Amit Sharma
- Assigned Role: Procurement Executive
- Result:
  - Can access Procurement module
  - Can create PR/PO
  - Cannot approve PR/PO

---

## 5. Recommended Access Model

### 5.1 Entity Relationship
The access model should be designed as:

- User
- Role
- Module
- Permission
- RoleModuleAccess
- UserRole

### 5.2 Relationship Summary
- One role can be assigned to many users
- One user can have one or more roles
- One role can have many modules
- One module can have many permissions
- Permissions are applied at role-module level

---

## 6. Functional Components

### 6.1 Module Management
Admin can:
- Create module
- Edit module
- Activate / deactivate module
- Define module code and name
- Set parent/child module structure if needed

### 6.2 Permission Management
Admin can:
- Define permission list
- Activate / deactivate permissions
- Standardize reusable permissions across modules

### 6.3 Role Management
Admin can:
- Create role
- Edit role
- Activate / deactivate role
- Assign modules to role
- Assign permissions for each role-module combination

### 6.4 User Management
Admin can:
- Create user
- Edit user
- Activate / deactivate user
- Assign one or more roles
- Reset password / login access

---

## 7. Permission Types

### 7.1 Standard Action Permissions
- Read
- Create
- Update
- Delete

### 7.2 Business Control Permissions
- Approve
- Reject
- Cancel
- Reopen
- Close

### 7.3 Operational Permissions
- Export
- Print
- Upload
- Download

### 7.4 Administrative Permissions
- Manage
- Configure
- Assign Access

---

## 8. Enterprise Rules

### 8.1 Principle of Least Privilege
Users should receive only the minimum access required.

### 8.2 Role-Based Access Only
Direct page/action permission should not usually be assigned to users individually.
Use roles as the main control layer.

### 8.3 Approval Separation
A creator should not automatically be allowed to approve the same transaction unless business rules explicitly allow it.

### 8.4 Inactive Control
Inactive users, roles, modules, or permissions must not grant access.

### 8.5 Auditability
Every access assignment change must be logged.

---

## 9. Suggested Role Examples

### 9.1 Procurement Executive
Modules:
- Procurement
- Vendor Master
Permissions:
- Read, Create, Update

### 9.2 Procurement Manager
Modules:
- Procurement
- Vendor Master
- Reports
Permissions:
- Read, Create, Update, Approve, Export

### 9.3 Store User
Modules:
- Inventory
- GRN
- Stock Transfer
Permissions:
- Read, Create, Update

### 9.4 QA Manager
Modules:
- Quality
- Reports
Permissions:
- Read, Approve, Reject, Export

### 9.5 System Admin
Modules:
- Admin / RBAC
- All required modules
Permissions:
- Full access

---

## 10. Recommended Screens

### 10.1 Module Master Screen
Fields:
- Module Name
- Module Code
- Parent Module
- Status

### 10.2 Permission Master Screen
Fields:
- Permission Name
- Permission Code
- Description
- Status

### 10.3 Role Master Screen
Fields:
- Role Name
- Role Code
- Description
- Status

### 10.4 User Master Screen
Fields:
- Full Name
- Email / Username
- Employee ID
- Department
- Status

### 10.5 Role Access Matrix Screen
Functions:
- Select role
- Select modules
- Assign permissions per module
- Save access matrix

### 10.6 User Role Assignment Screen
Functions:
- Select user
- Assign one or more roles
- View effective access

---

## 11. Future Enterprise Extensions

These are recommended for deeper production design:
- Plant-wise role restriction
- Warehouse-wise restriction
- Department-wise data visibility
- Approval limits by transaction amount
- Temporary delegated access
- Segregation of duties checks
- Menu-level and action-level access control

---

## 12. Next Step

Proceed to:
👉 Step 4 – Process Flow Design (module-wise transaction flow with statuses, approvals, exceptions, and handoff points)

