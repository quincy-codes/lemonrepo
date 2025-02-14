<template>
  <div class="max-w-4xl mx-auto">
    <UContainer class="max-w-8xl py-12 space-y-6">
      <h1 class="text-4xl sm:text-6xl lg:text-7xl tracking-tight text-gray-800 font-bold text-center font-display max-w-4xl mx-auto">
        Caching Guide
      </h1>

      <div class="w-full columns-2 py-4">
          <ButtonLink label="Documentation" size="sm" rounded="md" href="/docs" />
      </div>

      <div class="bg-gray-100 p-6 rounded-lg mb-8">
        <ul class="space-y-2">
          <li><a href="#cache-commands" class="text-blue-600 hover:underline">1. Cache Commands</a></li>
          <li><a href="#overview" class="text-blue-600 hover:underline">2. Overview</a></li>
          <li><a href="#configuration" class="text-blue-600 hover:underline">3. Configuration</a></li>
          <li><a href="#cache-service" class="text-blue-600 hover:underline">4. Cache Service</a></li>
          <li><a href="#cache-strategies" class="text-blue-600 hover:underline">5. Cache Strategies</a></li>
          <li><a href="#cache-management" class="text-blue-600 hover:underline">6. Cache Management</a></li>
          <li><a href="#best-practices" class="text-blue-600 hover:underline">7. Best Practices</a></li>
          <li><a href="#troubleshooting" class="text-blue-600 hover:underline">8. Troubleshooting</a></li>
        </ul>
      </div>

      <section id="cache-commands" class="mb-12">
        <h2 class="text-2xl font-semibold mb-4">1. Cache Commands</h2>
        
        <h3 class="text-xl font-semibold mb-4">Cache Clear</h3>
        <div class="bg-gray-800 rounded-lg p-4 mb-6">
          <pre class="text-green-400">
# Clear all Evolve cache
php artisan evolve:cache:clear --all

# Clear specific components
php artisan evolve:cache:clear --models    # Clear model registry cache
php artisan evolve:cache:clear --schema    # Clear schema cache
php artisan evolve:cache:clear --docs      # Clear API documentation cache
php artisan evolve:cache:clear --search    # Clear search results cache</pre>
        </div>

        <h3 class="text-xl font-semibold mb-4">Cache Warmup</h3>
        <div class="bg-gray-800 rounded-lg p-4 mb-6">
          <pre class="text-green-400">
# Warm up all cacheable content
php artisan evolve:cache:warmup --all

# Warm up specific components
php artisan evolve:cache:warmup --models    # Warm up model registry
php artisan evolve:cache:warmup --schema    # Warm up database schema
php artisan evolve:cache:warmup --docs      # Warm up API documentation
php artisan evolve:cache:warmup --search    # Warm up search indexes</pre>
        </div>

        <h3 class="text-xl font-semibold mb-4">Cache Metrics</h3>
        <div class="bg-gray-800 rounded-lg p-4 mb-6">
          <pre class="text-green-400">
# View basic metrics
php artisan evolve:cache:metrics

# View detailed metrics for specific tags
php artisan evolve:cache:metrics --tag=evolve-models --detailed</pre>
        </div>

        <p class="text-gray-700 mb-4">Metrics include:</p>
        <ul class="list-disc pl-6 space-y-2 text-gray-700">
          <li>Total keys and size</li>
          <li>Hit/miss rates</li>
          <li>Average TTL</li>
          <li>Per-tag statistics</li>
          <li>Top accessed keys</li>
        </ul>

        <h3 class="text-xl font-semibold mb-4">Cache Analysis</h3>
        <div class="bg-gray-800 rounded-lg p-4 mb-6">
          <pre class="text-green-400">
# Run basic analysis
php artisan evolve:cache:analyze

# Run analysis with custom thresholds
php artisan evolve:cache:analyze \
    --tag=evolve-models \
    --threshold=60 \
    --ttl-warning=86400 \
    --size-warning=5242880</pre>
        </div>

        <p class="text-gray-700 mb-4">Analysis includes:</p>
        <ul class="list-disc pl-6 space-y-2 text-gray-700">
          <li>Performance recommendations</li>
          <li>Size optimizations</li>
          <li>TTL warnings</li>
          <li>Hit rate analysis</li>
        </ul>
      </section>

      <section id="overview" class="mb-12">
        <h2 class="text-2xl font-semibold mb-4">2. Overview</h2>
        <p class="text-gray-700 mb-4">
          Evolve provides a powerful caching system that helps optimize performance through multiple caching strategies and intelligent cache management.
        </p>
      </section>

      <section id="configuration" class="mb-12">
        <h2 class="text-2xl font-semibold mb-4">3. Configuration</h2>
        
        <h3 class="text-xl font-semibold mb-4">Basic Setup</h3>
        <div class="bg-gray-800 rounded-lg p-4 mb-6">
          <pre class="text-green-400">
// config/evolve.php
return [
    'cache' => [
        'enabled' => env('EVOLVE_CACHE_ENABLED', true),
        'ttl' => env('EVOLVE_CACHE_TTL', 3600),
        'prefix' => env('EVOLVE_CACHE_PREFIX', 'evolve'),
        'store' => env('EVOLVE_CACHE_STORE', 'redis'),
        'tags_enabled' => true,
    ],
];</pre>
        </div>

        <h3 class="text-xl font-semibold mb-4">Environment Variables</h3>
        <div class="bg-gray-800 rounded-lg p-4 mb-6">
          <pre class="text-green-400">
