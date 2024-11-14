<template>
  <div class="max-w-xl mx-auto">
    <UContainer class="max-w-8xl py-12 space-y-6">
        <h1 class="text-4xl sm:text-6xl lg:text-7xl tracking-tight text-gray-800 font-bold text-center font-display max-w-4xl mx-auto">
          Evolve Documentation
        </h1>

        <div class="w-full columns-2 py-4" >
          <ButtonLink href="/evolve-core-documentation" />
          <ButtonLink href="/evolve-api-documentation" />
          <ButtonLink href="/evolve-UI-documentation" />
        </div>

        <h3 class="max-w-xl text-2xl sm:text-2xl font-semibold tracking-tight text-gray-900 font-display mb-4 mt-8">
          EvolveUI (Extension / Prerelease)
        </h3>

        <div class="grid grid-cols-1 md:grid-cols-3 gap-6 mb-16">
          <a href="#getting-started" class="block p-6 bg-white rounded-lg shadow hover:shadow-lg transition-shadow">
            <h3 class="text-xl font-semibold mb-2">Getting Started</h3>
            <p class="text-gray-600">Installation, configuration and basic setup</p>
          </a>
          <a href="#api-reference" class="block p-6 bg-white rounded-lg shadow hover:shadow-lg transition-shadow">
            <h3 class="text-xl font-semibold mb-2">API Reference</h3>
            <p class="text-gray-600">Endpoints, parameters and response formats</p>
          </a>
          <a href="#advanced" class="block p-6 bg-white rounded-lg shadow hover:shadow-lg transition-shadow">
            <h3 class="text-xl font-semibold mb-2">Advanced Usage</h3>
            <p class="text-gray-600">Hooks, events and complex operations</p>
          </a>
        </div>

        <div class="max-w-4xl mx-auto">
          <aside class="bg-gray-100 rounded-lg p-6 mb-12">
            <h2 class="text-xl font-semibold mb-4">On This Page</h2>
            <nav>
              <ul class="space-y-2">
                <li>
                  <a href="#getting-started" class="text-blue-600 hover:underline">1. Getting Started</a>
                  <ul class="ml-4 mt-2 space-y-2">
                    <li><a href="#requirements" class="text-blue-600 hover:underline">1.1 Requirements</a></li>
                    <li><a href="#installation" class="text-blue-600 hover:underline">1.2 Installation</a></li>
                    <li><a href="#configuration" class="text-blue-600 hover:underline">1.3 Configuration</a></li>
                  </ul>
                </li>
                <li>
                  <a href="#model-setup" class="text-blue-600 hover:underline">2. Model Setup</a>
                  <ul class="ml-4 mt-2 space-y-2">
                    <li><a href="#basic-model" class="text-blue-600 hover:underline">2.1 Basic Model</a></li>
                    <li><a href="#advanced-model" class="text-blue-600 hover:underline">2.2 Advanced Features</a></li>
                  </ul>
                </li>
                <li>
                  <a href="#api-reference" class="text-blue-600 hover:underline">3. API Reference</a>
                  <ul class="ml-4 mt-2 space-y-2">
                    <li><a href="#endpoints" class="text-blue-600 hover:underline">3.1 Endpoints</a></li>
                    <li><a href="#parameters" class="text-blue-600 hover:underline">3.2 Parameters</a></li>
                    <li><a href="#responses" class="text-blue-600 hover:underline">3.3 Responses</a></li>
                  </ul>
                </li>
                <li>
                  <a href="#advanced" class="text-blue-600 hover:underline">4. Advanced Usage</a>
                  <ul class="ml-4 mt-2 space-y-2">
                    <li><a href="#hooks" class="text-blue-600 hover:underline">4.1 Hook Methods</a></li>
                    <li><a href="#events" class="text-blue-600 hover:underline">4.2 Events</a></li>
                    <li><a href="#examples" class="text-blue-600 hover:underline">4.3 Complex Examples</a></li>
                  </ul>
                </li>
              </ul>
            </nav>
          </aside>

          <section id="getting-started" class="mb-16">
            <h2 class="text-2xl sm:text-2xl font-semibold tracking-tight text-gray-900 font-display mb-8">1. Getting Started</h2>

            <div id="requirements" class="mb-12">
              <h3 class="text-xl font-semibold mb-4">1.1 Requirements</h3>
              <div class="prose max-w-none">
                <ul class="list-disc ml-6">
                  <li>PHP ^7.4|^8.0</li>
                  <li>Laravel ^8.0|^9.0</li>
                  <li>thinkneverland/evolve-core</li>
                  <li>darkaonline/l5-swagger ^8.0</li>
                </ul>
              </div>
            </div>

            <div id="installation" class="mb-12">
              <h3 class="text-xl font-semibold mb-4">1.2 Installation</h3>
              <div class="bg-gray-800 rounded-lg p-4 mb-4 overflow-x-auto">
                <code class="text-green-400">composer require thinkneverland/evolve-api</code>
              </div>
              <p class="text-gray-600 mb-4">The service provider will be automatically registered. Publish the package assets:</p>
              <div class="bg-gray-800 rounded-lg p-4 overflow-x-auto">
                <code class="text-green-400">php artisan vendor:publish --provider="Thinkneverland\Evolve\Api\Providers\EvolveApiServiceProvider"</code>
              </div>
            </div>

            <div id="configuration" class="mb-12">
              <h3 class="text-xl font-semibold mb-4">1.3 Configuration</h3>
              <p class="text-gray-600 mb-4">Configure in <code class="bg-gray-100 px-2 py-1 rounded">config/evolve-api.php</code>:</p>
              <div class="bg-gray-800 rounded-lg p-4 overflow-x-auto">
          <pre class="text-green-400">
