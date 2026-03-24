# Frontend State Management Plan

## 1. Purpose
Defines how frontend state should be structured and managed across the ERP application.

This ensures:
- predictable data flow
- minimal bugs in workflow transitions
- consistent refresh behavior
- separation of server vs UI state
- scalability as modules grow

---

## 2. Folder Location
frontend/frontend_state_management_plan.md

---

## 3. State Categories

## A. Server State
Data fetched from backend APIs.

Examples:
- PR list
- PO detail
- GRN detail
- QC queue
- stock overview

### Rules
- always fetched via query layer
- cached
- invalidated after mutations
- never manually mutated directly

---

## B. Local UI State
Temporary UI behavior state.

Examples:
- modal open/close
- form inputs
- selected rows
- active tab
- filters

### Rules
- page/component owned
- reset on navigation unless preserved intentionally

---

## C. Derived State
Computed from server + UI state.

Examples:
- button visibility
- edit mode enable/disable
- status-based actions
- validation flags

---

## 4. Query Management Strategy

### Use Query Keys
Examples:
- pr.list
- pr.detail
- po.detail
- grn.detail
- qc.queue
- stock.overview

### Rules
- each page has its own keys
- invalidate specific keys, not global

---

## 5. Mutation Handling

### After mutation:
- invalidate related queries
- refetch fresh data
- avoid stale UI

### Example
GRN Accept:
- invalidate grn.detail
- invalidate stock.overview
- invalidate po.detail

---

## 6. Optimistic vs Strict Updates

### Use Strict Updates for:
- approvals
- QC results
- stock changes

### Use Optimistic Updates for:
- comments
- UI toggles

---

## 7. Page State Ownership

Each page owns:
- its queries
- its filters
- its modals
- its action state

Shared components should NOT manage business state.

---

## 8. Form State Strategy

### For Entry Pages (GRN, future forms)
- controlled form state
- validation before submit
- disable submit if invalid

---

## 9. Error Handling

### Types
- API error
- validation error
- network error

### Behavior
- show inline error
- allow retry
- preserve form state

---

## 10. Loading Strategy

### Types
- full page loading
- section loading
- action loading

---

## 11. Refresh Strategy

### Always refresh after:
- submit
- approve/reject
- QC result
- stock change

---

## 12. Completion Criteria
- server vs UI state clearly separated
- query keys defined
- mutation refresh defined
- no direct state mutation of server data

---

## 13. Next Files
- frontend_folder_structure_code_plan.md
- frontend_build_sequence_sprint_plan.md
