<template>
  <div class="max-w-4xl mx-auto">
    <UContainer class="max-w-8xl py-12 space-y-6">
      <h1 class="text-4xl sm:text-6xl lg:text-7xl tracking-tight text-gray-800 font-bold text-center font-display max-w-4xl mx-auto">
        Evolve Documentation
      </h1>

      <div class="w-full columns-2 py-4">
        <ButtonLink href="/evolve-core-documentation" />
        <ButtonLink href="/evolve-api-documentation" />
        <ButtonLink href="/evolve-dashboard-documentation" />
      </div>

      <h3 class="max-w-xl text-2xl sm:text-2xl font-semibold tracking-tight text-gray-900 font-display mb-4 mt-8">
        EvolveCore (Foundation / Prerelease)
      </h3>

      <h3 class="max-w-xl text-2xl sm:text-2xl font-semibold tracking-tight text-gray-900 font-display mb-4 mt-8">
        Table of Contents
      </h3>

      <div class="bg-gray-100 p-6 rounded-lg mb-8">
        <ul class="space-y-2">
          <li><a href="#installation" class="text-blue-600 hover:underline">1. Installation</a></li>
          <li><a href="#overview" class="text-blue-600 hover:underline">2. Overview</a></li>
          <li>
            <a href="#getting-started" class="text-blue-600 hover:underline">3. Getting Started</a>
            <ul class="ml-4 mt-2">
              <li><a href="#model-setup" class="text-blue-600 hover:underline">3.1 Model Setup</a></li>
              <li><a href="#basic-usage" class="text-blue-600 hover:underline">3.2 Basic Usage</a></li>
            </ul>
          </li>
          <li>
            <a href="#features" class="text-blue-600 hover:underline">4. Features</a>
            <ul class="ml-4 mt-2">
              <li><a href="#validation" class="text-blue-600 hover:underline">4.1 Validation</a></li>
              <li><a href="#filtering" class="text-blue-600 hover:underline">4.2 Filtering</a></li>
              <li><a href="#sorting" class="text-blue-600 hover:underline">4.3 Sorting</a></li>
              <li><a href="#events" class="text-blue-600 hover:underline">4.4 Events</a></li>
              <li><a href="#policies" class="text-blue-600 hover:underline">4.5 Policies</a></li>
              <li><a href="#caching" class="text-blue-600 hover:underline">4.6 Caching</a></li>
              <li><a href="#optimizations" class="text-blue-600 hover:underline">4.7 Optimizations</a></li>
              <li><a href="#metrics" class="text-blue-600 hover:underline">4.8 Metrics</a></li>
            </ul>
          </li>
          <li>
            <a href="#advanced" class="text-blue-600 hover:underline">5. Advanced Usage</a>
            <ul class="ml-4 mt-2">
              <li><a href="#extensions" class="text-blue-600 hover:underline">5.1 Extensions</a></li>
              <li><a href="#commands" class="text-blue-600 hover:underline">5.2 Commands</a></li>
            </ul>
          </li>
        </ul>
      </div>

      <h2 id="installation" class="text-center mx-auto max-w-xl text-2xl sm:text-2xl font-semibold tracking-tight text-gray-900 font-display mb-4 mt-24">
        1. Installation
      </h2>

      <p class="mb-4">Install EvolveCore using Composer:</p>

      <pre class="bg-gray-800 rounded-lg p-4 overflow-x-auto mb-6">
<code class="text-green-400">composer require thinkneverland/evolve-core</code>
</pre>

      <p class="mb-4">Required:</p>
      <ul class="list-disc ml-6 mb-4">
        <li>PHP ^7.4|^8.0</li>
        <li>Laravel ^8.0|^9.0</li>
        <li>spatie/laravel-permission ^5.5</li>
        <li>doctrine/dbal ^3.0</li>
      </ul>

      <p class="mb-4">Service provider is auto-registered. To register manually, add to config/app.php:</p>

      <pre class="bg-gray-800 rounded-lg p-4 overflow-x-auto mb-6">