return [
    'route_prefix' => 'evolve-api',    // API route prefix
    'middleware' => ['api'],           // Add custom middleware
    'avoid_duplicates' => false,       // Global duplicate prevention
];
          </pre>
              </div>
            </div>
          </section>
        </div>
      <section id="model-setup" class="mb-16">
        <h2 class="text-2xl sm:text-2xl font-semibold tracking-tight text-gray-900 font-display mb-8">2. Model Setup</h2>

        <div id="basic-model" class="mb-12">
          <h3 class="text-xl font-semibold mb-4">2.1 Basic Model</h3>
          <p class="text-gray-600 mb-4">Set up your model with the minimum required configuration:</p>
          <div class="bg-gray-800 rounded-lg p-4 overflow-x-auto">
          <pre class="text-green-400">
namespace App\Models;

use Illuminate\Database\Eloquent\Model;
use Thinkneverland\Evolve\Core\Traits\SortableFilterableTrait;

class Product extends Model
{
    use SortableFilterableTrait;

    protected $fillable = [
        'name',
        'price',
        'description'
    ];

    protected $filterable = [
        'name',
        'price'
    ];

    protected $sortable = [
        'name',
        'price',
        'created_at'
    ];
}
          </pre>
          </div>
        </div>

        <div id="advanced-model" class="mb-12">
          <h3 class="text-xl font-semibold mb-4">2.2 Advanced Features</h3>
          <p class="text-gray-600 mb-4">Enhance your model with advanced features:</p>
          <div class="bg-gray-800 rounded-lg p-4 overflow-x-auto">
          <pre class="text-green-400">
class Product extends Model
{
    use SortableFilterableTrait;

    protected $perPage = 25;

    // Define unique fields for duplicate prevention
    public static function uniqueFields()
    {
        return ['sku', 'name'];
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

    // Define relations
    public function category()
    {
        return $this->belongsTo(Category::class);
    }

    public function tags()
    {
        return $this->belongsToMany(Tag::class);
    }
}
          </pre>
          </div>
        </div>
      </section>

      <section id="api-reference" class="mb-16">
        <h2 class="text-2xl sm:text-2xl font-semibold tracking-tight text-gray-900 font-display mb-8">3. API Reference</h2>

        <div id="endpoints" class="mb-12">
          <h3 class="text-xl font-semibold mb-4">3.1 Endpoints</h3>
          <div class="bg-white rounded-lg shadow overflow-hidden">
            <table class="min-w-full">
              <thead class="bg-gray-50">
              <tr>
                <th class="px-6 py-3 text-left text-xs font-medium text-gray-500 uppercase tracking-wider">Method</th>
                <th class="px-6 py-3 text-left text-xs font-medium text-gray-500 uppercase tracking-wider">Endpoint</th>
                <th class="px-6 py-3 text-left text-xs font-medium text-gray-500 uppercase tracking-wider">Description</th>
              </tr>
              </thead>
              <tbody class="bg-white divide-y divide-gray-200">
              <tr>
                <td class="px-6 py-4"><code class="bg-green-100 text-green-800 px-2 py-1 rounded">GET</code></td>
                <td class="px-6 py-4 font-mono text-sm">/evolve-api/{model}</td>
                <td class="px-6 py-4">List resources</td>
              </tr>
              <tr>
                <td class="px-6 py-4"><code class="bg-blue-100 text-blue-800 px-2 py-1 rounded">POST</code></td>
                <td class="px-6 py-4 font-mono text-sm">/evolve-api/{model}</td>
                <td class="px-6 py-4">Create resource</td>
              </tr>
              <tr>
                <td class="px-6 py-4"><code class="bg-green-100 text-green-800 px-2 py-1 rounded">GET</code></td>
                <td class="px-6 py-4 font-mono text-sm">/evolve-api/{model}/{id}</td>
                <td class="px-6 py-4">Get single resource</td>
              </tr>
              <tr>
                <td class="px-6 py-4"><code class="bg-yellow-100 text-yellow-800 px-2 py-1 rounded">PUT</code></td>
                <td class="px-6 py-4 font-mono text-sm">/evolve-api/{model}/{id}</td>
                <td class="px-6 py-4">Update resource</td>
              </tr>
              <tr>
                <td class="px-6 py-4"><code class="bg-red-100 text-red-800 px-2 py-1 rounded">DELETE</code></td>
                <td class="px-6 py-4 font-mono text-sm">/evolve-api/{model}/{id}</td>
                <td class="px-6 py-4">Delete resource</td>
              </tr>
              </tbody>
            </table>
          </div>
        </div>

