<template>
  <div class="max-w-xl mx-auto">
    <UContainer class="max-w-4xl py-12 space-y-6">
      <h1 class="text-4xl sm:text-6xl lg:text-7xl tracking-tight text-gray-800 font-bold text-center font-display max-w-4xl mx-auto">
        EvolveUI Documentation
      </h1>

      <ButtonLink label="Core" size="md" rounded="md" href="/evolve-core-documentation" />
      <ButtonLink label="API" size="md" rounded="md" href="/evolve-api-documentation" />
      <ButtonLink label="UI" size="md" rounded="md" href="/evolve-UI-documentation" />

      <h3 class="max-w-xl text-2xl sm:text-2xl font-semibold tracking-tight text-gray-900 font-display mb-4 mt-8">
        Extension for EvolveCORE (Foundation / Prerelease)
      </h3>
      <nav class="my-8 p-6 bg-gray-50 rounded-lg">
        <h2 class="text-2xl font-semibold mb-4">Table of Contents</h2>
        <ul class="space-y-2">
          <li><a href="#introduction" class="text-blue-600 hover:text-blue-800">Introduction</a></li>
          <li><a href="#installation" class="text-blue-600 hover:text-blue-800">Installation</a></li>
          <li><a href="#configuration" class="text-blue-600 hover:text-blue-800">Configuration</a></li>
          <li><a href="#model-setup" class="text-blue-600 hover:text-blue-800">Model Setup</a></li>
          <li><a href="#routing" class="text-blue-600 hover:text-blue-800">Automatic Routing</a></li>
          <li>
            <a href="#components" class="text-blue-600 hover:text-blue-800">Components</a>
            <ul class="ml-4 mt-2 space-y-1">
              <li><a href="#index-component" class="text-blue-600 hover:text-blue-800">Index Component</a></li>
              <li><a href="#create-component" class="text-blue-600 hover:text-blue-800">Create Component</a></li>
              <li><a href="#edit-component" class="text-blue-600 hover:text-blue-800">Edit Component</a></li>
              <li><a href="#show-component" class="text-blue-600 hover:text-blue-800">Show Component</a></li>
              <li><a href="#fields-partial" class="text-blue-600 hover:text-blue-800">Fields Partial</a></li>
            </ul>
          </li>
          <li><a href="#customization" class="text-blue-600 hover:text-blue-800">Customization</a></li>
        </ul>
      </nav>
      <section id="introduction" class="my-8">
        <h2 class="text-center mx-auto max-w-xl text-2xl sm:text-2xl font-semibold tracking-tight text-gray-900 font-display mb-4 mt-24">
          Introduction
        </h2>
        <p class="text-gray-600 max-w-3xl mx-auto">
          EvolveUI is a powerful Laravel package that extends EvolveCORE to provide a complete CRUD interface built with Livewire and DaisyUI. It automatically generates user interfaces for your models with support for complex relationships and nested data structures.
        </p>
      </section>
      <section id="installation" class="my-8">
        <h2 class="text-center mx-auto max-w-xl text-2xl sm:text-2xl font-semibold tracking-tight text-gray-900 font-display mb-4 mt-24">
          Installation
        </h2>
        <div class="max-w-3xl mx-auto">
          <p class="text-gray-600 mb-4">
            1. Install the package via Composer:
          </p>
          <pre><code class="language-bash">composer require thinkneverland/evolve-ui</code></pre>

          <p class="text-gray-600 mt-4 mb-4">
            2. Publish the configuration and views:
          </p>
          <pre><code class="language-bash">php artisan vendor:publish --provider="Thinkneverland\Evolve\UI\EvolveUiServiceProvider"</code></pre>
        </div>
      </section>
      <section id="configuration" class="my-8">
      <h2 class="text-center mx-auto max-w-xl text-2xl sm:text-2xl font-semibold tracking-tight text-gray-900 font-display mb-4 mt-24">
        Configuration
      </h2>
      <div class="max-w-3xl mx-auto">
        <p class="text-gray-600 mb-4">
          After publishing the configuration file, you can find it at <code>config/evolve-ui.php</code>. Available options:
        </p>
        <pre><code class="language-php">