EVOLVE_CACHE_ENABLED=true
EVOLVE_CACHE_TTL=3600
EVOLVE_CACHE_PREFIX=evolve
EVOLVE_CACHE_STORE=redis</pre>
        </div>
      </section>

      <section id="cache-service" class="mb-12">
        <h2 class="text-2xl font-semibold mb-4">4. Cache Service</h2>
        
        <h3 class="text-xl font-semibold mb-4">Cache Tags</h3>
        <div class="bg-gray-800 rounded-lg p-4 mb-6">
          <pre class="text-green-400">
// Use cache tags
$users = $evolve->model(User::class)
    ->withCacheTags(['users', 'api'])
    ->cache()
    ->get();

// Flush tagged cache
Cache::tags(['users'])->flush();</pre>
        </div>

        <h3 class="text-xl font-semibold mb-4">Cache Invalidation</h3>
        <div class="bg-gray-800 rounded-lg p-4 mb-6">
          <pre class="text-green-400">
// Automatic cache invalidation
protected $evolveConfig = [
    'cache' => [
        'auto_invalidate' => true,
        'invalidate_on' => ['update', 'delete'],
    ],
];

// Manual cache invalidation
$user->invalidateCache();</pre>
        </div>
      </section>

      <section id="cache-strategies" class="mb-12">
        <h2 class="text-2xl font-semibold mb-4">5. Cache Strategies</h2>
        
        <h3 class="text-xl font-semibold mb-4">Model Caching</h3>
        <div class="bg-gray-800 rounded-lg p-4 mb-6">
          <pre class="text-green-400">
use ThinkNeverland\Evolve\Traits\HasEvolve;

class User extends Model
{
    use HasEvolve;

    protected $evolveConfig = [
        'cache' => [
            'enabled' => true,
            'ttl' => 3600,
            'tags' => ['users'],
        ],
    ];
}</pre>
        </div>

        <h3 class="text-xl font-semibold mb-4">Query Caching</h3>
        <div class="bg-gray-800 rounded-lg p-4 mb-6">
          <pre class="text-green-400">
// Enable query cache
$users = $evolve->model(User::class)
    ->cache()
    ->get();

// Custom TTL
$users = $evolve->model(User::class)
    ->cache(ttl: 1800)
    ->get();</pre>
        </div>
      </section>

      <section id="cache-management" class="mb-12">
        <h2 class="text-2xl font-semibold mb-4">6. Cache Management</h2>
        
        <h3 class="text-xl font-semibold mb-4">Cache Tags</h3>
        <div class="bg-gray-800 rounded-lg p-4 mb-6">
          <pre class="text-green-400">
// Use cache tags
$users = $evolve->model(User::class)
    ->withCacheTags(['users', 'api'])
    ->cache()
    ->get();

// Flush tagged cache
Cache::tags(['users'])->flush();</pre>
        </div>

        <h3 class="text-xl font-semibold mb-4">Cache Invalidation</h3>
        <div class="bg-gray-800 rounded-lg p-4 mb-6">
          <pre class="text-green-400">
// Automatic cache invalidation
protected $evolveConfig = [
    'cache' => [
        'auto_invalidate' => true,
        'invalidate_on' => ['update', 'delete'],
    ],
];

// Manual cache invalidation
$user->invalidateCache();</pre>
        </div>
      </section>

      <section id="best-practices" class="mb-12">
        <h2 class="text-2xl font-semibold mb-4">7. Best Practices</h2>
        
        <h3 class="text-xl font-semibold mb-4">Cache Strategy Selection</h3>
        <ul class="list-disc pl-6 space-y-2 text-gray-700">
          <li><strong>Model Caching:</strong> For frequently accessed individual models</li>
          <li><strong>Query Caching:</strong> For complex or expensive queries</li>
          <li><strong>Response Caching:</strong> For API responses</li>
          <li><strong>Relation Caching:</strong> For related data that changes infrequently</li>
        </ul>

        <h3 class="text-xl font-semibold mb-4">Performance Optimization</h3>
        <div class="bg-gray-800 rounded-lg p-4 mb-6">
          <pre class="text-green-400">
// Batch cache operations
$evolve->cache()->batch(function($cache) {
    $cache->put('key1', 'value1');
    $cache->put('key2', 'value2');
});</pre>
        </div>
      </section>

      <section id="troubleshooting" class="mb-12">
        <h2 class="text-2xl font-semibold mb-4">8. Troubleshooting</h2>
        
        <h3 class="text-xl font-semibold mb-4">Common Issues</h3>
        <div class="bg-gray-800 rounded-lg p-4 mb-6">
          <pre class="text-green-400">
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
   - Consider cache warming strategies</pre>
        </div>

        <h3 class="text-xl font-semibold mb-4">Debugging</h3>
        <div class="bg-gray-800 rounded-lg p-4 mb-6">
          <pre class="text-green-400">
// Enable cache debugging
Config::set('evolve.cache.debug', true);

// Monitor cache operations
Log::debug('Cache operation', [
    'key' => $key,
    'tags' => $tags,
    'duration' => $duration
]);</pre>
        </div>
      </section>
    </UContainer>
  </div>
</template>

<script setup lang="ts">
// Component logic can be added here if needed
</script> 