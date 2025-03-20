# Shadow API Documentation

## Table of Contents

- [Introduction](#introduction)
- [Configuration](#configuration)
- [Authentication](#authentication)
- [Request Format](#request-format)
- [Response Format](#response-format)
- [Endpoints](#endpoints)
- [Query Parameters](#query-parameters)
- [Error Handling](#error-handling)
- [Documentation Generation](#documentation-generation)
- [Help System](#help-system)
- [Additional Resources](#additional-resources)

## Introduction

Shadow provides automatic API generation for your Laravel models. It creates RESTful endpoints with advanced querying capabilities, relationship handling, and comprehensive response formatting.

## Configuration

Enable API generation in your `config/shadow.php` and configure the API prefix:

```php
// .env
SHADOW_API_PREFIX=shadow-api  # Default value if not set

// config/shadow.php
return [
    'auto_generate_api' => true,
    'api_route_prefix' => env('SHADOW_API_PREFIX', 'shadow-api'),
    'api_auth_middleware' => ['api'],
    
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
];
```

All examples in this documentation use the default prefix `/shadow-api`. Replace this with your configured prefix if different.

## Authentication

Authentication is handled through Laravel's built-in authentication system. Configure the middleware in `config/shadow.php`:

```php
'api_auth_middleware' => ['api', 'auth:sanctum']
```

## Request Format

### Headers

Required headers:

```http
Accept: application/json
Content-Type: application/json
```

Optional headers:

```http
X-Request-ID: <unique-request-id>        # For request tracking
X-Client-Version: <client-version>       # For version-specific handling
X-Language: en                           # For localization
Cache-Control: no-cache                  # For cache control
```

### Query Parameters

#### Foreign Key Search

```http
# Create/Update with foreign key search
POST /shadow-api/orders
{
    "company": "Acme Corp",        # Will search and resolve to company_id
    "customer": "john@example.com" # Will search and resolve to customer_id
}

# Configuration in your model:
class Order extends Model
{
    use HasShadow;

    public static function shadowConfig(): array
    {
        return [
            'foreign_keys' => [
                'company_id' => [
                    'model' => Company::class,
                    'search_fields' => ['name', 'code'],
                    'strategy' => 'first',
                    'fuzzy' => true
                ],
                'customer_id' => [
                    'model' => Customer::class,
                    'search_fields' => ['email', 'name'],
                    'strategy' => 'error_if_multiple',
                    'required' => true,
                    'conditions' => [
                        'status' => 'active'
                    ]
                ]
            ]
        ];
    }
}
```

Foreign key search options:

- `model`: Target model class to search in
- `search_fields`: Fields to search (defaults to ['name', 'title', 'label'])
- `strategy`: How to handle multiple matches:
  - `first`: Use first match (default)
  - `latest`: Use most recent match
  - `error_if_multiple`: Error if multiple matches found
- `fuzzy`: Enable fuzzy matching (default: true)
- `required`: Whether the field must resolve to a match
- `conditions`: Additional conditions to filter matches

#### Filtering

```http
# Simple filters
GET /shadow-api/users?filter[status]=active
GET /shadow-api/users?filter[role]=admin

# Multiple filters
GET /shadow-api/users?filter[status]=active&filter[role]=admin

# Range filters
GET /shadow-api/users?filter[created_at][from]=2024-01-01&filter[created_at][to]=2024-12-31
GET /shadow-api/users?filter[age][min]=18&filter[age][max]=65

# Array filters
GET /shadow-api/users?filter[status][]=active&filter[status][]=pending

# Complex filters
GET /shadow-api/users?filter[or][0][status]=active&filter[or][1][role]=admin
```

#### Sorting

```http
# Single field sorting
GET /shadow-api/users?sort=name                 # Ascending
GET /shadow-api/users?sort=-name                # Descending

# Multiple field sorting
GET /shadow-api/users?sort=-created_at,name     # Sort by created_at desc, then name asc

# Relationship sorting
GET /shadow-api/users?sort=posts.count,-email
```

#### Pagination

```http
# Basic pagination
GET /shadow-api/users?page=2&per_page=15

# Cursor pagination
GET /shadow-api/users?cursor=eyJpZCI6MTAwfQ==

# Offset/Limit
GET /shadow-api/users?offset=30&limit=15
```

#### Including Relations

```http
# Single relation
GET /shadow-api/users?include=profile

# Multiple relations
GET /shadow-api/users?include=profile,posts

# Nested relations
GET /shadow-api/users?include=posts.comments,profile.settings

# Filtered relations
GET /shadow-api/users?include=posts:recent
GET /shadow-api/users?include=posts:where(status=published)
```

#### Field Selection

```http
# Select specific fields
GET /shadow-api/users?fields=id,name,email

# Fields for included relations
GET /shadow-api/users?fields[users]=id,name&fields[posts]=title,content

# Excluding fields
GET /shadow-api/users?exclude=password,deleted_at
```

### Example Requests

#### Basic CRUD Operations

```http
# Create
POST /shadow-api/users
{
    "name": "John Doe",
    "email": "john@example.com",
    "role": "user",
    "settings": {
        "notifications": true,
        "theme": "dark"
    }
}

# Read with relations and filters
GET /shadow-api/users?filter[role]=admin
    &include=profile,posts.comments
    &fields[users]=id,name,email
    &fields[posts]=title
    &sort=-created_at
    &page=1
    &per_page=15

# Update with partial data
PATCH /shadow-api/users/123
{
    "settings": {
        "notifications": false
    }
}

# Delete with cascade
DELETE /shadow-api/users/123?cascade=true
```

#### Advanced Operations

```http
# Bulk create with validation
POST /shadow-api/users/bulk
{
    "items": [
        {
            "name": "John Doe",
            "email": "john@example.com"
        },
        {
            "name": "Jane Doe",
            "email": "jane@example.com"
        }
    ],
    "options": {
        "validate": true,
        "skip_duplicates": true
    }
}

# Complex search
POST /shadow-api/users/search
{
    "query": {
        "bool": {
            "must": [
                { "term": { "status": "active" } },
                { "range": { "age": { "gte": 18 } } }
            ],
            "should": [
                { "match": { "name": "John" } },
                { "match": { "email": "john" } }
            ]
        }
    },
    "sort": [
        { "created_at": "desc" },
        "_score"
    ]
}
```

## Response Format

### Success Responses

#### Single Resource

```json
{
    "data": {
        "id": 1,
        "type": "users",
        "attributes": {
            "name": "John Doe",
            "email": "john@example.com",
            "created_at": "2024-01-01T00:00:00Z",
            "updated_at": "2024-01-01T00:00:00Z"
        },
        "relationships": {
            "posts": {
                "data": [
                    { "id": 1, "type": "posts" }
                ]
            }
        },
        "meta": {
            "version": "1.0",
            "timestamps": {
                "created_at": "2024-01-01T00:00:00Z",
                "updated_at": "2024-01-01T00:00:00Z"
            }
        }
    },
    "included": [
        {
            "id": 1,
            "type": "posts",
            "attributes": {
                "title": "First Post",
                "content": "Content here..."
            }
        }
    ]
}
```

#### Collection

```json
{
    "data": [...],
    "meta": {
        "current_page": 1,
        "per_page": 15,
        "total": 100,
        "total_pages": 7,
        "filters_applied": {
            "status": "active",
            "role": "admin"
        },
        "sort_applied": ["-created_at", "name"]
    },
    "links": {
        "first": "/shadow-api/users?page=1",
        "last": "/shadow-api/users?page=7",
        "prev": null,
        "next": "/shadow-api/users?page=2",
        "self": "/shadow-api/users?page=1"
    },
    "included": [...],
    "debug": {
        "query_time": "123ms",
        "cache_hit": true,
        "query_log": [...]
    }
}
```

## Endpoints

For each model with the `HasShadow` trait, the following endpoints are automatically generated:

```
GET    /shadow-api/{model}          # List resources
POST   /shadow-api/{model}          # Create resource
GET    /shadow-api/{model}/{id}     # Show resource
PUT    /shadow-api/{model}/{id}     # Update resource
DELETE /shadow-api/{model}/{id}     # Delete resource
```

### Meta Endpoints

Shadow provides built-in meta endpoints for API discovery and documentation:

```
GET    /shadow-api/models           # List all available Shadow-enabled models
GET    /shadow-api/                 # API documentation homepage
```

#### Models Endpoint

The models endpoint returns a list of all registered Shadow-enabled models:

```http
GET /shadow-api/models
```

Example response:

```json
{
    "success": true,
    "message": "Available models",
    "data": [
        {
            "model": "App\\Models\\User",
            "endpoint": "users",
            "display_name": "Users",
            "description": "User management",
            "fillable": ["name", "email", "password"],
            "searchable": ["name", "email"],
            "relationships": ["posts", "profile"],
            "endpoints": {
                "list": "/shadow-api/users",
                "create": "/shadow-api/users",
                "show": "/shadow-api/users/{id}",
                "update": "/shadow-api/users/{id}",
                "delete": "/shadow-api/users/{id}"
            }
        },
        {
            "model": "App\\Models\\Post",
            "endpoint": "posts",
            "display_name": "Posts",
            "description": "Blog posts",
            "fillable": ["title", "content", "user_id"],
            "searchable": ["title", "content"],
            "relationships": ["user", "comments"],
            "endpoints": {
                "list": "/shadow-api/posts",
                "create": "/shadow-api/posts",
                "show": "/shadow-api/posts/{id}",
                "update": "/shadow-api/posts/{id}",
                "delete": "/shadow-api/posts/{id}"
            }
        }
    ]
}
```

### Help System Endpoints

Shadow provides a comprehensive help system through dedicated endpoints:

```
GET    /shadow-api/help                       # Help system documentation
GET    /shadow-api/help/{model}               # Help for a specific model
GET    /shadow-api/help/resources/{model}/schema    # Get model schema
GET    /shadow-api/help/resources/{model}/actions   # Get available actions
```

#### Help Endpoint

```http
GET /shadow-api/help
```

Returns overall documentation about API usage, authentication, available models, and common query parameters.

#### Model Help Endpoint

```http
GET /shadow-api/help/users
```

Returns detailed documentation about the requested model, including available fields, relationships, validation rules, and examples.

## Query Parameters

### Filtering

```http
# Simple filters
GET /shadow-api/users?filter[status]=active

# Multiple filters
GET /shadow-api/users?filter[status]=active&filter[role]=admin

# Range filters
GET /shadow-api/users?filter[created_at][from]=2024-01-01
```

Configure filterable fields in your model:

```php
protected $shadowConfig = [
    'filterable' => ['status', 'role', 'created_at']
];
```

### Sorting

```http
# Ascending sort
GET /shadow-api/users?sort=name

# Descending sort
GET /shadow-api/users?sort=-name

# Multiple fields
GET /shadow-api/users?sort=-created_at,name
```

Configure sortable fields:

```php
protected $shadowConfig = [
    'sortable' => ['name', 'created_at', 'email']
];
```

### Including Relations

```http
# Single relation
GET /shadow-api/users?include=profile

# Multiple relations
GET /shadow-api/users?include=profile,posts

# Nested relations
GET /shadow-api/users?include=posts.comments
```

Configure relationships:

```php
protected $shadowConfig = [
    'relationships' => [
        'posts' => [
            'fields' => ['title', 'content'],
            'constraints' => [
                'published' => fn($q) => $q->where('status', 'published')
            ]
        ]
    ]
];
```

### Pagination

```http
# Page-based pagination
GET /shadow-api/users?page=1&per_page=15
```

### Search

```http
# Global search
GET /shadow-api/users?search=john

# Field-specific search
GET /shadow-api/users?search[name]=john
```

Configure searchable fields:

```php
protected $shadowConfig = [
    'searchable' => ['name', 'email', 'description']
];
```

## Error Handling

### Error Codes

- `400`: Bad Request
- `401`: Unauthorized
- `403`: Forbidden
- `404`: Not Found
- `422`: Validation Error
- `429`: Too Many Requests
- `500`: Server Error

### Error Response Format

All error responses follow a consistent format:

```json
{
    "error": {
        "code": "error_code",
        "message": "Human readable message",
        "details": {
            "field": ["Error messages"],
            "trace_id": "unique-error-trace-id"
        },
        "debug": {
            "file": "Controller.php",
            "line": 123,
            "trace": [...],
            "query_log": [...]
        }
    }
}
```

### Common Error Codes

#### Client Errors (4xx)

- `400 bad_request`: Malformed request syntax
- `401 unauthorized`: Authentication failed or not provided
- `403 forbidden`: Authenticated but not authorized
- `404 not_found`: Resource not found
- `405 method_not_allowed`: HTTP method not allowed
- `422 validation_failed`: Validation errors
- `429 too_many_requests`: Rate limit exceeded

#### Server Errors (5xx)

- `500 internal_error`: Unexpected server error
- `502 bad_gateway`: Invalid response from upstream server
- `503 service_unavailable`: Service temporarily unavailable
- `504 gateway_timeout`: Upstream server timeout

### Validation Errors

```json
{
    "error": {
        "code": "validation_failed",
        "message": "The given data was invalid.",
        "details": {
            "email": [
                "The email field is required.",
                "The email must be a valid email address."
            ],
            "age": [
                "The age must be at least 18."
            ]
        }
    }
}
```

### Rate Limiting Errors

```json
{
    "error": {
        "code": "too_many_requests",
        "message": "Rate limit exceeded",
        "details": {
            "retry_after": 60,
            "limit": 100,
            "remaining": 0,
            "reset_at": "2024-01-01T00:01:00Z"
        }
    }
}
```

## Documentation Generation

### Command Line Options

```bash
# Basic documentation generation
php artisan shadow:generate-docs

# Generate Swagger documentation
php artisan shadow:generate-docs --format=swagger

# Generate Postman collection
php artisan shadow:generate-docs --format=postman

# Generate with examples
php artisan shadow:generate-docs --include-examples

# Generate for specific resources
php artisan shadow:generate-docs --resources=users,posts

# Custom output
php artisan shadow:generate-docs --output=custom/path --format=markdown

# Generate with authentication examples
php artisan shadow:generate-docs --include-auth-examples

# Generate with test data
php artisan shadow:generate-docs --with-test-data
```

### OpenAPI/Swagger Generation

Shadow can automatically generate OpenAPI (Swagger) specification for your API:

```bash
# Generate Swagger documentation
php artisan shadow:generate-docs --format=swagger --output=public/swagger
```

This creates a Swagger specification at `public/swagger/api-docs.json` which can be viewed with tools like Swagger UI.

### Postman Collection Export

Generate Postman collections for testing your API:

```bash
# Generate Postman collection
php artisan shadow:generate-docs --format=postman --output=postman/collection.json
```

This creates a Postman collection file that you can import into Postman for easy API testing and documentation.

### Output Formats

Shadow supports multiple documentation formats:

- `markdown`: Human-readable Markdown documentation
- `swagger`: OpenAPI specification (JSON/YAML)
- `postman`: Postman collection
- `html`: Static HTML documentation
- `blade`: Laravel Blade templates (customizable)

### Model Documentation Configuration

You can customize the documentation for each model using the `documentation` key in your Shadow configuration:

```php
protected $shadowConfig = [
    'documentation' => [
        'description' => 'User management endpoints',
        'fields' => [
            'name' => [
                'type' => 'string',
                'description' => 'User\'s full name',
                'example' => 'John Doe',
                'validation' => ['required', 'max:255']
            ],
            'email' => [
                'type' => 'string',
                'description' => 'User\'s email address',
                'example' => 'john@example.com',
                'validation' => ['required', 'email', 'unique:users']
            ],
            'role' => [
                'type' => 'enum',
                'description' => 'User\'s role in the system',
                'options' => ['user', 'admin', 'moderator'],
                'default' => 'user'
            ]
        ],
        'relationships' => [
            'posts' => [
                'type' => 'hasMany',
                'model' => Post::class,
                'description' => 'User\'s blog posts'
            ],
            'profile' => [
                'type' => 'hasOne',
                'model' => Profile::class,
                'description' => 'User\'s profile information'
            ]
        ],
        'examples' => [
            'create' => [
                'description' => 'Create a new user',
                'request' => [
                    'method' => 'POST',
                    'path' => '/shadow-api/users',
                    'body' => [
                        'name' => 'John Doe',
                        'email' => 'john@example.com',
                        'role' => 'user'
                    ]
                ],
                'response' => [
                    'status' => 201,
                    'body' => [
                        'data' => [
                            'id' => 1,
                            'name' => 'John Doe',
                            'email' => 'john@example.com'
                        ]
                    ]
                ]
            ]
        ]
    ]
];
```

### Documentation Templates

#### Custom Endpoint Template

```php
// resources/views/vendor/shadow/api/endpoint.blade.php
<div class="endpoint">
    <h3>{{ $endpoint->method }} {{ $endpoint->path }}</h3>
    
    <div class="description">
        {{ $endpoint->description }}
    </div>
    
    <div class="parameters">
        <h4>Parameters</h4>
        @foreach($endpoint->parameters as $param)
            <div class="parameter">
                <code>{{ $param->name }}</code>
                <span class="type">{{ $param->type }}</span>
                @if($param->required)
                    <span class="required">Required</span>
                @endif
                <p>{{ $param->description }}</p>
            </div>
        @endforeach
    </div>
    
    <div class="examples">
        <h4>Examples</h4>
        @foreach($endpoint->examples as $example)
            @include('shadow::api.example', ['example' => $example])
        @endforeach
    </div>
</div>
```

#### Custom Layout

```php
// resources/views/vendor/shadow/api/layout.blade.php
<!DOCTYPE html>
<html>
<head>
    <title>{{ config('shadow.documentation.title') }}</title>
    <link rel="stylesheet" href="{{ asset('vendor/shadow/css/docs.css') }}">
</head>
<body>
    <div class="sidebar">
        @include('shadow::api.sidebar')
    </div>
    
    <div class="content">
        @yield('content')
    </div>
    
    <script src="{{ asset('vendor/shadow/js/docs.js') }}"></script>
</body>
</html>
```

### Model Documentation

```php
class User extends Model
{
    use HasShadow;
    
    protected $shadowConfig = [
        'documentation' => [
            'description' => 'User management endpoints',
            'fields' => [
                'name' => [
                    'type' => 'string',
                    'description' => 'User\'s full name',
                    'example' => 'John Doe',
                    'validation' => ['required', 'max:255']
                ],
                'email' => [
                    'type' => 'string',
                    'description' => 'User\'s email address',
                    'example' => 'john@example.com',
                    'validation' => ['required', 'email', 'unique:users']
                ],
                'role' => [
                    'type' => 'enum',
                    'description' => 'User\'s role in the system',
                    'options' => ['user', 'admin', 'moderator'],
                    'default' => 'user'
                ]
            ],
            'relationships' => [
                'posts' => [
                    'type' => 'hasMany',
                    'model' => Post::class,
                    'description' => 'User\'s blog posts'
                ],
                'profile' => [
                    'type' => 'hasOne',
                    'model' => Profile::class,
                    'description' => 'User\'s profile information'
                ]
            ],
            'examples' => [
                'create' => [
                    'description' => 'Create a new user',
                    'request' => [
                        'method' => 'POST',
                        'path' => '/shadow-api/users',
                        'body' => [
                            'name' => 'John Doe',
                            'email' => 'john@example.com',
                            'role' => 'user'
                        ]
                    ],
                    'response' => [
                        'status' => 201,
                        'body' => [
                            'data' => [
                                'id' => 1,
                                'name' => 'John Doe',
                                'email' => 'john@example.com'
                            ]
                        ]
                    ]
                ]
            ]
        ]
    ];
}
```

## Help System

### Interactive Help Commands

```bash
# Get all available commands
php artisan shadow:help

# Get help for specific resource
php artisan shadow:help users

# Get help for specific action
php artisan shadow:help users:create

# Generate help documentation
php artisan shadow:help --generate
```

### Help Endpoint Examples

#### Get Resource Schema

```http
GET /shadow-api/help/resources/users/schema
```

Response:

```json
{
    "resource": "users",
    "schema": {
        "type": "object",
        "properties": {
            "id": {
                "type": "integer",
                "readOnly": true
            },
            "name": {
                "type": "string",
                "maxLength": 255
            },
            "email": {
                "type": "string",
                "format": "email"
            },
            "role": {
                "type": "string",
                "enum": ["user", "admin", "moderator"],
                "default": "user"
            }
        },
        "required": ["name", "email"]
    },
    "validation_rules": {
        "name": ["required", "string", "max:255"],
        "email": ["required", "email", "unique:users"],
        "role": ["in:user,admin,moderator"]
    }
}
```

#### Get Available Actions

```http
GET /shadow-api/help/resources/users/actions
```

Response:

```json
{
    "resource": "users",
    "actions": {
        "list": {
            "method": "GET",
            "path": "/shadow-api/users",
            "description": "List all users",
            "parameters": {
                "filter": {
                    "type": "object",
                    "properties": {
                        "status": {
                            "type": "string",
                            "enum": ["active", "inactive"]
                        }
                    }
                }
            }
        },
        "create": {
            "method": "POST",
            "path": "/shadow-api/users",
            "description": "Create a new user",
            "request_body": {
                "type": "object",
                "properties": {
                    "name": {
                        "type": "string"
                    },
                    "email": {
                        "type": "string"
                    }
                }
            }
        }
    }
}
```

## Additional Resources

For more detailed information, please refer to our specialized guides:

- [Advanced Usage Guide](advanced-usage.md) - Detailed examples of advanced features and customizations
- [Security Best Practices](security.md) - Comprehensive security implementation guide
- [Core Documentation](CORE.md) - Core functionality and basic concepts
- [Configuration Guide](CONFIG.md) - Detailed configuration options

### Documentation Generation

The Shadow package includes powerful documentation generation tools. For detailed information about generating and customizing documentation, see our [Documentation Generation Guide](docs/documentation.md).

Key features:

- Automatic API documentation generation
- Interactive API explorer
- Custom template support
- Multiple output formats (Markdown, OpenAPI/Swagger)
- Example request/response generation

### Help System Integration

Shadow provides a comprehensive help system that can be accessed both through the CLI and API endpoints. For detailed information, see our [Help System Guide](docs/help.md).

Features include:

- Interactive CLI help commands
- API endpoint documentation
- Resource schema information
- Example generation
- Custom help content integration

For implementation examples and more advanced usage, please refer to our [Core Documentation](CORE.md).
