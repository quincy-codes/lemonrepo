<template>
  <div class="max-w-xl mx-auto">
    <UContainer class="max-w-8xl py-12 space-y-6">
      <h1 class="text-4xl sm:text-6xl lg:text-7xl tracking-tight text-gray-800 font-bold text-center font-display max-w-4xl mx-auto">
        Evolve Documentation
      </h1>

      <div class="w-full columns-3 py-4" >
        <ButtonLink label="Core" size="sm" rounded="sm" href="/evolve-core-documentation" />
        <ButtonLink label="API" size="sm" rounded="sm" href="/evolve-api-documentation" />
        <ButtonLink label="UI" size="sm" rounded="sm" href="/evolve-UI-documentation" />
      </div>

      <h3 class="max-w-xl text-2xl sm:text-2xl font-semibold tracking-tight text-gray-900 font-display mb-4 mt-8">
        EvolveUI (Extension / Prerelease)
      </h3>

        <div class="grid grid-cols-1 md:grid-cols-3 gap-4 mb-12">
          <a href="#getting-started" class="block p-6 bg-white rounded-lg shadow hover:shadow-lg transition-shadow">
            <h2 class="text-xl font-semibold mb-2">Getting Started</h2>
            <p class="text-gray-600">Installation and basic setup</p>
          </a>
          <a href="#configuration" class="block p-6 bg-white rounded-lg shadow hover:shadow-lg transition-shadow">
            <h2 class="text-xl font-semibold mb-2">Configuration</h2>
            <p class="text-gray-600">Customize and configure</p>
          </a>
          <a href="#usage" class="block p-6 bg-white rounded-lg shadow hover:shadow-lg transition-shadow">
            <h2 class="text-xl font-semibold mb-2">Usage Guide</h2>
            <p class="text-gray-600">Implementation examples</p>
          </a>
        </div>

        <div class="bg-gray-100 rounded-lg p-6 mb-12">
          <h2 class="text-2xl font-semibold mb-4">Table of Contents</h2>
          <nav>
            <ul class="space-y-2">
              <li>
                <a href="#getting-started" class="text-blue-600 hover:underline">1. Getting Started</a>
                <ul class="ml-4 mt-2 space-y-1">
                  <li><a href="#installation" class="text-blue-600 hover:underline">1.1 Installation</a></li>
                  <li><a href="#requirements" class="text-blue-600 hover:underline">1.2 Requirements</a></li>
                </ul>
              </li>
              <li>
                <a href="#configuration" class="text-blue-600 hover:underline">2. Configuration</a>
                <ul class="ml-4 mt-2 space-y-1">
                  <li><a href="#basic-configuration" class="text-blue-600 hover:underline">2.1 Basic Configuration</a></li>
                  <li><a href="#model-setup" class="text-blue-600 hover:underline">2.2 Model Setup</a></li>
                  <li><a href="#routing" class="text-blue-600 hover:underline">2.3 Routing</a></li>
                </ul>
              </li>
              <li>
                <a href="#usage" class="text-blue-600 hover:underline">3. Usage Guide</a>
                <ul class="ml-4 mt-2 space-y-1">
                  <li><a href="#crud-operations" class="text-blue-600 hover:underline">3.1 CRUD Operations</a></li>
                  <li><a href="#relationships" class="text-blue-600 hover:underline">3.2 Handling Relationships</a></li>
                  <li><a href="#validation" class="text-blue-600 hover:underline">3.3 Validation</a></li>
                  <li><a href="#customization" class="text-blue-600 hover:underline">3.4 Customization</a></li>
                </ul>
              </li>
            </ul>
          </nav>
        </div>

        <section id="getting-started" class="mb-12">
          <h2 class="text-2xl sm:text-2xl font-semibold tracking-tight text-gray-900 font-display mb-6">1. Getting Started</h2>

          <h3 id="installation" class="text-xl font-semibold mb-4">1.1 Installation</h3>
          <p class="mb-4">Install Evolve UI via Composer:</p>
          <pre class="bg-gray-800 rounded-lg p-4 overflow-x-auto mb-6">
<code class="text-green-400">composer require thinkneverland/evolve-ui</code></pre>

          <h3 id="requirements" class="text-xl font-semibold mb-4">1.2 Requirements</h3>
          <ul class="list-disc ml-6 space-y-2 mb-6">
            <li>PHP ^7.4|^8.0</li>
            <li>Laravel ^8.0|^9.0</li>
            <li>Livewire ^2.0</li>
            <li>Evolve Core</li>
          </ul>
        </section>

        <section id="configuration" class="mb-12">
          <h2 class="text-2xl sm:text-2xl font-semibold tracking-tight text-gray-900 font-display mb-6">2. Configuration</h2>

          <h3 id="basic-configuration" class="text-xl font-semibold mb-4">2.1 Basic Configuration</h3>
          <p class="mb-4">Publish the configuration file:</p>
          <pre class="bg-gray-800 rounded-lg p-4 overflow-x-auto mb-6">
