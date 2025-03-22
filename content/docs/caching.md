# Caching Guide

## Table of Contents

1. [Cache Commands](#cache-commands)
2. [Overview](#overview)
3. [Configuration](#configuration)
4. [Cache Service](#cache-service)
5. [Cache Strategies](#cache-strategies)
6. [Cache Management](#cache-management)
7. [Best Practices](#best-practices)
8. [Troubleshooting](#troubleshooting)

## 1. Cache Commands

### Cache Clear

```bash
# Clear all Shadow cache
php artisan shadow:cache:clear --all

# Clear specific components
php artisan shadow:cache:clear --models    # Clear model registry cache
php artisan shadow:cache:clear --schema    # Clear schema cache
php artisan shadow:cache:clear --docs      # Clear API documentation cache
php artisan shadow:cache:clear --search    # Clear search results cache
```

### Cache Warmup

```bash
# Warm up all cacheable content
php artisan shadow:cache:warmup --all

# Warm up specific components
php artisan shadow:cache:warmup --models    # Warm up model registry
php artisan shadow:cache:warmup --schema    # Warm up database schema
php artisan shadow:cache:warmup --docs      # Warm up API documentation
php artisan shadow:cache:warmup --search    # Warm up search indexes
```

### Cache Metrics

```bash
# View basic metrics
php artisan shadow:cache:metrics

# View detailed metrics for specific tags
php artisan shadow:cache:metrics --tag=shadow-models --detailed
```

Metrics include:

- Total keys and size
- Hit/miss rates
- Average TTL
- Per-tag statistics
- Top accessed keys

### Cache Analysis

```bash
# Run basic analysis
php artisan shadow:cache:analyze

# Run analysis with custom thresholds
php artisan shadow:cache:analyze \
    --tag=shadow-models \
    --threshold=60 \
    --ttl-warning=86400 \
    --size-warning=5242880
```

Analysis includes:

- Performance recommendations
- Size optimizations
- TTL warnings
- Hit rate analysis

## 2. Overview

Shadow provides a powerful caching system that helps optimize performance through multiple caching strategies and intelligent cache management.

## 3. Configuration

### Basic Setup

```php
// config/shadow.php
return [
    'cache' => [
        'enabled' => env('SHADOW_CACHE_ENABLED', true),
        'ttl' => env('SHADOW_CACHE_TTL', 3600),
        'prefix' => env('SHADOW_CACHE_PREFIX', 'shadow'),
        'store' => env('SHADOW_CACHE_STORE', 'redis'),
        'tags_enabled' => true,
    ],
];
```

### Environment Variables

```env
SHADOW_CACHE_ENABLED=true
SHADOW_CACHE_TTL=3600
SHADOW_CACHE_PREFIX=shadow
SHADOW_CACHE_STORE=redis
```

## 4. Cache Service

### Cache Tags

```php
// Use cache tags
$users = $shadow->model(User::class)
    ->withCacheTags(['users', 'api'])
    ->cache()
    ->get();

// Flush tagged cache
Cache::tags(['users'])->flush();
```

### Cache Invalidation

```php
// Automatic cache invalidation
protected $shadowConfig = [
    'cache' => [
        'auto_invalidate' => true,
        'invalidate_on' => ['update', 'delete'],
    ],
];

// Manual cache invalidation
$user->invalidateCache();
```

## 5. Cache Strategies

### Model Caching

```php
use ThinkNeverland\Shadow\Traits\HasShadow;

class User extends Model
{
    use HasShadow;

    protected $shadowConfig = [
        'cache' => [
            'enabled' => true,
            'ttl' => 3600,
            'tags' => ['users'],
        ],
    ];
}
```

### Query Caching

```php
// Enable query cache
$users = $shadow->model(User::class)
    ->cache()
    ->get();

// Custom TTL
$users = $shadow->model(User::class)
    ->cache(ttl: 1800)
    ->get();
```

## 6. Cache Management

### Cache Tags

```php
// Use cache tags
$users = $shadow->model(User::class)
    ->withCacheTags(['users', 'api'])
    ->cache()
    ->get();

// Flush tagged cache
Cache::tags(['users'])->flush();
```

### Cache Invalidation

```php
// Automatic cache invalidation
protected $shadowConfig = [
    'cache' => [
        'auto_invalidate' => true,
        'invalidate_on' => ['update', 'delete'],
    ],
];

// Manual cache invalidation
$user->invalidateCache();
```

## 7. Best Practices

### Cache Strategy Selection

- **Model Caching:** For frequently accessed individual models
- **Query Caching:** For complex or expensive queries
- **Response Caching:** For API responses
- **Relation Caching:** For related data that changes infrequently

### Performance Optimization

```php
// Batch cache operations
$shadow->cache()->batch(function($cache) {
    $cache->put('key1', 'value1');
    $cache->put('key2', 'value2');
});
```

## 8. Troubleshooting

### Common Issues

```
1. Tags Not Working
   - Ensure your cache driver supports tags (Redis, Memcached)
   - File and database drivers don't support tags
   - The service will automatically fall back to non-tagged caching

2. Cache Not Being Cleared
   - Verify cache driver configuration
   - Check cache key naming
   - Ensure proper tag usage

3. Performance Issues
   - Monitor cache hit/miss ratios
   - Adjust cache duration
   - Consider cache warming strategies
```

### Debugging

```php
// Enable cache debugging
Config::set('shadow.cache.debug', true);

// Monitor cache operations
Log::debug('Cache operation', [
    'key' => $key,
    'tags' => $tags,
    'duration' => $duration
]);
```