<code class="text-green-400">'providers' => [
    // ...
    Thinkneverland\Evolve\Core\EvolveCoreServiceProvider::class,
];</code>
</pre>

      <h2 id="overview" class="text-center mx-auto max-w-xl text-2xl sm:text-2xl font-semibold tracking-tight text-gray-900 font-display mb-4 mt-24">
        2. Overview
      </h2>

      <p class="mb-4">Evolve Core provides powerful model CRUD, optimization, validation, querying and more out of the box. Enables rapid, high-quality app development.</p>

      <p class="mb-4">Key features:</p>
      <ul class="list-disc ml-6 mb-4">
        <li>Auto-discovery of Evolvable models</li>
        <li>Automatic validation based on schema</li>
        <li>Filterable/sortable traits for easy querying</li>
        <li>Advanced query optimization</li>
        <li>Comprehensive event system</li>
        <li>Flexible policies for authorization</li>
        <li>Caching and metrics built-in</li>
      </ul>

      <h2 id="getting-started" class="text-center mx-auto max-w-xl text-2xl sm:text-2xl font-semibold tracking-tight text-gray-900 font-display mb-4 mt-24">
        3. Getting Started
      </h2>

      <h3 id="model-setup" class="text-xl mt-8 mb-4 font-semibold">3.1 Model Setup</h3>

      <p class="mb-4">Make model Evolvable with the trait and interface:</p>

      <pre class="bg-gray-800 rounded-lg p-4 overflow-x-auto mb-6">
<code class="text-green-400">use Thinkneverland\Evolve\Core\Traits\Evolvable;
use Thinkneverland\Evolve\Core\Contracts\EvolveModelInterface;

class YourModel extends Model implements EvolveModelInterface
{
  use Evolvable;

  // Model code...
}</code>
</pre>

      <p class="mb-4">Auto-discovery happens at app boot. To disable, set <code class="bg-gray-200 p-1 rounded">evolve.models.auto_discover</code> to <code class="bg-gray-200 p-1 rounded">false</code> and register manually:</p>

      <pre class="bg-gray-800 rounded-lg p-4 overflow-x-auto mb-6">
<code class="text-green-400">use Thinkneverland\Evolve\Core\Support\ModelRegistry;
// ...
ModelRegistry::register(YourModel::class);</code>
</pre>

      <h3 id="basic-usage" class="text-xl mt-8 mb-4 font-semibold">3.2 Basic Usage</h3>

      <p class="mb-4">Get paginated, filtered, sorted results:</p>

      <pre class="bg-gray-800 rounded-lg p-4 overflow-x-auto mb-6">
<code class="text-green-400">$results = YourModel::evolve(
  ['status' => 'active', 'category' => 'new'], // Filters
  'name,-created_at' // Sort fields
)->paginate();</code>
</pre>

      <p class="mb-4">Perform common model operations:</p>

      <pre class="bg-gray-800 rounded-lg p-4 overflow-x-auto mb-6">
<code class="text-green-400">$model = new YourModel;
$model->field = 'value';
$model->save();

$model = YourModel::find(1);
$model->update(['field' => 'new value']);
$model->delete();

$dto = $model->toDTO();
$rules = $model->getValidationRules();</code>
</pre>

      <h2 id="features" class="text-center mx-auto max-w-xl text-2xl sm:text-2xl font-semibold tracking-tight text-gray-900 font-display mb-4 mt-24">
        4. Features
      </h2>

      <h3 id="validation" class="text-xl mt-8 mb-4 font-semibold">4.1 Validation</h3>

      <p class="mb-4">Evolve generates validation rules by inspecting model schema and caches them for performance. Customize rules by defining a <code class="bg-gray-200 p-1 rounded">validationRules()</code> method:</p>

      <pre class="bg-gray-800 rounded-lg p-4 overflow-x-auto mb-6">
