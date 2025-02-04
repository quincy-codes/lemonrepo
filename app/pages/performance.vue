<template>
  <div class="max-w-4xl mx-auto">
    <UContainer class="max-w-8xl py-12 space-y-6">
      <h1 class="text-4xl sm:text-6xl lg:text-7xl tracking-tight text-gray-800 font-bold text-center font-display max-w-4xl mx-auto">
        Performance Guide
      </h1>

      <div class="w-full columns-2 py-4">
          <ButtonLink label="Documentation" size="sm" rounded="md" href="/docs" />
      </div>

      <div class="bg-gray-100 p-6 rounded-lg mb-8">
        <ul class="space-y-2">
          <li><a href="#overview" class="text-blue-600 hover:underline">1. Overview</a></li>
          <li><a href="#caching" class="text-blue-600 hover:underline">2. Caching Strategies</a></li>
          <li><a href="#query-optimization" class="text-blue-600 hover:underline">3. Query Optimization</a></li>
          <li><a href="#resource-management" class="text-blue-600 hover:underline">4. Resource Management</a></li>
          <li><a href="#monitoring" class="text-blue-600 hover:underline">5. Performance Monitoring</a></li>
          <li><a href="#best-practices" class="text-blue-600 hover:underline">6. Best Practices</a></li>
        </ul>
      </div>

      <section id="overview" class="mb-12">
        <h2 class="text-2xl font-semibold mb-4">1. Overview</h2>
        <p class="text-gray-700 mb-4">
          Evolve provides comprehensive performance optimization features to ensure your API remains fast and efficient under high load. This guide covers key strategies and best practices for optimizing your application's performance.
        </p>
      </section>

      <section id="caching" class="mb-12">
        <h2 class="text-2xl font-semibold mb-4">2. Caching Strategies</h2>
        
        <h3 class="text-xl font-semibold mb-4">Response Caching</h3>
        <div class="bg-gray-800 rounded-lg p-4 mb-6">
          <pre class="text-green-400">
// Enable response caching
protected $evolveConfig = [
    'cache' => [
        'ttl' => 3600,
        'tags' => ['api', 'users'],
        'conditions' => [
            'method' => ['GET'],
            'status' => [200],
        ],
    ],
];</pre>
        </div>

        <h3 class="text-xl font-semibold mb-4">Query Result Caching</h3>
        <div class="bg-gray-800 rounded-lg p-4 mb-6">
          <pre class="text-green-400">
// Cache query results
$users = User::cache(ttl: 3600)
    ->whereActive()
    ->withCount('posts')
    ->get();

// Cache with tags
$posts = Post::cacheTag(['posts', 'recent'])
    ->latest()
    ->take(10)
    ->get();</pre>
        </div>
      </section>

      <section id="query-optimization" class="mb-12">
        <h2 class="text-2xl font-semibold mb-4">3. Query Optimization</h2>
        
        <h3 class="text-xl font-semibold mb-4">Eager Loading</h3>
        <div class="bg-gray-800 rounded-lg p-4 mb-6">
          <pre class="text-green-400">
// Configure default eager loads
protected $evolveConfig = [
    'with' => ['profile', 'settings'],
    'withCount' => ['posts'],
    'withExists' => ['subscription'],
];

// Conditional eager loading
$users = User::with(['posts' => function($query) {
    $query->recent()->select('id', 'user_id', 'title');
}])->get();</pre>
        </div>

        <h3 class="text-xl font-semibold mb-4">Index Optimization</h3>
        <div class="bg-gray-800 rounded-lg p-4 mb-6">
          <pre class="text-green-400">
// Add composite indexes for common queries
Schema::table('posts', function (Blueprint $table) {
    $table->index(['user_id', 'status', 'created_at']);
    $table->fullText('content');
});</pre>
        </div>
      </section>

      <section id="resource-management" class="mb-12">
        <h2 class="text-2xl font-semibold mb-4">4. Resource Management</h2>
        
        <h3 class="text-xl font-semibold mb-4">Memory Optimization</h3>
        <div class="bg-gray-800 rounded-lg p-4 mb-6">
          <pre class="text-green-400">
// Use standard pagination with configurable page size
Route::get('/users', function () {
    return User::paginate(config('evolve.pagination.per_page', 15));
});

// Chunk processing for large operations
User::chunk(100, function ($users) {
    foreach ($users as $user) {
        ProcessUser::dispatch($user);
    }
});</pre>
        </div>

        <h3 class="text-xl font-semibold mb-4">Resource Limits</h3>
        <div class="bg-gray-800 rounded-lg p-4 mb-6">
          <pre class="text-green-400">
// Configure resource limits
'limits' => [
    'max_includes' => 3,
    'max_fields' => 20,
    'max_page_size' => 100,
    'max_nested_level' => 2,
],

// Rate limiting
'rate_limiting' => [
    'enabled' => true,
    'max_requests' => 60,
    'decay_minutes' => 1,
]</pre>
        </div>
      </section>

      <section id="monitoring" class="mb-12">
        <h2 class="text-2xl font-semibold mb-4">5. Performance Monitoring</h2>
        
        <h3 class="text-xl font-semibold mb-4">Query Monitoring</h3>
        <div class="bg-gray-800 rounded-lg p-4 mb-6">
          <pre class="text-green-400">
// Enable query logging
DB::enableQueryLog();

// Monitor slow queries
'monitoring' => [
    'slow_query_threshold' => 1000, // milliseconds
    'log_slow_queries' => true,
    'alert_on_slow_queries' => true,
];</pre>
        </div>

        <h3 class="text-xl font-semibold mb-4">Performance Metrics</h3>
        <div class="bg-gray-800 rounded-lg p-4 mb-6">
          <pre class="text-green-400">
// Configure performance metrics
'metrics' => [
    'enabled' => true,
    'collectors' => [
        'request_duration',
        'memory_usage',
        'query_count',
        'cache_hits',
    ],
    'storage' => 'redis',
];</pre>
        </div>
      </section>

      <section id="best-practices" class="mb-12">
        <h2 class="text-2xl font-semibold mb-4">6. Best Practices</h2>
        
        <h3 class="text-xl font-semibold mb-4">General Guidelines</h3>
        <ul class="list-disc pl-6 space-y-2 text-gray-700">
          <li>Use appropriate caching strategies for your use case</li>
          <li>Implement eager loading to prevent N+1 queries</li>
          <li>Optimize database indexes for common queries</li>
          <li>Configure pagination settings in config/evolve.php</li>
          <li>Monitor and log performance metrics</li>
          <li>Set appropriate resource limits</li>
          <li>Use queue workers for heavy operations</li>
        </ul>

        <h3 class="text-xl font-semibold mb-4">Configuration Example</h3>
        <div class="bg-gray-800 rounded-lg p-4 mb-6">
          <pre class="text-green-400">
// Optimal performance configuration
'performance' => [
    'cache' => [
        'enabled' => true,
        'ttl' => 3600,
        'tags_enabled' => true,
    ],
    'query' => [
        'chunk_size' => 100,
        'slow_threshold' => 1000,
    ],
    'resources' => [
        'pagination' => [
            'default_size' => 15,
            'max_size' => 100,
        ],
    ],
    'monitoring' => [
        'enabled' => true,
        'alert_threshold' => 5000,
    ],
];</pre>
        </div>
      </section>
    </UContainer>
  </div>
</template>

<script setup lang="ts">
// Component logic can be added here if needed
</script> 