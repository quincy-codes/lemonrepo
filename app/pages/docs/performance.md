# Performance Optimization Guide

## Table of Contents

- [Overview](#overview)
- [Query Optimization](#query-optimization)
- [Caching Strategies](#caching-strategies)
- [Resource Management](#resource-management)
- [Monitoring and Profiling](#monitoring-and-profiling)
- [Best Practices](#best-practices)

## Overview

This guide covers performance optimization strategies for Shadow, helping you build efficient and scalable applications.

## Query Optimization

### Eager Loading

```php
// Configure default eager loading
protected $shadowConfig = [
    'with' => ['profile', 'roles'],
    'withCount' => ['posts'],
];

// Dynamic eager loading
$users = $shadow->model(User::class)
    ->with(['posts' => function($query) {
        $query->latest();
    }])
    ->get();
```

### Query Caching

```php
// Enable query cache
'query_cache' => [
    'enabled' => true,
    'ttl' => 3600, // 1 hour
],

// Use query cache
$users = $shadow->model(User::class)
    ->cache()
    ->get();
```

### Index Optimization

- Use appropriate database indexes
- Configure composite indexes for common queries
- Monitor query performance with `EXPLAIN`

## Caching Strategies

### Model Caching

```php
// Enable model cache
protected $shadowConfig = [
    'cache' => [
        'enabled' => true,
        'ttl' => 3600,
    ],
];
```

### Relation Caching

```php
// Cache related data
$user->cacheRelation('posts', 3600);
```

### Cache Tags

```php
// Use cache tags for better organization
$shadow->model(User::class)
    ->withCacheTags(['users', 'api'])
    ->get();
```

## Resource Management

### Pagination

```php
// Configure default pagination
'pagination' => [
    'default_limit' => 15,
    'max_limit' => 100,
],

// Use cursor pagination for large datasets
$users = $shadow->model(User::class)
    ->cursorPaginate();
```

### Field Selection

```php
// Select only needed fields
$users = $shadow->model(User::class)
    ->select(['id', 'name', 'email'])
    ->get();
```

### Batch Processing

```php
// Process large datasets in chunks
$shadow->model(User::class)
    ->chunk(100, function($users) {
        // Process chunk
    });
```

## Monitoring and Profiling

### Query Logging

```php
// Enable query logging
'debug' => [
    'query_log' => true,
    'slow_query_threshold' => 1000, // ms
],
```

### Performance Metrics

```php
// Monitor performance metrics
$shadow->model(User::class)
    ->withMetrics()
    ->get();
```

### Debug Toolbar Integration

```php
// Enable debug toolbar
'debug_toolbar' => [
    'enabled' => true,
    'collectors' => ['queries', 'cache'],
],
```

## Best Practices

### 1. Query Optimization

- Use appropriate indexes
- Implement eager loading
- Avoid N+1 queries
- Use query caching for frequently accessed data

### 2. Caching Strategy

- Implement multi-layer caching
- Use appropriate cache drivers
- Set reasonable TTL values
- Clear cache strategically

### 3. Resource Usage

- Implement pagination
- Use field selection
- Process large datasets in chunks
- Monitor memory usage

### 4. API Optimization

- Compress responses
- Use ETags
- Implement rate limiting
- Cache API responses

### 5. Database Optimization

- Optimize table structure
- Use appropriate data types
- Regular maintenance
- Monitor query performance

## Configuration Example

```php
// config/shadow.php
return [
    'performance' => [
        'query_cache' => [
            'enabled' => true,
            'ttl' => 3600,
        ],
        'eager_loading' => [
            'enabled' => true,
            'default_relations' => [],
        ],
        'pagination' => [
            'default_limit' => 15,
            'max_limit' => 100,
        ],
        'optimization' => [
            'minimize_fields' => true,
            'optimize_includes' => true,
        ],
    ],
];
```

## Monitoring Tools

1. Query Monitor

```php
php artisan shadow:monitor:queries
```

2. Cache Analysis

```php
php artisan shadow:analyze:cache
```

3. Performance Report

```php
php artisan shadow:report:performance
```

## Related Documentation

- [Configuration Guide](config.md)
- [Advanced Usage](advanced-usage.md)
- [API Documentation](api.md)
