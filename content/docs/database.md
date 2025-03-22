# Database Guide

## Table of Contents

- [Overview](#overview)
- [Configuration](#configuration)
- [Model Setup](#model-setup)
- [Query Optimization](#query-optimization)
- [Relationships](#relationships)
- [Advanced Features](#advanced-features)

## Overview

Shadow provides powerful database integration features that extend Laravel's Eloquent ORM with additional functionality for API development.

## Configuration

### Basic Setup

```php
// config/shadow.php
return [
    'database' => [
        'connection' => env('DB_CONNECTION', 'mysql'),
        'query_log' => env('SHADOW_QUERY_LOG', false),
        'slow_query_threshold' => env('SHADOW_SLOW_QUERY_THRESHOLD', 1000),
        'max_execution_time' => env('SHADOW_MAX_EXECUTION_TIME', 30),
    ],
];
```

### Environment Variables

```env
DB_CONNECTION=mysql
SHADOW_QUERY_LOG=true
SHADOW_SLOW_QUERY_THRESHOLD=1000
SHADOW_MAX_EXECUTION_TIME=30
```

## Model Setup

### Basic Model Configuration

```php
use ThinkNeverland\Shadow\Traits\HasShadow;

class User extends Model
{
    use HasShadow;

    protected $shadowConfig = [
        'searchable' => ['name', 'email'],
        'filterable' => ['status', 'role'],
        'sortable' => ['created_at', 'updated_at'],
        'includes' => ['profile', 'posts'],
    ];
}
```

### Field Configuration

```php
protected $fields = [
    'name' => [
        'type' => 'string',
        'searchable' => true,
        'filterable' => true,
        'validation' => 'required|string|max:255',
    ],
    'email' => [
        'type' => 'email',
        'unique' => true,
        'validation' => 'required|email|unique:users',
    ],
];
```

## Query Optimization

### Indexing Strategy

```php
// In migration
Schema::create('users', function (Blueprint $table) {
    $table->id();
    $table->string('name')->index();
    $table->string('email')->unique();
    $table->enum('status', ['active', 'inactive'])->index();
    $table->timestamps();
    
    // Composite index
    $table->index(['status', 'created_at']);
});
```

### Query Cache

```php
// Enable query cache
protected $shadowConfig = [
    'cache' => [
        'enabled' => true,
        'ttl' => 3600,
    ],
];

// Use query cache
$users = $shadow->model(User::class)
    ->cache()
    ->get();
```

### Eager Loading

```php
// Configure default eager loading
protected $with = ['profile'];

// Dynamic eager loading
$users = $shadow->model(User::class)
    ->with(['posts' => function($query) {
        $query->latest();
    }])
    ->get();
```

## Relationships

### Defining Relationships

```php
class User extends Model
{
    use HasShadow;

    public function profile()
    {
        return $this->hasOne(Profile::class);
    }

    public function posts()
    {
        return $this->hasMany(Post::class);
    }

    protected $shadowConfig = [
        'relationships' => [
            'profile' => [
                'type' => 'hasOne',
                'model' => Profile::class,
                'searchable' => true,
            ],
            'posts' => [
                'type' => 'hasMany',
                'model' => Post::class,
                'filterable' => true,
            ],
        ],
    ];
}
```

### Nested Relationships

```php
// Configure nested relationships
protected $shadowConfig = [
    'includes' => [
        'posts' => [
            'comments' => [
                'user',
            ],
        ],
    ],
];
```

## Advanced Features

### Soft Deletes

```php
use ThinkNeverland\Shadow\Traits\HasShadow;
use Illuminate\Database\Eloquent\SoftDeletes;

class User extends Model
{
    use HasShadow, SoftDeletes;

    protected $shadowConfig = [
        'soft_deletes' => true,
        'force_delete' => false,
    ];
}
```

### Custom Query Scopes

```php
class User extends Model
{
    public function scopeActive($query)
    {
        return $query->where('status', 'active');
    }

    protected $shadowConfig = [
        'scopes' => [
            'active' => true,
            'recent' => ['days' => 7],
        ],
    ];
}
```

### Query Filters

```php
// Define custom filters
protected $shadowConfig = [
    'filters' => [
        'status' => StatusFilter::class,
        'date_range' => DateRangeFilter::class,
    ],
];

// Custom filter class
class StatusFilter implements Filter
{
    public function apply($query, $value)
    {
        return $query->where('status', $value);
    }
}
```

### Events and Observers

```php
// Register model observers
protected $shadowConfig = [
    'observers' => [
        UserObserver::class,
    ],
];

// Model observer
class UserObserver
{
    public function created(User $user)
    {
        // Handle after create
    }

    public function updated(User $user)
    {
        // Handle after update
    }
}
```

## Examples

### 1. Complex Queries

```php
$users = $shadow->model(User::class)
    ->withFilters()
    ->withIncludes(['profile', 'posts.comments'])
    ->withSort('-created_at')
    ->cache()
    ->paginate(config('shadow.pagination.per_page', 15));
```

### 2. Batch Operations

```php
// Batch update with pagination
$shadow->model(User::class)
    ->where('status', 'inactive')
    ->paginate(config('shadow.pagination.per_page', 15))
    ->through(function($user) {
        $user->update(['status' => 'active']);
    });

// Or use chunking for very large datasets
$shadow->model(User::class)
    ->where('status', 'inactive')
    ->chunk(100, function($users) {
        foreach ($users as $user) {
            $user->update(['status' => 'active']);
        }
    });
```

### 3. Custom Query Builder

```php
class UserQueryBuilder extends QueryBuilder
{
    public function whereActive()
    {
        return $this->where('status', 'active');
    }

    // Add custom pagination method if needed
    public function paginateResults(int $perPage = null)
    {
        $perPage = $perPage ?? config('shadow.pagination.per_page', 15);
        return $this->paginate($perPage);
    }
}

// Use custom query builder
protected $shadowConfig = [
    'query_builder' => UserQueryBuilder::class,
];
```

## Performance Tips

1. Use appropriate indexes for frequently queried columns
2. Implement caching for frequently accessed data
3. Use eager loading to prevent N+1 queries
4. Configure pagination settings in `config/shadow.php`
5. Use chunking for processing large datasets
6. Optimize relationship queries with field selection
7. Monitor query performance with logging
8. Consider implementing request caching for read-heavy endpoints

## Related Documentation

- [Performance Optimization](performance.md)
- [API Documentation](api.md)
- [Configuration Guide](config.md)
