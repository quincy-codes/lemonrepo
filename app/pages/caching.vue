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
          <li><a href="#overview" class="text-blue-600 hover:underline">1. Overview</a></li>
          <li><a href="#configuration" class="text-blue-600 hover:underline">2. Configuration</a></li>
          <li><a href="#model-caching" class="text-blue-600 hover:underline">3. Model Caching</a></li>
          <li><a href="#query-caching" class="text-blue-600 hover:underline">4. Query Caching</a></li>
          <li><a href="#cache-invalidation" class="text-blue-600 hover:underline">5. Cache Invalidation</a></li>
          <li><a href="#best-practices" class="text-blue-600 hover:underline">6. Best Practices</a></li>
        </ul>
      </div>

      <section id="overview" class="mb-12">
        <h2 class="text-2xl font-semibold mb-4">1. Overview</h2>
        <p class="text-xl text-gray-600 text-center max-w-3xl mx-auto">
          Shadow provides comprehensive caching capabilities to improve performance and reduce database load. This guide covers all aspects of caching in Shadow.
        </p>
      </section>

      <section id="configuration" class="mb-12">
        <h2 class="text-2xl font-semibold mb-4">2. Configuration</h2>
        
        <h3 class="text-xl font-semibold mb-2">Global Cache Settings</h3>
        <p class="mb-4">Configure global cache settings in your config file:</p>
        <div class="bg-slate-800 rounded-lg p-4 mb-6">
          <pre class="text-green-400">
// config/shadow.php
return [
    'cache' => [
        'enabled' => env('SHADOW_CACHE_ENABLED', true),
        'ttl' => env('SHADOW_CACHE_TTL', 3600),
        'prefix' => env('SHADOW_CACHE_PREFIX', 'shadow'),
        'store' => env('SHADOW_CACHE_STORE', 'redis'),
        'tags_enabled' => true,
    ],
];</pre>
        </div>

        <h3 class="text-xl font-semibold mb-4">Environment Variables</h3>
        <div class="bg-gray-800 rounded-lg p-4 mb-6">
          <pre class="text-green-400">
SHADOW_CACHE_ENABLED=true
SHADOW_CACHE_TTL=3600
SHADOW_CACHE_PREFIX=shadow
SHADOW_CACHE_STORE=redis</pre>
        </div>
      </section>

      <section id="model-caching" class="mb-12">
        <h2 class="text-2xl font-semibold mb-4">3. Model Caching</h2>
        
        <h3 class="text-xl font-semibold mb-2">Model-Level Cache Configuration</h3>
        <p class="mb-4">Configure caching at the model level:</p>
        <div class="bg-slate-800 rounded-lg p-4 mb-6">
          <pre class="text-green-400">
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
}</pre>
        </div>
      </section>

      <section id="query-caching" class="mb-12">
        <h2 class="text-2xl font-semibold mb-4">4. Query Caching</h2>
        
        <h3 class="text-xl font-semibold mb-2">Query-Level Caching</h3>
        <p class="mb-4">Cache query results for better performance:</p>
        <div class="bg-slate-800 rounded-lg p-4 mb-6">
          <pre class="text-green-400">
// Enable query cache
$users = $shadow->model(User::class)
    ->cache()
    ->get();

// Custom TTL
$users = $shadow->model(User::class)
    ->cache(ttl: 1800)
    ->get();</pre>
        </div>
      </section>

      <section id="cache-invalidation" class="mb-12">
        <h2 class="text-2xl font-semibold mb-4">5. Cache Invalidation</h2>
        
        <h3 class="text-xl font-semibold mb-2">Automatic Invalidation</h3>
        <p class="mb-4">Shadow automatically invalidates cache when data changes:</p>
        <div class="bg-slate-800 rounded-lg p-4 mb-6">
          <pre class="text-green-400">
// Automatic cache invalidation
protected $shadowConfig = [
    'cache' => [
        'auto_invalidate' => true,
        'invalidate_on' => ['update', 'delete'],
    ],
];</pre>
        </div>

        <h3 class="text-xl font-semibold mb-2">Manual Invalidation</h3>
        <p class="mb-4">Manually invalidate cache when needed:</p>
        <div class="bg-slate-800 rounded-lg p-4 mb-6">
          <pre class="text-green-400">
// Manual cache invalidation
$user->invalidateCache();</pre>
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
$shadow->cache()->batch(function($cache) {
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