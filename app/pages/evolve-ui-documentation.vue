<template>
  <div class="max-w-xl mx-auto">
    <UContainer class="max-w-4xl py-12 space-y-6">
      <h1 class="text-4xl sm:text-6xl lg:text-7xl tracking-tight text-gray-800 font-bold text-center font-display max-w-4xl mx-auto">
        Evolve Documentation
      </h1>

      <ButtonLink label="Core" size="sm" rounded="md" href="/evolve-core-documentation" />
      <ButtonLink label="API" size="sm" rounded="md" href="/evolve-api-documentation" />
      <ButtonLink label="UI" size="sm" rounded="md" href="/evolve-UI-documentation" />

      <h3 class="max-w-xl text-2xl sm:text-2xl font-semibold tracking-tight text-gray-900 font-display mb-4 mt-8">
        EvolveUI (Extension / Prerelease)
      </h3>

        <div class="mb-8">
          <h2 class="text-2xl font-semibold mb-4" id="table-of-contents">Table of Contents</h2>
          <ul class="list-disc pl-6 space-y-2">
            <li><a href="#installation" class="text-blue-600 hover:underline">Installation</a></li>
            <li><a href="#configuration" class="text-blue-600 hover:underline">Configuration</a></li>
            <li><a href="#basic-usage" class="text-blue-600 hover:underline">Basic Usage</a></li>
            <li><a href="#model-integration" class="text-blue-600 hover:underline">Model Integration</a></li>
            <li><a href="#crud-operations" class="text-blue-600 hover:underline">CRUD Operations</a>
              <ul class="list-disc pl-6 mt-2">
                <li><a href="#index-view" class="text-blue-600 hover:underline">Index View</a></li>
                <li><a href="#create-form" class="text-blue-600 hover:underline">Create Form</a></li>
                <li><a href="#edit-form" class="text-blue-600 hover:underline">Edit Form</a></li>
                <li><a href="#show-view" class="text-blue-600 hover:underline">Show View</a></li>
              </ul>
            </li>
            <li><a href="#relationship-handling" class="text-blue-600 hover:underline">Relationship Handling</a></li>
            <li><a href="#customization" class="text-blue-600 hover:underline">Customization</a></li>
          </ul>
        </div>

        <section id="installation" class="mb-12">
          <h2 class="text-2xl font-semibold mb-4">Installation</h2>
          <p class="mb-4">Install Evolve UI via Composer:</p>
          <pre class="bg-gray-100 p-4 rounded-md mb-4">composer require thinkneverland/evolve-ui</pre>

          <p class="mb-4">Publish the configuration file:</p>
          <pre class="bg-gray-100 p-4 rounded-md mb-4">php artisan vendor:publish --tag=evolve-ui-config</pre>
        </section>

        <section id="configuration" class="mb-12">
          <h2 class="text-2xl font-semibold mb-4">Configuration</h2>
          <p class="mb-4">Configure Evolve UI in <code>config/evolve-ui.php</code>:</p>
          <pre class="bg-gray-100 p-4 rounded-md mb-4">
return [
    'prefix' => env('EVOLVE_UI_PREFIX', ''),
    'middleware' => ['web', 'auth'],
    'per_page' => 10,
    'views' => [
        'layout' => 'layouts.app',
    ],
];
</pre>
          <ul class="list-disc pl-6 space-y-2 mb-4">
            <li><code>prefix</code>: URL prefix for all Evolve UI routes</li>
            <li><code>middleware</code>: Middleware applied to all routes</li>
            <li><code>per_page</code>: Default pagination count</li>
            <li><code>views.layout</code>: Default layout template</li>
          </ul>
        </section>

        <section id="basic-usage" class="mb-12">
          <h2 class="text-2xl font-semibold mb-4">Basic Usage</h2>
          <p class="mb-4">To use Evolve UI with your models:</p>
          <pre class="bg-gray-100 p-4 rounded-md mb-4">
use Thinkneverland\Evolve\Core\Contracts\EvolveModelInterface;
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
}
</pre>
        </section>

        <section id="model-integration" class="mb-12">
          <h2 class="text-2xl font-semibold mb-4">Model Integration</h2>
          <p class="mb-4">Evolve UI automatically discovers and generates CRUD interfaces for models that:</p>
          <ul class="list-disc pl-6 space-y-2 mb-4">
            <li>Implement <code>EvolveModelInterface</code></li>
            <li>Use the <code>EvolveModel</code> trait</li>
            <li>Return <code>true</code> from <code>shouldEvolve()</code></li>
          </ul>

          <p class="mb-4">Key model configuration methods:</p>
          <pre class="bg-gray-100 p-4 rounded-md mb-4">
public static function excludedFields(): array
{
    return ['password', 'remember_token'];
}

public static function excludedRelations(): array
{
    return ['secretData'];
}