<code class="text-green-400">// In model class
public function validationRules(string $action): array
{
    return [
        'create' => [
            'title' => 'required|string|max:255|unique:posts',
            'body' => 'required|string'
        ],
        'update' => [
            'title' => 'required|string|max:255|unique:posts,title,'.$this->id,
            'body' => 'required|string'
        ]
    ][$action] ?? [];
}</code>
</pre>

      <p class="mb-4">Access rules:</p>

      <pre class="bg-gray-800 rounded-lg p-4 overflow-x-auto mb-6">
<code class="text-green-400">$rules = $model->getValidationRules('create');</code>
</pre>

      <h3 id="filtering" class="text-xl mt-8 mb-4 font-semibold">4.2 Filtering</h3>

      <p class="mb-4">Evolve makes it easy to filter models and relations:</p>

      <pre class="bg-gray-800 rounded-lg p-4 overflow-x-auto mb-6">
<code class="text-green-400">$results = YourModel::filter([
  'status' => 'active',
  'credit' => ['min' => 100, 'max' => 1000], // Range
  'category.name' => 'Popular', // Related model field
])->get();</code>
</pre>

      <p class="mb-4">Exclude fields from filtering:</p>

      <pre class="bg-gray-800 rounded-lg p-4 overflow-x-auto mb-6">
<code class="text-green-400">// In model class
public static function excludedFields(): array
{
    return ['password', 'remember_token'];
}</code>
</pre>

      <h3 id="sorting" class="text-xl mt-8 mb-4 font-semibold">4.3 Sorting</h3>

      <p class="mb-4">Sort models and relations:</p>

      <pre class="bg-gray-800 rounded-lg p-4 overflow-x-auto mb-6">
<code class="text-green-400">$results = YourModel::sort('name,-created_at')->get();</code>
</pre>

      <p class="mb-4">Sort by relations:</p>

      <pre class="bg-gray-800 rounded-lg p-4 overflow-x-auto mb-6">
<code class="text-green-400">$results = User::sort('posts.published_at')->get();</code>
</pre>

      <p class="mb-4">Exclude fields from sorting:</p>

      <pre class="bg-gray-800 rounded-lg p-4 overflow-x-auto mb-6">
<code class="text-green-400">// In model class
public static function excludedFields(): array
{
    return ['remember_token'];
}</code>
</pre>

      <h3 id="events" class="text-xl mt-8 mb-4 font-semibold">4.4 Events</h3>

      <p class="mb-4">Evolve dispatches model lifecycle events. Listen to them for custom logic:</p>

      <pre class="bg-gray-800 rounded-lg p-4 overflow-x-auto mb-6">
<code class="text-green-400">use Thinkneverland\Evolve\Core\Events\EvolveModelCreated;

Event::listen(EvolveModelCreated::class, function ($event) {
    // Handle created model...
    $model = $event->model;
});</code>
</pre>

      <p class="mb-4">Available events:</p>

      <pre class="bg-gray-800 rounded-lg p-4 overflow-x-auto mb-6">
<code class="text-green-400">EvolveModelCreated
EvolveModelUpdated
EvolveModelDeleted
EvolveModelRestored</code>
</pre>

      <h3 id="policies" class="text-xl mt-8 mb-4 font-semibold">4.5 Policies</h3>

      <p class="mb-4">Evolve auto-registers policies based on permissions. Super admins bypass all checks. Customize policy rules:</p>

      <pre class="bg-gray-800 rounded-lg p-4 overflow-x-auto mb-6">
<code class="text-green-400">class YourModelPolicy
{
    public function view(User $user, YourModel $model)
    {
        return $model->status === 'published';
    }
}</code>
</pre>

      <h3 id="caching" class="text-xl mt-8 mb-4 font-semibold">4.6 Caching</h3>

      <p class="mb-4">Cache query results:</p>

      <pre class="bg-gray-800 rounded-lg p-4 overflow-x-auto mb-6">
