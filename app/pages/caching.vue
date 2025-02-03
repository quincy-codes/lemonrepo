<template>
  <div class="w-full">
    <UContainer class="py-12 space-y-6">
      <h1 class="text-4xl sm:text-6xl lg:text-7xl tracking-tight text-gray-800 font-bold text-center font-display mx-auto">
        Caching Guide
      </h1>

      <div class="w-full columns-2 py-4">
          <ButtonLink label="Documentation Home" size="md" rounded="md" href="/docs" />
      </div>

      <div class="bg-gray-100 p-6 rounded-lg mb-8">
        <ul class="space-y-2">
          <li><a href="#overview" class="text-blue-600 hover:underline">1. Overview</a></li>
          <li><a href="#configuration" class="text-blue-600 hover:underline">2. Configuration</a></li>
          <li><a href="#cache-strategies" class="text-blue-600 hover:underline">3. Cache Strategies</a></li>
          <li><a href="#cache-management" class="text-blue-600 hover:underline">4. Cache Management</a></li>
          <li><a href="#advanced-features" class="text-blue-600 hover:underline">5. Advanced Features</a></li>
          <li><a href="#best-practices" class="text-blue-600 hover:underline">6. Best Practices</a></li>
        </ul>
      </div>

      <section id="overview" class="mb-12">
        <h2 class="text-2xl font-semibold mb-4">1. Overview</h2>
        <p class="text-gray-700 mb-4">
          Evolve provides a powerful caching system that helps optimize performance through multiple caching strategies and intelligent cache management.
        </p>
      </section>

      <section id="configuration" class="mb-12">
        <h2 class="text-2xl font-semibold mb-4">2. Configuration</h2>
        
        <h3 class="text-xl font-semibold mb-4">Basic Setup</h3>
        <div class="bg-gray-800 rounded-lg p-4 mb-6">
          <pre class="text-green-400">
&lt;?php

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

      <section id="cache-strategies" class="mb-12">
        <h2 class="text-2xl font-semibold mb-4">3. Cache Strategies</h2>
        
        <h3 class="text-xl font-semibold mb-4">Model Caching</h3>
        <div class="bg-gray-800 rounded-lg p-4 mb-6">
          <pre class="text-green-400">
&lt;?php

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
        <h2 class="text-2xl font-semibold mb-4">4. Cache Management</h2>
        
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

      <section id="advanced-features" class="mb-12">
        <h2 class="text-2xl font-semibold mb-4">5. Advanced Features</h2>
        
        <h3 class="text-xl font-semibold mb-4">Cache Warmup</h3>
        <div class="bg-gray-800 rounded-lg p-4 mb-6">
          <pre class="text-green-400">
// Cache warmup command
php artisan evolve:cache:warmup

// Custom warmup logic
class UserCacheWarmer implements CacheWarmer
{
    public function warm()
    {
        User::chunk(100, function($users) {
            foreach ($users as $user) {
                $user->warmCache();
            }
        });
    }
}</pre>
        </div>

        <h3 class="text-xl font-semibold mb-4">Cache Monitoring</h3>
        <div class="bg-gray-800 rounded-lg p-4 mb-6">
          <pre class="text-green-400">
// Monitor cache hits/misses
'monitoring' => [
    'enabled' => true,
    'driver' => 'redis',
    'metrics' => ['hits', 'misses', 'keys'],
],

// Get cache statistics
$stats = $evolve->cache()->getStats();</pre>
        </div>
      </section>

      <section id="best-practices" class="mb-12">
        <h2 class="text-2xl font-semibold mb-4">6. Best Practices</h2>
        
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
    </UContainer>
  </div>
</template>

<script setup lang="ts">
// Component logic can be added here if needed
</script> 