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
        EvolveAPI (Extension / Prerelease)
      </h3>

        <div class="bg-blue-50 border-l-4 border-blue-500 p-4 mb-8">
          <p class="text-blue-700">EvolveAPI extends EvolveCore to provide automatic REST API generation with advanced features like relation handling, hooks, events, and Swagger documentation.</p>
        </div>

        <div class="mb-8">
          <h2 class="text-2xl font-bold mb-4">Table of Contents</h2>
          <ul class="space-y-2">
            <li><a href="#requirements" class="text-blue-600 hover:underline">1. Requirements</a></li>
            <li><a href="#installation" class="text-blue-600 hover:underline">2. Installation</a></li>
            <li><a href="#configuration" class="text-blue-600 hover:underline">3. Configuration</a></li>
            <li><a href="#model-setup" class="text-blue-600 hover:underline">4. Model Setup</a></li>
            <li><a href="#api-features" class="text-blue-600 hover:underline">5. API Features</a></li>
            <li><a href="#hooks" class="text-blue-600 hover:underline">6. Hook Methods</a></li>
            <li><a href="#events" class="text-blue-600 hover:underline">7. Events</a></li>
            <li><a href="#api-examples" class="text-blue-600 hover:underline">8. API Examples</a></li>
            <li><a href="#swagger-docs" class="text-blue-600 hover:underline">9. Swagger Documentation</a></li>
          </ul>
        </div>

        <section id="requirements" class="mb-12">
          <h2 class="text-2xl font-bold mb-4">1. Requirements</h2>
          <ul class="list-disc pl-6 space-y-2">
            <li>PHP ^7.4|^8.0</li>
            <li>Laravel ^8.0|^9.0</li>
            <li>thinkneverland/evolve-core</li>
            <li>darkaonline/l5-swagger ^8.0</li>
          </ul>
        </section>

        <section id="installation" class="mb-12">
          <h2 class="text-2xl font-bold mb-4">2. Installation</h2>
          <div class="bg-gray-800 rounded-lg p-4 mb-4">
            <code class="text-green-400">composer require thinkneverland/evolve-api</code>
          </div>
          <p class="mb-4">The service provider will be automatically registered. Publish the assets:</p>
          <div class="bg-gray-800 rounded-lg p-4">
            <code class="text-green-400">php artisan vendor:publish --provider="Thinkneverland\Evolve\Api\Providers\EvolveApiServiceProvider"</code>
          </div>
        </section>

        <section id="configuration" class="mb-12">
          <h2 class="text-2xl font-bold mb-4">3. Configuration</h2>
          <p class="mb-4">After publishing, configure in <code class="bg-gray-100 px-2 py-1 rounded">config/evolve-api.php</code>:</p>
          <div class="bg-gray-800 rounded-lg p-4">
      <pre class="text-green-400">
return [
    'route_prefix' => 'evolve-api', // API route prefix
    'middleware' => ['api'], // Add custom middleware
    'avoid_duplicates' => false, // Global duplicate prevention setting
];
      </pre>
          </div>
        </section>

        <section id="model-setup" class="mb-12">
          <h2 class="text-2xl font-bold mb-4">4. Model Setup</h2>
          <p class="mb-4">Example model setup with all available options:</p>
          <div class="bg-gray-800 rounded-lg p-4">
      <pre class="text-green-400">
namespace App\Models;

use Illuminate\Database\Eloquent\Model;
use Thinkneverland\Evolve\Core\Traits\SortableFilterableTrait;

class Product extends Model
{
    use SortableFilterableTrait;

    protected $fillable = ['name', 'price', 'description'];

    // Define filterable fields
    protected $filterable = [
        'name',
        'price',
        'category_id'
    ];

    // Define sortable fields
    protected $sortable = [
        'name',
        'price',
        'created_at'
    ];

    // Define default per page
    protected $perPage = 25;

    // Define unique fields for duplicate prevention
    public static function uniqueFields()
    {
        return ['sku', 'name'];
    }

    // Define relations that can be loaded
    public function category()
    {
        return $this->belongsTo(Category::class);
    }

    // Define validation rules
    public static function getValidationRules($action = 'create', $model = null)
    {
        return [
            'name' => 'required|string|max:255',
            'price' => 'required|numeric|min:0',
            'category_id' => 'required|exists:categories,id'
        ];
    }
}
      </pre>
          </div>
        </section>

        <section id="api-features" class="mb-12">
          <h2 class="text-2xl font-bold mb-4">5. API Features</h2>

          <h3 class="text-xl font-bold mt-6 mb-4">5.1 Basic CRUD Operations</h3>
          <div class="overflow-x-auto">
            <table class="min-w-full bg-white border border-gray-300 rounded-lg">
              <thead class="bg-gray-50">
              <tr>
                <th class="px-6 py-3 border-b">Method</th>
                <th class="px-6 py-3 border-b">Endpoint</th>
                <th class="px-6 py-3 border-b">Description</th>
              </tr>
              </thead>
              <tbody class="divide-y divide-gray-300">
              <tr>
                <td class="px-6 py-4"><code class="bg-green-100 px-2 py-1 rounded">GET</code></td>
                <td class="px-6 py-4"><code>/evolve-api/products</code></td>
                <td class="px-6 py-4">List with filtering, sorting, pagination</td>
              </tr>
              <tr>
                <td class="px-6 py-4"><code class="bg-blue-100 px-2 py-1 rounded">POST</code></td>
                <td class="px-6 py-4"><code>/evolve-api/products</code></td>
                <td class="px-6 py-4">Create with relations</td>
              </tr>
              <tr>
                <td class="px-6 py-4"><code class="bg-green-100 px-2 py-1 rounded">GET</code></td>
                <td class="px-6 py-4"><code>/evolve-api/products/{id}</code></td>
                <td class="px-6 py-4">Get single record</td>
              </tr>
              <tr>
                <td class="px-6 py-4"><code class="bg-yellow-100 px-2 py-1 rounded">PUT</code></td>
                <td class="px-6 py-4"><code>/evolve-api/products/{id}</code></td>
                <td class="px-6 py-4">Update with relations</td>
              </tr>
              <tr>
                <td class="px-6 py-4"><code class="bg-red-100 px-2 py-1 rounded">DELETE</code></td>
                <td class="px-6 py-4"><code>/evolve-api/products/{id}</code></td>
                <td class="px-6 py-4">Delete with hooks</td>
              </tr>
              </tbody>
            </table>
          </div>

          <h3 class="text-xl font-bold mt-8 mb-4">5.2 Advanced Features</h3>
          <ul class="list-disc pl-6 space-y-2">
            <li>Automatic relation handling (nested create/update)</li>
            <li>Duplicate prevention</li>
            <li>Database transactions</li>
            <li>Before/After hooks</li>
            <li>Events dispatching</li>
            <li>Validation</li>
          </ul>
        </section>

        <section id="hooks" class="mb-12">
          <h2 class="text-2xl font-bold mb-4">6. Hook Methods</h2>
          <p class="mb-4">Create a controller in <code class="bg-gray-100 px-2 py-1 rounded">App\Http\Controllers\Api</code> to implement hooks:</p>
          <div class="bg-gray-800 rounded-lg p-4">
      <pre class="text-green-400">
