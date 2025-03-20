# Configuration Guide

## Table of Contents

- [Overview](#overview)
- [Basic Configuration](#basic-configuration)
- [Environment Variables](#environment-variables)
- [Advanced Configuration](#advanced-configuration)
- [Security Configuration](#security-configuration)
- [Performance Configuration](#performance-configuration)
- [Customization](#customization)

## Overview

Shadow provides extensive configuration options to customize its behavior. This guide covers all available configuration options and best practices for setting them up.

## Basic Configuration

After installing Shadow, publish the configuration file:

```bash
php artisan vendor:publish --provider="ThinkNeverland\\Shadow\\ShadowServiceProvider"
```

This will create `config/shadow.php` with default settings.

### Core Settings

```php
// config/shadow.php
return [
    'auto_generate_api' => true,
    'api_route_prefix' => env('SHADOW_API_PREFIX', 'shadow-api'),
    'api_auth_middleware' => ['api'],
    
    'response' => [
        'format' => 'json',
        'success_key' => 'success',
        'error_key' => 'error',
    ],
];
```

## Environment Variables

Recommended `.env` configuration:

```env
# Core Settings
SHADOW_DEBUG=false
SHADOW_API_PREFIX=shadow-api     # API route prefix, defaults to 'shadow-api'
SHADOW_DOCS_AUTH_ENABLED=true    # Enable authentication for API docs

# Cache Settings
SHADOW_CACHE_ENABLED=true
SHADOW_CACHE_TTL=3600
SHADOW_CACHE_PREFIX=shadow

# Security Settings
SHADOW_AUTH_ENABLED=true
SHADOW_RATE_LIMITING_ENABLED=true
```

## Advanced Configuration

### API Configuration

```php
'api' => [
    'auto_generate_api' => true,
    'api_route_prefix' => env('SHADOW_API_PREFIX', 'shadow-api'),
    'api_auth_middleware' => ['api'],
    'headers' => [
        'version' => 'X-Shadow-Version',
        'request-id' => 'X-Request-ID',
    ],
    'response' => [
        'format' => 'json',
        'include_debug' => env('SHADOW_DEBUG', false),
        'success_key' => 'success',
        'error_key' => 'error',
    ],
],
```

### Cache Configuration

```php
'cache' => [
    'enabled' => env('SHADOW_CACHE_ENABLED', true),
    'ttl' => env('SHADOW_CACHE_TTL', 3600),
    'prefix' => env('SHADOW_CACHE_PREFIX', 'shadow'),
    'store' => env('SHADOW_CACHE_STORE', 'redis'),
    'tags_enabled' => true,
],
```

### Query Configuration

```php
'query' => [
    'max_limit' => 100,
    'default_limit' => 15,
    'max_depth' => 3,
    'allowed_includes' => ['*'],
    'allowed_filters' => ['*'],
    'allowed_sorts' => ['*'],
    'eager_loading' => [
        'enabled' => true,
        'max_depth' => 2,
    ],
],
```

## Security Configuration

### Authentication

```php
'auth' => [
    'enabled' => env('SHADOW_AUTH_ENABLED', true),
    'guards' => ['api', 'sanctum'],
    'api_key' => [
        'enabled' => env('SHADOW_API_KEY_ENABLED', true),
        'header' => 'X-API-Key',
        'in_query' => false,
    ],
    'tokens' => [
        'lifetime' => 3600,
        'refresh_lifetime' => 1209600,
        'rotation' => true,
    ],
],
```

### Rate Limiting

```php
'rate_limiting' => [
    'enabled' => env('SHADOW_RATE_LIMITING_ENABLED', true),
    'default' => [
        'limit' => 60,
        'window' => 60,
    ],
    'auth' => [
        'limit' => 5,
        'window' => 300,
    ],
    'endpoints' => [
        'search' => [
            'limit' => 30,
            'window' => 60,
        ],
    ],
],
```

### Encryption

```php
'encryption' => [
    'key_rotation' => true,
    'algorithm' => 'AES-256-GCM',
    'fields' => [
        'users' => [
            'ssn' => true,
            'bank_account' => [
                'searchable' => true,
            ],
        ],
    ],
],
```

## Performance Configuration

### Query Optimization

```php
'performance' => [
    'query_cache' => [
        'enabled' => env('SHADOW_QUERY_CACHE_ENABLED', true),
        'ttl' => 60,
    ],
    'eager_loading' => [
        'enabled' => env('SHADOW_EAGER_LOADING_ENABLED', true),
        'default_relations' => [],
    ],
    'pagination' => [
        'max_limit' => 100,
        'default_limit' => 15,
    ],
    'optimization' => [
        'minimize_fields' => true,
        'optimize_includes' => true,
    ],
],
```

### Caching Strategies

```php
'caching' => [
    'strategies' => [
        'model' => [
            'enabled' => true,
            'ttl' => 3600,
        ],
        'query' => [
            'enabled' => true,
            'ttl' => 60,
        ],
        'relation' => [
            'enabled' => true,
            'ttl' => 1800,
        ],
    ],
],
```

## Customization

### Custom Middleware

```php
'middleware' => [
    'groups' => [
        'api' => [
            \App\Http\Middleware\CustomApiMiddleware::class,
        ],
    ],
    'aliases' => [
        'custom-auth' => \App\Http\Middleware\CustomAuthMiddleware::class,
    ],
],
```

### Custom Response Formats

```php
'responses' => [
    'formats' => [
        'custom' => [
            'handler' => \App\Formatters\CustomFormatter::class,
            'content_type' => 'application/x-custom',
        ],
    ],
],
```

### Event Listeners

```php
'events' => [
    'listeners' => [
        'shadow.model.created' => [
            \App\Listeners\ModelCreatedListener::class,
        ],
    ],
],
```

## Best Practices

1. **Environment-Specific Configuration**
   - Use `.env` files for environment-specific settings
   - Keep sensitive information in `.env`
   - Use reasonable defaults in config files

2. **Security**
   - Enable authentication in production
   - Configure rate limiting
   - Use HTTPS in production
   - Enable encryption for sensitive fields

3. **Performance**
   - Enable caching in production
   - Configure eager loading appropriately
   - Set reasonable pagination limits
   - Enable query optimization

4. **Customization**
   - Create custom middleware for specific needs
   - Use custom formatters for special response formats
   - Add event listeners for business logic

For more detailed examples and implementation guides, refer to our [Core Documentation](core.md).