<code class="text-green-400">php artisan vendor:publish --tag=evolve-ui-config</code></pre>

          <p class="mb-4">Configure in <code class="bg-gray-100 px-2 py-1 rounded">config/evolve-ui.php</code>:</p>
          <pre class="bg-gray-800 rounded-lg p-4 overflow-x-auto mb-6">
<code class="text-green-400">return [
    'prefix' => env('EVOLVE_UI_PREFIX', ''),
    'middleware' => ['web', 'auth'],
    'per_page' => 10,
    'views' => [
        'layout' => 'layouts.app',
    ],
];</code></pre>

          <h3 id="model-setup" class="text-xl font-semibold mb-4">2.2 Model Setup</h3>
          <p class="mb-4">Implement the EvolveModelInterface in your models:</p>
          <pre class="bg-gray-800 rounded-lg p-4 overflow-x-auto mb-6">
<code class="text-green-400">use Thinkneverland\Evolve\Core\Contracts\EvolveModelInterface;
use Thinkneverland\Evolve\Core\Traits\EvolveModel;

class User extends Model implements EvolveModelInterface
{
    use EvolveModel;

    public static function shouldEvolve(): bool
    {
        return true;
    }

    public static function excludedFields(): array
    {
        return ['password'];
    }

    public static function excludedRelations(): array
    {
        return [];
    }
}</code></pre>

          <h3 id="routing" class="text-xl font-semibold mb-4">2.3 Routing</h3>
          <p class="mb-4">Routes are automatically registered for your Evolve models:</p>
          <ul class="list-disc ml-6 space-y-2 mb-6">
            <li>Index: <code class="bg-gray-100 px-2 py-1 rounded">/evolve/{model-plural}</code></li>
            <li>Create: <code class="bg-gray-100 px-2 py-1 rounded">/evolve/{model-plural}/create</code></li>
            <li>Edit: <code class="bg-gray-100 px-2 py-1 rounded">/evolve/{model-plural}/{id}/edit</code></li>
            <li>Show: <code class="bg-gray-100 px-2 py-1 rounded">/evolve/{model-plural}/{id}</code></li>
          </ul>
        </section>

        <section id="usage" class="mb-12">
          <h2 class="text-2xl sm:text-2xl font-semibold tracking-tight text-gray-900 font-display mb-6">3. Usage Guide</h2>

          <h3 id="crud-operations" class="text-xl font-semibold mb-4">3.1 CRUD Operations</h3>
          <p class="mb-4">Evolve UI provides full CRUD functionality with:</p>
          <ul class="list-disc ml-6 space-y-2 mb-6">
            <li>Searchable & sortable index views</li>
            <li>Automatic form generation</li>
            <li>Nested relationship handling</li>
            <li>Delete confirmations</li>
            <li>Transaction support</li>
          </ul>

          <h3 id="relationships" class="text-xl font-semibold mb-4">3.2 Handling Relationships</h3>
          <p class="mb-4">Define relationships in your model:</p>
          <pre class="bg-gray-800 rounded-lg p-4 overflow-x-auto mb-6">
<code class="text-green-400">public static function getAllRelations(): array
{
    return [
        'customer' => ['type' => 'belongsTo'],
        'items' => ['type' => 'hasMany']
    ];
}</code></pre>

          <h3 id="validation" class="text-xl font-semibold mb-4">3.3 Validation</h3>
          <p class="mb-4">Implement validation rules:</p>
          <pre class="bg-gray-800 rounded-lg p-4 overflow-x-auto mb-6">
<code class="text-green-400">public static function getValidationRules(string $action, $model = null): array
{
    return [
        'fields.name' => 'required|string|max:255',
        'fields.email' => 'required|email|unique:users,email'
    ];
}</code></pre>

          <h3 id="customization" class="text-xl font-semibold mb-4">3.4 Customization</h3>
          <p class="mb-4">Publish view files for customization:</p>
          <pre class="bg-gray-800 rounded-lg p-4 overflow-x-auto mb-6">
<code class="text-green-400">php artisan vendor:publish --tag=evolve-ui-views</code></pre>

          <p class="mb-4">Available views:</p>
          <ul class="list-disc ml-6 space-y-2 mb-6">
            <li><code class="bg-gray-100 px-2 py-1 rounded">index.blade.php</code>: List view template</li>
            <li><code class="bg-gray-100 px-2 py-1 rounded">create.blade.php</code>: Creation form template</li>
            <li><code class="bg-gray-100 px-2 py-1 rounded">edit.blade.php</code>: Edit form template</li>
            <li><code class="bg-gray-100 px-2 py-1 rounded">show.blade.php</code>: Detail view template</li>
            <li><code class="bg-gray-100 px-2 py-1 rounded">fields.blade.php</code>: Field rendering partial</li>
          </ul>
        </section>
    </UContainer>
  </div>
</template>
<script setup lang="ts">
</script>