namespace App\Http\Controllers\Api;

class ProductController
{
    public function beforeUpdate($request, &$data, $model)
    {
        // Modify data before update
        $data['modified_by'] = auth()->id();
    }

    public function afterUpdate($request, $model)
    {
        // Perform actions after update
        Cache::tags('products')->flush();
    }

    public function beforeDelete($request, $model)
    {
        // Prevent deletion if needed
        if ($model->has_orders) {
            return response()->json([
                'error' => 'Cannot delete product with orders'
            ], 422);
        }
    }

    public function afterDelete($request, $model)
    {
        // Cleanup after deletion
        Storage::delete($model->image_path);
    }
}
      </pre>
          </div>
        </section>

        <section id="events" class="mb-12">
          <h2 class="text-2xl font-bold mb-4">7. Events</h2>
          <p class="mb-4">EvolveAPI dispatches the following events:</p>
          <ul class="list-disc pl-6 space-y-2">
            <li><code class="bg-gray-100 px-2 py-1 rounded">EvolveModelCreated</code></li>
            <li><code class="bg-gray-100 px-2 py-1 rounded">EvolveModelUpdated</code></li>
            <li><code class="bg-gray-100 px-2 py-1 rounded">EvolveModelDeleted</code></li>
          </ul>
        </section>

        <section id="api-examples" class="mb-12">
          <h2 class="text-2xl font-bold mb-4">8. API Examples</h2>

          <h3 class="text-xl font-bold mt-6 mb-4">8.1 Simple Queries</h3>
          <div class="space-y-4">
            <div>
              <p class="font-bold mb-2">Basic List:</p>
              <code class="bg-gray-100 px-2 py-1 rounded block">GET /evolve-api/products</code>
            </div>
            <div>
              <p class="font-bold mb-2">Single Filter:</p>
              <code class="bg-gray-100 px-2 py-1 rounded block">GET /evolve-api/products?filter[price][gt]=100</code>
            </div>
            <div>
              <p class="font-bold mb-2">Simple Sort:</p>
              <code class="bg-gray-100 px-2 py-1 rounded block">GET /evolve-api/products?sort=-created_at</code>
            </div>
          </div>

          <h3 class="text-xl font-bold mt-6 mb-4">8.2 Complex Queries</h3>
          <div class="space-y-4">
            <div>
              <p class="font-bold mb-2">Multiple Filters with Relations:</p>
              <code class="bg-gray-100 px-2 py-1 rounded block">GET /evolve-api/products?filter[price][between]=10,100&filter[category.name]=Electronics&sort=-price,name&per_page=25</code>
            </div>
            <div>
              <p class="font-bold mb-2">Create with Relations:</p>
              <div class="bg-gray-800 rounded-lg p-4">
          <pre class="text-green-400">
POST /evolve-api/products
{
    "name": "New Product",
    "price": 199.99,
    "category": {
        "name": "Electronics"
    },
    "tags": [
        {"name": "Featured"},
        {"name": "New"}
    ]
}
          </pre>
              </div>
            </div>
            <div>
              <p class="font-bold mb-2">Update with Duplicate Prevention:</p>
              <div class="bg-gray-800 rounded-lg p-4">
          <pre class="text-green-400">
PUT /evolve-api/products/1?avoid_duplicates=true
{
    "name": "Updated Product",
    "variants": [
        {
            "sku": "PROD-1-VAR-1",
            "price": 199.99
        }
    ]
}
          </pre>
              </div>
            </div>
          </div>
        </section>

        <section id="swagger-docs" class="mb-12">
          <h2 class="text-2xl font-bold mb-4">9. Swagger Documentation</h2>
          <p class="mb-4">Generate and access Swagger documentation:</p>
          <div class="space-y-4">
            <div class="bg-gray-800 rounded-lg p-4">
              <code class="text-green-400">php artisan evolve-api:generate-docs</code>
            </div>
            <p>Access the documentation at:</p>
            <code class="bg-gray-100 px-2 py-1 rounded block">/evolve-api/docs</code>
          </div>
        </section>
    </UContainer>
  </div>
</template>
<script setup lang="ts">
</script>