        <div id="parameters" class="mb-12">
          <h3 class="text-xl font-semibold mb-4">3.2 Parameters</h3>

          <h4 class="font-semibold text-gray-800 mb-2">Filtering</h4>
          <div class="bg-gray-800 rounded-lg p-4 mb-6 overflow-x-auto">
          <pre class="text-green-400">
# Basic filter
GET /evolve-api/products?filter[price]=100

# Operator filter
GET /evolve-api/products?filter[price][gt]=100

# Multiple filters
GET /evolve-api/products?filter[price][gt]=100&filter[category]=electronics

# Relation filter
GET /evolve-api/products?filter[category.name]=Electronics
          </pre>
          </div>

          <h4 class="font-semibold text-gray-800 mb-2">Sorting</h4>
          <div class="bg-gray-800 rounded-lg p-4 mb-6 overflow-x-auto">
          <pre class="text-green-400">
# Single sort
GET /evolve-api/products?sort=price

# Descending sort
GET /evolve-api/products?sort=-price

# Multiple sort
GET /evolve-api/products?sort=-price,name
          </pre>
          </div>

          <h4 class="font-semibold text-gray-800 mb-2">Pagination</h4>
          <div class="bg-gray-800 rounded-lg p-4 overflow-x-auto">
          <pre class="text-green-400">
GET /evolve-api/products?page=1&per_page=25
          </pre>
          </div>
        </div>

        <div id="responses" class="mb-12">
          <h3 class="text-xl font-semibold mb-4">3.3 Responses</h3>
          <p class="text-gray-600 mb-4">Example successful response:</p>
          <div class="bg-gray-800 rounded-lg p-4 overflow-x-auto">
          <pre class="text-green-400">
{
    "success": true,
    "data": {
        "id": 1,
        "name": "Example Product",
        "price": 99.99,
        "created_at": "2024-11-14T10:00:00.000000Z",
        "updated_at": "2024-11-14T10:00:00.000000Z"
    },
    "message": "Resource created successfully"
}
          </pre>
          </div>
        </div>
      </section>

      <section id="advanced" class="mb-16">
        <h2 class="text-2xl sm:text-2xl font-semibold tracking-tight text-gray-900 font-display mb-8">4. Advanced Usage</h2>

        <div id="hooks" class="mb-12">
          <h3 class="text-xl font-semibold mb-4">4.1 Hook Methods</h3>
          <p class="text-gray-600 mb-4">Create a controller to implement custom hooks:</p>
          <div class="bg-gray-800 rounded-lg p-4 overflow-x-auto">
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
        if ($model->has_active_orders) {
            return response()->json([
                'error' => 'Cannot delete product with active orders'
            ], 422);
        }
    }
}
          </pre>
          </div>
        </div>

        <div id="events" class="mb-12">
          <h3 class="text-xl font-semibold mb-4">4.2 Events</h3>
          <div class="prose max-w-none">
            <ul class="list-disc ml-6">
              <li><code class="bg-gray-100 px-2 py-1 rounded">EvolveModelCreated</code> - Dispatched after model creation</li>
              <li><code class="bg-gray-100 px-2 py-1 rounded">EvolveModelUpdated</code> - Dispatched after model update</li>
              <li><code class="bg-gray-100 px-2 py-1 rounded">EvolveModelDeleted</code> - Dispatched after model deletion</li>
            </ul>
          </div>
        </div>

        <div id="examples" class="mb-12">
          <h3 class="text-xl font-semibold mb-4">4.3 Complex Examples</h3>

          <h4 class="font-semibold text-gray-800 mb-2">Create with Relations</h4>
          <div class="bg-gray-800 rounded-lg p-4 mb-6 overflow-x-auto">
          <pre class="text-green-400">
POST /evolve-api/products
{
    "name": "Premium Laptop",
    "price": 1299.99,
    "category": {
        "name": "Electronics",
        "slug": "electronics"
    },
    "tags": [
        {"name": "Featured"},
        {"name": "New Arrival"}
    ],
    "specifications": [
        {
            "name": "RAM",
            "value": "16GB"
        },
        {
            "name": "Storage",
            "value": "512GB SSD"
        }
    ]
}
          </pre>
          </div>

          <h4 class="font-semibold text-gray-800 mb-2">Update with Duplicate Prevention</h4>
          <div class="bg-gray-800 rounded-lg p-4 overflow-x-auto">
          <pre class="text-green-400">
PUT /evolve-api/products/1?avoid_duplicates=true
{
    "name": "Updated Product Name",
    "variants": [
        {
            "sku": "PROD-1-VAR-1",
            "price": 99.99,
            "attributes": [
                {
                    "name": "Color",
                    "value": "Blue"
                }
            ]
        }
    ]
}
          </pre>
          </div>
        </div>
      </section>

    </UContainer>
  </div>
</template>
<script setup lang="ts">
</script>