<code class="text-green-400">$results = YourModel::cache(3600)->get();</code>
</pre>

      <p class="mb-4">Cache counts:</p>

      <pre class="bg-gray-800 rounded-lg p-4 overflow-x-auto mb-6">
<code class="text-green-400">$countActiveUsers = User::whereActive(1)->cacheCount(3600);</code>
</pre>

      <h3 id="optimizations" class="text-xl mt-8 mb-4 font-semibold">4.7 Optimizations</h3>

      <p class="mb-4">Evolve applies many query optimizations, like:</p>
      <ul class="list-disc ml-6 mb-4">
        <li>Eager loading</li>
        <li>Chunking</li>
        <li>Query plan caching</li>
        <li>Connection pooling</li>
        <li>Index hints</li>
      </ul>

      <p class="mb-4">Manually optimize queries:</p>

      <pre class="bg-gray-800 rounded-lg p-4 overflow-x-auto mb-6">
<code class="text-green-400">$results = YourModel::optimized()->where('status', 'active')->get();</code>
</pre>

      <h3 id="metrics" class="text-xl mt-8 mb-4 font-semibold">4.8 Metrics</h3>
      <p class="mb-4">Evolve automatically tracks insightful query metrics:</p>
      <pre class="bg-gray-800 rounded-lg p-4 overflow-x-auto mb-6">
<code class="text-green-400">$metrics = QueryMonitor::getStats();
// Returns associative array with metrics like total queries,
// slow queries, avg query time, errors, etc.</code>
</pre>
      <p class="mb-4">Record custom metrics:</p>
      <pre class="bg-gray-800 rounded-lg p-4 overflow-x-auto mb-6">
<code class="text-green-400">QueryMonitor::recordQuery('my_metric', [
  'duration' => 150, // ms
  'result_count' => 25,
  // ... other data
]);</code>
</pre>
      <h2 id="advanced" class="text-center mx-auto max-w-xl text-2xl sm:text-2xl font-semibold tracking-tight text-gray-900 font-display mb-4 mt-24">
        5. Advanced Usage
      </h2>
      <h3 id="extensions" class="text-xl mt-8 mb-4 font-semibold">5.1 Extensions</h3>
      <p class="mb-4">Register custom extensions to enhance Evolve models:</p>
      <pre class="bg-gray-800 rounded-lg p-4 overflow-x-auto mb-6">
<code class="text-green-400">ModelRegistry::registerExtension('sluggable', function ($model) {
    // Return config array
    return ['source' => 'title'];
});

// In model class
public static function sluggable(): array
{
    return static::getExtensionConfig('sluggable');
}</code>
</pre>
      <p class="mb-4">Access extension data:</p>
      <pre class="bg-gray-800 rounded-lg p-4 overflow-x-auto mb-6">
<code class="text-green-400">$model->slug; // From extension</code>
</pre>
      <h3 id="commands" class="text-xl mt-8 mb-4 font-semibold">5.2 Commands</h3>
      <p class="mb-4">Evolve provides helpful Artisan commands:</p>
      <ul class="list-disc ml-6 mb-4">
        <li><code class="bg-gray-200 p-1 rounded">evolve:clear-cache</code> - Clear all Evolve caches</li>
        <li><code class="bg-gray-200 p-1 rounded">evolve:generate-report</code> - Generate performance report</li>
        <li><code class="bg-gray-200 p-1 rounded">evolve:monitor</code> - Monitor Evolve metrics in real-time</li>
        <li><code class="bg-gray-200 p-1 rounded">evolve:prune</code> - Prune old Evolve data</li>
      </ul>
      <p>EvolveCore makes it easy to build high-performance Laravel apps with clean, concise code. Install the package, make your models Evolvable, and leverage the powerful features to rapidly develop robust applications.</p>

    </UContainer>
  </div>
</template>

<script setup lang="ts">
</script>