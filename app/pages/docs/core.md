# Shadow Core Documentation

## Table of Contents

- [Introduction](#introduction)
- [Core Concepts](#core-concepts)
- [Model Integration](#model-integration)
- [Query Building](#query-building)
- [API Integration](#api-integration)
- [Cache Management](#cache-management)
- [Debugging](#debugging)
- [Advanced Usage](#advanced-usage)

## Introduction

Shadow is a powerful Laravel package that provides automatic API generation, model discovery, and advanced query capabilities. It simplifies the process of building scalable Laravel applications by providing a robust foundation for common operations.

## Core Concepts

### The Shadow Service

The `ShadowService` is the central component that manages all core operations, including API handling, model discovery, and CRUD operations:

```php
use Thinkneverland\Shadow\Services\ShadowService;

class UserController extends Controller
{
    protected $shadow;
    
    public function __construct(ShadowService $shadow)
    {
        $this->shadow = $shadow;
    }
    
    public function index()
    {
        return $this->shadow->handleIndex('users');
    }
}
```

### Model Registry

The Model Registry maintains a catalog of all Shadow-enabled models and their configurations:

```php
use Thinkneverland\Shadow\Services\Registry\ModelRegistry;

// Models are automatically discovered and registered
// You can customize the registration with configuration
protected $shadowConfig = [
    'searchable' => ['name', 'email'],
    'cache' => true,
    'cache_ttl' => 3600
];
```

## Model Integration

### Basic Integration

1. Add the HasShadow trait:

```php
use Thinkneverland\Shadow\Traits\HasShadow;

class User extends Model
{
    use HasShadow;
}
```

### Automatic API Generation

When `auto_generate_api` is enabled in the config, RESTful API endpoints are automatically generated for your models:

```php
// config/shadow.php
'auto_generate_api' => true,
'api_route_prefix' => 'shadow-api',
'api_auth_middleware' => ['api'],
```

This creates the following endpoints:

- GET /shadow-api/{model} - List resources
- POST /shadow-api/{model} - Create resource
- GET /shadow-api/{model}/{id} - Show resource
- PUT/PATCH /shadow-api/{model}/{id} - Update resource
- DELETE /shadow-api/{model}/{id} - Delete resource

### Advanced Configuration

```php
class User extends Model
{
    use HasShadow;
    
    protected $shadowConfig = [
        'searchable' => ['name', 'email'],
        'filterable' => ['status', 'role'],
        'sortable' => ['created_at', 'updated_at'],
        
        // Relationship configuration
        'relationships' => [
            'posts' => [
                'fields' => ['title', 'content'],
                'constraints' => [
                    'published' => function($query) {
                        return $query->where('status', 'published');
                    }
                ]
            ]
        ],
        
        // Field configuration
        'fields' => [
            'status' => [
                'type' => 'select',
                'options' => ['active', 'inactive']
            ]
        ],
        
        // Cache configuration
        'cache' => [
            'enabled' => true,
            'ttl' => 3600,
            'tags' => ['users']
        ],
        
        // Exclusions
        'exclude' => [
            'fields' => ['password', 'remember_token'],
            'relationships' => ['audits']
        ]
    ];
}
```

## Query Building

### API Query Parameters

The API supports various query parameters for filtering, sorting, and including relationships:

```
# Filtering
GET /shadow-api/users?filter[status]=active

# Sorting
GET /shadow-api/users?sort=-created_at

# Including relationships
GET /shadow-api/users?include=posts,profile

# Pagination
GET /shadow-api/users?page=1&per_page=10

# Search
GET /shadow-api/users?search=john
```

### Global Search

Shadow provides a powerful global search capability across all registered models:

```php
// Search across all models
$results = $shadow->handleSearch([
    'query' => 'search term',
    'per_model' => 5
]);
```

## Validation

Shadow can automatically infer validation rules from your schema:

```php
// config/shadow.php
'validation' => [
    'infer_schema_rules' => true,
],
```

You can also define custom validation rules in your model:

```php
protected $shadowConfig = [
    'validation' => [
        'rules' => [
            'email' => 'required|email|unique:users',
            'name' => 'required|min:2'
        ]
    ]
];
```

## Response Format

The API response format is customizable through configuration:

```php
// config/shadow.php
'response' => [
    'format' => 'json',
    'success_key' => 'success',
    'error_key' => 'error',
    'pagination' => [
        'per_page',
        'current_page',
        'total',
        'last_page',
    ],
    'meta' => [
        'filters',
        'sorts',
        'includes',
    ]
]
```

## Error Handling

Shadow provides comprehensive error handling with customizable error responses:

```php
// config/shadow.php
'response' => [
    'error_format' => [
        'error' => true,
        'message' => ':message',
        'errors' => ':errors',
        'code' => ':code',
    ]
]
```

## Security

By default, Shadow applies the middleware specified in your configuration:

```php
// config/shadow.php
'api_auth_middleware' => ['api', 'auth:sanctum'],
```

You can also implement model-specific authorization using Laravel's standard authorization features.
