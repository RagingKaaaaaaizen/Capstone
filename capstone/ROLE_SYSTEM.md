# Three-Tier Role System

## Overview
The application now implements a three-tier role system with different levels of access and permissions.

## Roles

### 1. SuperAdmin
**Full control and oversight capabilities**

**Permissions:**
- ✅ Full access to all features
- ✅ Manage all user accounts (create, edit, delete)
- ✅ View audit logs and who added/disposed items
- ✅ Manage other admins
- ✅ All inventory management tasks
- ✅ System configuration
- ✅ View all data and reports

**Access to:**
- Admin Panel
- Account Management
- All CRUD operations
- Audit trails

### 2. Admin
**Day-to-day inventory management**

**Permissions:**
- ✅ Add/dispose items
- ✅ Manage stock levels
- ✅ Create and edit inventory items
- ✅ Manage categories, brands, storage locations
- ✅ View employee data
- ✅ Manage workflows and requests
- ❌ Cannot manage other user accounts
- ❌ Cannot view audit logs

**Access to:**
- Admin Panel (limited)
- Inventory Management
- Stock Operations
- Employee Management
- Workflow Management

### 3. Viewer
**Read-only access for interns and assistants**

**Permissions:**
- ✅ View all inventory data
- ✅ View stock levels
- ✅ View employee information
- ✅ View workflows and requests
- ❌ Cannot modify any data
- ❌ Cannot add/dispose items
- ❌ Cannot access admin functions

**Access to:**
- View Stocks
- View Reports
- Read-only access to all data

## Implementation Details

### Backend Changes
- Updated `role.js` to include three roles
- Modified authorization middleware to support role arrays
- Updated all route permissions
- Default role changed from 'User' to 'Viewer'

### Frontend Changes
- Updated role enum in TypeScript
- Created role-based navigation component
- Updated routing guards
- Added SuperAdmin guard for sensitive functions

### Database Changes
- Default role for new accounts is now 'Viewer'
- First account created becomes 'SuperAdmin'

## Usage Examples

### Creating Users with Different Roles
```javascript
// SuperAdmin can create any role
POST /accounts
{
  "firstName": "John",
  "lastName": "Doe", 
  "email": "john@example.com",
  "password": "password123",
  "role": "SuperAdmin" // or "Admin" or "Viewer"
}

// Admin can only create Admin or Viewer roles
POST /accounts
{
  "firstName": "Jane",
  "lastName": "Smith",
  "email": "jane@example.com", 
  "password": "password123",
  "role": "Admin" // or "Viewer" only
}
```

### Route Protection Examples
```javascript
// Allow SuperAdmin and Admin only
router.post('/items', authorize([Role.SuperAdmin, Role.Admin]), create);

// Allow all roles to view
router.get('/items', authorize([Role.SuperAdmin, Role.Admin, Role.Viewer]), getAll);

// SuperAdmin only
router.get('/audit-logs', authorize([Role.SuperAdmin]), getAuditLogs);
```

## Security Considerations
- SuperAdmin can manage all accounts
- Admin can perform inventory operations but not manage accounts
- Viewer has read-only access
- Role inheritance: SuperAdmin > Admin > Viewer
- All sensitive operations require appropriate role validation 