public static function getValidationRules(string $action, $model = null): array
{
    return [
        'fields.name' => 'required|string|max:255',
        'fields.email' => 'required|email|unique:users,email'
    ];
}
</pre>
        </section>

        <section id="crud-operations" class="mb-12">
          <h2 class="text-2xl font-semibold mb-4">CRUD Operations</h2>

          <section id="index-view" class="mb-8">
            <h3 class="text-xl font-semibold mb-4">Index View</h3>
            <p class="mb-4">Features:</p>
            <ul class="list-disc pl-6 space-y-2 mb-4">
              <li>Searchable columns</li>
              <li>Sortable columns</li>
              <li>Configurable pagination</li>
              <li>Delete confirmation modal</li>
            </ul>
            <p class="mb-4">Routes are automatically generated as:</p>
            <pre class="bg-gray-100 p-4 rounded-md mb-4">/evolve/{model-plural}</pre>
          </section>

          <section id="create-form" class="mb-8">
            <h3 class="text-xl font-semibold mb-4">Create Form</h3>
            <p class="mb-4">Features:</p>
            <ul class="list-disc pl-6 space-y-2 mb-4">
              <li>Automatic field generation based on model attributes</li>
              <li>Nested relationship support</li>
              <li>Validation using model rules</li>
              <li>Transaction support for related data</li>
            </ul>
            <p class="mb-4">Routes are automatically generated as:</p>
            <pre class="bg-gray-100 p-4 rounded-md mb-4">/evolve/{model-plural}/create</pre>
          </section>

          <section id="edit-form" class="mb-8">
            <h3 class="text-xl font-semibold mb-4">Edit Form</h3>
            <p class="mb-4">Features:</p>
            <ul class="list-disc pl-6 space-y-2 mb-4">
              <li>Pre-populated fields</li>
              <li>Nested relationship editing</li>
              <li>Validation with existing model context</li>
              <li>Transaction support for updates</li>
            </ul>
            <p class="mb-4">Routes are automatically generated as:</p>
            <pre class="bg-gray-100 p-4 rounded-md mb-4">/evolve/{model-plural}/{id}/edit</pre>
          </section>

          <section id="show-view" class="mb-8">
            <h3 class="text-xl font-semibold mb-4">Show View</h3>
            <p class="mb-4">Features:</p>
            <ul class="list-disc pl-6 space-y-2 mb-4">
              <li>Detailed model attribute display</li>
              <li>Related data visualization</li>
              <li>Formatted date/time fields</li>
              <li>JSON data pretty printing</li>
            </ul>
            <p class="mb-4">Routes are automatically generated as:</p>
            <pre class="bg-gray-100 p-4 rounded-md mb-4">/evolve/{model-plural}/{id}</pre>
          </section>
        </section>

        <section id="relationship-handling" class="mb-12">
          <h2 class="text-2xl font-semibold mb-4">Relationship Handling</h2>
          <p class="mb-4">Evolve UI supports nested relationships in forms:</p>
          <pre class="bg-gray-100 p-4 rounded-md mb-4">
class Order extends Model implements EvolveModelInterface
{
    use EvolveModel;

    public function customer()
    {
        return $this->belongsTo(Customer::class);
    }

    public function items()
    {
        return $this->hasMany(OrderItem::class);
    }

    public static function getAllRelations(): array
    {
        return [
            'customer' => ['type' => 'belongsTo'],
            'items' => ['type' => 'hasMany']
        ];
    }
}
</pre>
          <p class="mb-4">The UI will automatically:</p>
          <ul class="list-disc pl-6 space-y-2 mb-4">
            <li>Generate nested forms for related models</li>
            <li>Handle creation/updates in transactions</li>
            <li>Support both single and multiple relationships</li>
            <li>Apply validation rules recursively</li>
          </ul>
        </section>

        <section id="customization" class="mb-12">
          <h2 class="text-2xl font-semibold mb-4">Customization</h2>
          <p class="mb-4">Publish views for customization:</p>
          <pre class="bg-gray-100 p-4 rounded-md mb-4">php artisan vendor:publish --tag=evolve-ui-views</pre>

          <p class="mb-4">Available views for customization:</p>
          <ul class="list-disc pl-6 space-y-2 mb-4">
            <li><code>index.blade.php</code>: List view template</li>
            <li><code>create.blade.php</code>: Creation form template</li>
            <li><code>edit.blade.php</code>: Edit form template</li>
            <li><code>show.blade.php</code>: Detail view template</li>
            <li><code>fields.blade.php</code>: Field rendering partial</li>
          </ul>

          <p class="mb-4">Change the layout template in config:</p>
          <pre class="bg-gray-100 p-4 rounded-md mb-4">
// config/evolve-ui.php
return [
    'views' => [
        'layout' => 'admin.layouts.app'
    ]
];
</pre>
        </section>

    </UContainer>
  </div>
</template>
<script setup lang="ts">
</script>