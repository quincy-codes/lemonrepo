# Authorization Guide

## Table of Contents

- [Overview](#overview)
- [Configuration](#configuration)
- [Permission System](#permission-system)
- [Role Management](#role-management)
- [Policy Implementation](#policy-implementation)
- [Advanced Usage](#advanced-usage)

## Overview

Shadow provides a robust authorization system that integrates with Laravel's built-in authorization features while adding powerful extensions for API-specific needs.

## Configuration

### Basic Setup

```php
// config/shadow.php
return [
    'authorization' => [
        'enabled' => true,
        'model' => 'App\\Models\\User',
        'default_guard' => 'api',
        'permissions' => [
            'cache_enabled' => true,
            'cache_ttl' => 3600,
        ],
    ],
];
```

### Environment Variables

```env
SHADOW_AUTH_ENABLED=true
SHADOW_PERMISSIONS_CACHE_ENABLED=true
SHADOW_PERMISSIONS_CACHE_TTL=3600
```

## Permission System

### Defining Permissions

```php
// Using the Permission model
use ThinkNeverland\Shadow\Models\Permission;

Permission::create([
    'name' => 'view-users',
    'guard_name' => 'api',
    'description' => 'Can view users',
]);
```

### Permission Groups

```php
// Group related permissions
'permission_groups' => [
    'users' => [
        'view-users',
        'create-users',
        'update-users',
        'delete-users',
    ],
],
```

### Checking Permissions

```php
// In controllers or services
if ($user->can('view-users')) {
    // User can view users
}

// Using middleware
Route::middleware('can:view-users')->group(function () {
    // Protected routes
});
```

## Role Management

### Creating Roles

```php
use ThinkNeverland\Shadow\Models\Role;

$role = Role::create([
    'name' => 'admin',
    'guard_name' => 'api',
    'description' => 'Administrator role',
]);

$role->givePermissionTo('view-users', 'create-users');
```

### Assigning Roles

```php
// Assign role to user
$user->assignRole('admin');

// Check role
if ($user->hasRole('admin')) {
    // User is admin
}
```

### Role Hierarchy

```php
// Define role hierarchy
'roles' => [
    'super-admin' => [
        'admin' => [
            'manager' => [
                'user' => [],
            ],
        ],
    ],
],
```

## Policy Implementation

### Resource Policies

```php
namespace App\Policies;

class UserPolicy
{
    public function view(User $user, User $target)
    {
        return $user->can('view-users') || $user->id === $target->id;
    }

    public function update(User $user, User $target)
    {
        return $user->can('update-users') || $user->id === $target->id;
    }
}
```

### API Resource Authorization

```php
namespace App\Http\Resources;

class UserResource extends JsonResource
{
    public function toArray($request)
    {
        return [
            'id' => $this->id,
            'email' => $this->when(
                $request->user()->can('view-user-email'),
                $this->email
            ),
        ];
    }
}
```

## Advanced Usage

### Custom Permission Validators

```php
namespace App\Authorization;

class CustomPermissionValidator
{
    public function validate($user, $permission)
    {
        // Custom validation logic
        return true;
    }
}
```

### Dynamic Permissions

```php
// Register dynamic permission
$shadow->registerPermission('view-{model}', function($user, $model) {
    return $user->hasRole('admin') || $user->owns($model);
});
```

### Caching Strategies

```php
// Configure permission caching
'cache' => [
    'store' => 'redis',
    'prefix' => 'shadow_permissions',
    'ttl' => 3600,
],
```

### Field-Level Authorization

```php
// Define field-level permissions
protected $fieldPermissions = [
    'email' => 'view-user-email',
    'phone' => 'view-user-phone',
];

// Check field permission
if ($user->canViewField('users', 'email')) {
    // Show email field
}
```

## Examples

### 1. Basic Role-Based Access

```php
// routes/api.php
Route::middleware(['auth:sanctum', 'role:admin'])->group(function () {
    Route::get('/admin/dashboard', 'AdminController@dashboard');
});
```

### 2. Complex Permission Checks

```php
// Multiple permission check
if ($user->hasAllPermissions(['view-users', 'update-users'])) {
    // User has all required permissions
}

// Any permission check
if ($user->hasAnyPermission(['view-users', 'admin'])) {
    // User has at least one permission
}
```

### 3. Resource Authorization

```php
class UserController extends Controller
{
    public function update(Request $request, User $user)
    {
        $this->authorize('update', $user);
        
        // Update user
    }
}
```

## Testing

```php
// Testing authorization
public function test_user_cannot_access_admin_area()
{
    $user = User::factory()->create();
    
    $response = $this->actingAs($user)
        ->get('/admin/dashboard');
    
    $response->assertStatus(403);
}
```

## Related Documentation

- [Authentication Guide](authentication.md)
- [Security Best Practices](security.md)
- [API Documentation](api.md)