return [
    // URL prefix for all EvolveUI routes
    'prefix' => env('EVOLVE_UI_PREFIX', ''),

    // Middleware applied to all EvolveUI routes
    'middleware' => ['web', 'auth'],

    // Number of items to show per page in index views
    'per_page' => 10,

    // View customization
    'views' => [
        'layout' => 'layouts.app', // The layout view to extend
    ],
];</code></pre>
      </div>
    </section>
      <section id="model-setup" class="my-8">
        <h2 class="text-center mx-auto max-w-xl text-2xl sm:text-2xl font-semibold tracking-tight text-gray-900 font-display mb-4 mt-24">
          Model Setup
        </h2>
        <div class="max-w-3xl mx-auto">
          <p class="text-gray-600 mb-4">
            To use EvolveUI with your models, implement the EvolveModelInterface and add required methods:
          </p>
          <pre><code class="language-php">use Thinkneverland\Evolve\Core\Contracts\EvolveModelInterface;

class User extends Model implements EvolveModelInterface
{
    public static function shouldEvolve(): bool
    {
        return true;
    }

    public static function excludedFields(): array
    {
        return ['password', 'remember_token'];
    }

    public static function excludedRelations(): array
    {
        return [];
    }

    public static function getValidationRules(string $operation): array
    {
        return [
            'fields.name' => 'required|string|max:255',
            'fields.email' => 'required|email|unique:users,email',
        ];
    }

    public static function getAllRelations(): array
    {
        return [
            'profile' => Profile::class,
            'posts' => Post::class,
        ];
    }
}</code></pre>
        </div>
      </section>

      <section id="routing" class="my-8">
        <h2 class="text-center mx-auto max-w-xl text-2xl sm:text-2xl font-semibold tracking-tight text-gray-900 font-display mb-4 mt-24">
          Automatic Routing
        </h2>
        <div class="max-w-3xl mx-auto">
          <p class="text-gray-600 mb-4">
            EvolveUI automatically registers RESTful routes for all models implementing EvolveModelInterface. Routes are prefixed based on your configuration:
          </p>
          <pre><code class="language-php">// Example routes for User model:
evolve/users             // Index
evolve/users/create      // Create form
evolve/users/{id}        // Show
evolve/users/{id}/edit   // Edit form</code></pre>
        </div>
      </section>
      <section id="components" class="my-8">
        <h2 class="text-center mx-auto max-w-xl text-2xl sm:text-2xl font-semibold tracking-tight text-gray-900 font-display mb-4 mt-24">
          Components
        </h2>

        <div id="index-component" class="max-w-3xl mx-auto mt-8">
          <h3 class="text-xl font-semibold mb-4">Index Component</h3>
          <p class="text-gray-600 mb-4">The Index Component provides a powerful data grid with built-in features:</p>
          <ul class="list-disc ml-6 mb-4 text-gray-600">
            <li>Real-time search with automatic field detection</li>
            <li>Column sorting with persistent state</li>
            <li>Pagination with configurable items per page</li>
            <li>Delete confirmation modal</li>
            <li>Query string parameters for sharing filtered views</li>
          </ul>
          <p class="text-gray-600 mb-4">Implementation example:</p>
          <pre><code class="language-php">class EvolveIndexComponent extends Component
{
    use WithPagination;

    public $search = '';
    public $sortField = 'id';
    public $sortDirection = 'asc';
    public $perPage;

    protected $queryString = [
        'search',
        'sortField',
        'sortDirection',
        'perPage'
    ];

    public function sortBy($field)
    {
        if ($this->sortField === $field) {
            $this->sortDirection = $this->sortDirection === 'asc' ? 'desc' : 'asc';
        } else {
            $this->sortField = $field;
            $this->sortDirection = 'asc';
        }
    }

    protected function getSearchableFields()
    {
        $model = new $this->modelClass;
        $columns = \Schema::getColumnListing($model->getTable());
        return array_diff($columns, $this->modelClass::excludedFields());
    }
}</code></pre>
        </div>
        <div id="create-component" class="max-w-3xl mx-auto mt-8">
          <h3 class="text-xl font-semibold mb-4">Create Component</h3>
          <p class="text-gray-600 mb-4">The Create Component handles complex form generation and data persistence:</p>
          <ul class="list-disc ml-6 mb-4 text-gray-600">
            <li>Automatic field generation based on model attributes</li>
            <li>Support for nested relationships</li>
            <li>Transaction-based saving</li>
            <li>Dynamic validation rules</li>
            <li>Error handling with user feedback</li>
          </ul>
          <p class="text-gray-600 mb-4">Relationship handling example:</p>
          <pre><code class="language-php">protected function createModelWithRelations($modelClass, $data)
{
    $relations = array_diff(
        array_keys($modelClass::getAllRelations()),
        $modelClass::excludedRelations()
    );

    // Filter out relation data
    $modelData = array_filter($data, function ($key) use ($relations) {
        return !in_array($key, $relations);
    }, ARRAY_FILTER_USE_KEY);

    // Create main model
    $model = $modelClass::create($modelData);

    // Create related models
    foreach ($relations as $relation) {
        if (isset($data[$relation])) {
            $relationData = $data[$relation];
            $relationClass = get_class($model->$relation()->getRelated());
            $relationModel = $this->createModelWithRelations(
                $relationClass,
                $relationData
            );
            $model->$relation()->save($relationModel);
        }
    }

    return $model;
}</code></pre>
        </div>
        <div id="edit-component" class="max-w-3xl mx-auto mt-8">
          <h3 class="text-xl font-semibold mb-4">Edit Component</h3>
          <p class="text-gray-600 mb-4">The Edit Component extends creation functionality with advanced features:</p>
          <ul class="list-disc ml-6 mb-4 text-gray-600">
            <li>Recursive relationship loading</li>
            <li>Smart field extraction</li>
            <li>Handles both single and many-to-many relationships</li>
            <li>Maintains data integrity with transactions</li>
            <li>Context-aware validation rules</li>
          </ul>
          <p class="text-gray-600 mb-4">Relationship update example:</p>
          <pre><code class="language-php">protected function updateModelWithRelations($model, $data)
{
    $relations = array_diff(
        array_keys($this->modelClass::getAllRelations()),
        $this->modelClass::excludedRelations()
    );

    // Update main model
    $modelData = array_filter($data, function ($key) use ($relations) {
        return !in_array($key, $relations);
    }, ARRAY_FILTER_USE_KEY);

    $model->update($modelData);

    // Update relationships
    foreach ($relations as $relation) {
        if (isset($data[$relation])) {
            $relationData = $data[$relation];
            if (is_array($relationData) && array_key_exists(0, $relationData)) {
                // Handle many relations
                $model->$relation()->delete();
                foreach ($relationData as $item) {
                    $relationClass = get_class($model->$relation()->getRelated());
                    $relationModel = $this->createModelWithRelations(
                        $relationClass,
                        $item
                    );
                    $model->$relation()->save($relationModel);
                }
            } else {
                // Handle single relation
                $relationClass = get_class($model->$relation()->getRelated());
                if ($model->$relation) {
                    $this->updateModelWithRelations(
                        $model->$relation,
                        $relationData
                    );
                } else {
                    $relationModel = $this->createModelWithRelations(
                        $relationClass,
                        $relationData
                    );
                    $model->$relation()->save($relationModel);
                }
            }
        }
    }

    return $model;
}</code></pre>
        </div>
        <div id="show-component" class="max-w-3xl mx-auto mt-8">
          <h3 class="text-xl font-semibold mb-4">Show Component</h3>
          <p class="text-gray-600 mb-4">The Show Component provides detailed record viewing:</p>
          <ul class="list-disc ml-6 mb-4 text-gray-600">
            <li>Automatic relationship loading</li>
            <li>Smart data type formatting</li>
            <li>Handles nested relationship display</li>
            <li>Quick navigation to edit view</li>
            <li>Responsive layout for all screen sizes</li>
          </ul>
          <p class="text-gray-600 mb-4">Model loading example:</p>
          <pre><code class="language-php">protected function loadModel()
{
    $this->modelInstance = $this->modelClass::with($this->getRelations())
        ->findOrFail($this->modelId);
}

protected function getRelations()
{
    return array_diff(
        array_keys($this->modelClass::getAllRelations()),
        $this->modelClass::excludedRelations()
    );
}</code></pre>
        </div>
        <div id="best-practices" class="max-w-3xl mx-auto mt-8">
          <h3 class="text-xl font-semibold mb-4">Best Practices</h3>
          <ul class="list-disc ml-6 mb-4 text-gray-600">
            <li>Always implement model interfaces correctly</li>
            <li>Use transactions for complex operations</li>
            <li>Define clear validation rules</li>
            <li>Properly exclude sensitive fields</li>
            <li>Handle relationships carefully</li>
            <li>Test recursive operations with deep relationships</li>
          </ul>
        </div>
      </section>
    </UContainer>
  </div>
</template>