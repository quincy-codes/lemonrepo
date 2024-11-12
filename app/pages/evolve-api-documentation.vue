<template>
  <div class="max-w-xl mx-auto">
    <UContainer class="max-w-4xl py-12 space-y-6">
      <div class="max-w-6xl mx-auto px-4 py-8">
        <h1 class="text-4xl sm:text-6xl lg:text-7xl tracking-tight text-gray-800 font-bold text-center font-display max-w-4xl mx-auto">
          EvolveAPI Documentation
        </h1>
        <ButtonLink label="Core" size="md" rounded="md" href="/evolve-core-documentation" />
        <ButtonLink label="API" size="md" rounded="md" href="/evolve-api-documentation" />
        <ButtonLink label="UI" size="md" rounded="md" href="/evolve-UI-documentation" />

        <h3 class="max-w-xl text-2xl sm:text-2xl font-semibold tracking-tight text-gray-900 font-display mb-4 mt-8">
          Extension for EvolveCORE (Foundation / Prerelease)
        </h3>
        <!-- Table of Contents -->
        <div class="mt-12">
          <h3 class="max-w-xl text-2xl sm:text-2xl font-semibold tracking-tight text-gray-900 font-display mb-4 mt-8">
            Table of Contents
          </h3>
          <nav class="mb-12">
            <ul class="list-none space-y-2">
              <li><a href="#introduction" class="text-blue-600 hover:text-blue-800">Introduction</a></li>
              <li><a href="#installation" class="text-blue-600 hover:text-blue-800">Installation</a></li>
              <li><a href="#configuration" class="text-blue-600 hover:text-blue-800">Configuration</a></li>
              <li><a href="#model-setup" class="text-blue-600 hover:text-blue-800">Model Setup</a></li>
              <li><a href="#api-features" class="text-blue-600 hover:text-blue-800">API Features</a></li>
              <li><a href="#endpoints" class="text-blue-600 hover:text-blue-800">API Endpoints</a></li>
              <li><a href="#documentation" class="text-blue-600 hover:text-blue-800">Documentation System</a></li>
              <li><a href="#hooks" class="text-blue-600 hover:text-blue-800">Custom Hooks</a></li>
              <li><a href="#relations" class="text-blue-600 hover:text-blue-800">Handling Relations</a></li>
            </ul>
          </nav>
        </div>

        <!-- Introduction Section -->
        <section id="introduction" class="mb-16">
          <h2 class="text-center mx-auto max-w-xl text-2xl sm:text-2xl font-semibold tracking-tight text-gray-900 font-display mb-4 mt-24">
            Introduction
          </h2>
          <div class="prose max-w-none">
            <p>EvolveAPI is a powerful extension to EvolveCORE that automatically generates RESTful API endpoints for your Laravel models. It provides:</p>
            <ul>
              <li>Automatic REST API generation for Evolve-enabled models</li>
              <li>Built-in Swagger documentation</li>
              <li>Automatic relationship handling</li>
              <li>Filtering and sorting capabilities</li>
              <li>Custom hooks for business logic</li>
              <li>Transaction management</li>
              <li>Event dispatching</li>
            </ul>
          </div>
        </section>

        <!-- Installation Section -->
        <section id="installation" class="mb-16">
          <h2 class="text-center mx-auto max-w-xl text-2xl sm:text-2xl font-semibold tracking-tight text-gray-900 font-display mb-4 mt-24">
            Installation
          </h2>
          <div class="prose max-w-none">
            <h4>1. Install via Composer</h4>
            <pre class="bg-gray-100 p-4 rounded"><code>composer require thinkneverland/evolve-api</code></pre>

            <h4 class="mt-6">2. Publish Assets</h4>
            <pre class="bg-gray-100 p-4 rounded"><code>php artisan vendor:publish --provider="Thinkneverland\Evolve\Api\Providers\EvolveApiServiceProvider"</code></pre>

            <h4 class="mt-6">3. Requirements</h4>
            <ul>
              <li>PHP ^7.4|^8.0</li>
              <li>Laravel ^8.0|^9.0</li>
              <li>evolve/core package</li>
            </ul>
          </div>
        </section>

        <!-- Configuration Section -->
        <section id="configuration" class="mb-16">
          <h2 class="text-center mx-auto max-w-xl text-2xl sm:text-2xl font-semibold tracking-tight text-gray-900 font-display mb-4 mt-24">
            Configuration
          </h2>
          <div class="prose max-w-none">
            <p>The package can be configured through the <code>config/evolve-api.php</code> file:</p>
            <pre class="bg-gray-100 p-4 rounded"><code>return [
    /*
    |--------------------------------------------------------------------------
    | EvolveAPI Route Prefix
    |--------------------------------------------------------------------------
    |
    | This value determines the route prefix for all EvolveAPI routes and the
    | Swagger UI documentation.
    |
    */

    'route_prefix' => 'evolve-api',
];</code></pre>
          </div>
        </section>

        <!-- Model Setup Section -->
        <section id="model-setup" class="mb-16">
          <h2 class="text-center mx-auto max-w-xl text-2xl sm:text-2xl font-semibold tracking-tight text-gray-900 font-display mb-4 mt-24">
            Model Setup
          </h2>
          <div class="prose max-w-none">
            <p>To make your models work with EvolveAPI:</p>
            <pre class="bg-gray-100 p-4 rounded"><code>use Thinkneverland\Evolve\Core\Contracts\EvolveModelInterface;
use Thinkneverland\Evolve\Core\Traits\SortableFilterableTrait;

class YourModel extends Model implements EvolveModelInterface
{
    use SortableFilterableTrait;

    // Define which fields should be unique when avoiding duplicates
    public static function uniqueFields(): array
    {
        return ['email', 'username'];
    }

    // Define validation rules
    public static function getValidationRules(string $operation, $model = null): array
    {
        return [
            'name' => 'required|string|max:255',
            'email' => 'required|email|unique:users,email'
        ];
    }

    // Control whether this model should be exposed via API
    public static function shouldEvolve(): bool
    {
        return true;
    }
}</code></pre>
          </div>
        </section>

        <!-- API Features Section -->
        <section id="api-features" class="mb-16">
          <h2 class="text-center mx-auto max-w-xl text-2xl sm:text-2xl font-semibold tracking-tight text-gray-900 font-display mb-4 mt-24">
            API Features
          </h2>
          <div class="prose max-w-none">
            <h4>Filtering</h4>
            <pre class="bg-gray-100 p-4 rounded"><code>GET /evolve-api/users?filter[name]=John&filter[age][gt]=25</code></pre>

            <h4 class="mt-6">Sorting</h4>
            <pre class="bg-gray-100 p-4 rounded"><code>GET /evolve-api/users?sort=-created_at,name</code></pre>

            <h4 class="mt-6">Pagination</h4>
            <pre class="bg-gray-100 p-4 rounded"><code>GET /evolve-api/users?per_page=20</code></pre>

            <h4 class="mt-6">Duplicate Prevention</h4>
            <pre class="bg-gray-100 p-4 rounded"><code>POST /evolve-api/users?avoid_duplicates=true</code></pre>
          </div>
        </section>
        <!-- API Endpoints Section -->
        <section id="endpoints" class="mb-16">
          <h2 class="text-center mx-auto max-w-xl text-2xl sm:text-2xl font-semibold tracking-tight text-gray-900 font-display mb-4 mt-24">
            API Endpoints
          </h2>
          <div class="prose max-w-none">
            <table class="min-w-full border-collapse border border-gray-300">
              <thead class="bg-gray-50">
              <tr>
                <th class="px-6 py-3 border border-gray-300 text-left">Method</th>
                <th class="px-6 py-3 border border-gray-300 text-left">Endpoint</th>
                <th class="px-6 py-3 border border-gray-300 text-left">Description</th>
                <th class="px-6 py-3 border border-gray-300 text-left">Features</th>
              </tr>
              </thead>
              <tbody class="bg-white">
              <tr>
                <td class="px-6 py-4 border border-gray-300">GET</td>
                <td class="px-6 py-4 border border-gray-300">/evolve/{model}</td>
                <td class="px-6 py-4 border border-gray-300">List Resources</td>
                <td class="px-6 py-4 border border-gray-300">
                  <ul class="list-disc pl-4">
                    <li>Filtering</li>
                    <li>Sorting</li>
                    <li>Pagination</li>
                  </ul>
                </td>
              </tr>
              <tr>
                <td class="px-6 py-4 border border-gray-300">POST</td>
                <td class="px-6 py-4 border border-gray-300">/evolve/{model}</td>
                <td class="px-6 py-4 border border-gray-300">Create Resource</td>
                <td class="px-6 py-4 border border-gray-300">
                  <ul class="list-disc pl-4">
                    <li>Validation</li>
                    <li>Relationships</li>
                    <li>Duplicate Prevention</li>
                  </ul>
                </td>
              </tr>
              <tr>
                <td class="px-6 py-4 border border-gray-300">GET</td>
                <td class="px-6 py-4 border border-gray-300">/evolve/{model}/{id}</td>
                <td class="px-6 py-4 border border-gray-300">Show Resource</td>
                <td class="px-6 py-4 border border-gray-300">
                  <ul class="list-disc pl-4">
                    <li>Relationships</li>
                    <li>Custom Loading</li>
                  </ul>
                </td>
              </tr>
              <tr>
                <td class="px-6 py-4 border border-gray-300">PUT</td>
                <td class="px-6 py-4 border border-gray-300">/evolve/{model}/{id}</td>
                <td class="px-6 py-4 border border-gray-300">Update Resource</td>
                <td class="px-6 py-4 border border-gray-300">
                  <ul class="list-disc pl-4">
                    <li>Validation</li>
                    <li>Relationships</li>
                    <li>Hooks</li>
                  </ul>
                </td>
              </tr>
              <tr>
                <td class="px-6 py-4 border border-gray-300">DELETE</td>
                <td class="px-6 py-4 border border-gray-300">/evolve/{model}/{id}</td>
                <td class="px-6 py-4 border border-gray-300">Delete Resource</td>
                <td class="px-6 py-4 border border-gray-300">
                  <ul class="list-disc pl-4">
                    <li>Soft Deletes</li>
                    <li>Hooks</li>
                    <li>Events</li>
                  </ul>
                </td>
              </tr>
              </tbody>
            </table>
          </div>
        </section>

        <!-- Documentation System Section -->
        <section id="documentation" class="mb-16">
          <h2 class="text-center mx-auto max-w-xl text-2xl sm:text-2xl font-semibold tracking-tight text-gray-900 font-display mb-4 mt-24">
            Documentation System
          </h2>
          <div class="prose max-w-none">
            <h4>Generate Documentation</h4>
            <pre class="bg-gray-100 p-4 rounded"><code>php artisan evolve-api:generate-docs</code></pre>

            <h4 class="mt-6">Access Documentation</h4>
            <ul class="list-disc pl-4">
              <li>Swagger UI: <code>/docs</code></li>
              <li>OpenAPI JSON: <code>/docs.json</code></li>
            </ul>

            <h4 class="mt-6">Documentation Generation Process</h4>
            <pre class="bg-gray-100 p-4 rounded"><code>// The documentation generator:
1. Scans your Models directory
2. Identifies Evolve-enabled models
3. Extracts model properties and relationships
4. Generates OpenAPI specification
5. Creates interactive Swagger UI</code></pre>
          </div>
        </section>

        <!-- Custom Hooks Section -->
        <section id="hooks" class="mb-16">
          <h2 class="text-center mx-auto max-w-xl text-2xl sm:text-2xl font-semibold tracking-tight text-gray-900 font-display mb-4 mt-24">
            Custom Hooks
          </h2>
          <div class="prose max-w-none">
            <p>Create a controller in <code>App\Http\Controllers\Api</code> that matches your model name to implement hooks:</p>
            <pre class="bg-gray-100 p-4 rounded"><code>namespace App\Http\Controllers\Api;

class UserController extends Controller
{
    public function beforeUpdate(Request $request, &$data, $model)
    {
        // Modify data before update
        $data['modified_at'] = now();
    }

    public function afterUpdate(Request $request, $model)
    {
        // Perform actions after update
        Cache::tags('users')->flush();
    }

    public function beforeDelete(Request $request, $model)
    {
        // Prevent deletion if conditions aren't met
        if ($model->hasActiveSubscriptions()) {
            return response()->json([
                'error' => 'Cannot delete user with active subscriptions'
            ], 422);
        }
    }

    public function afterDelete(Request $request, $model)
    {
        // Cleanup after deletion
        Storage::delete("avatars/{$model->id}");
    }
}</code></pre>
          </div>
        </section>

        <!-- Relations Section -->
        <section id="relations" class="mb-16">
          <h2 class="text-center mx-auto max-w-xl text-2xl sm:text-2xl font-semibold tracking-tight text-gray-900 font-display mb-4 mt-24">
            Handling Relations
          </h2>
          <div class="prose max-w-none">
            <h4>Creating with Relations</h4>
            <pre class="bg-gray-100 p-4 rounded"><code>// POST /evolve-api/users
{
    "name": "John Doe",
    "email": "john@example.com",
    "posts": [
        {
            "title": "First Post",
            "content": "Content here..."
        }
    ],
    "profile": {
        "bio": "Full-stack developer",
        "twitter": "@johndoe"
    }
}</code></pre>

            <h4 class="mt-6">Relation Features</h4>
            <ul class="list-disc pl-4">
              <li>Automatic relation detection and handling</li>
              <li>Nested creation and updates</li>
              <li>Support for all Laravel relation types</li>
              <li>Duplicate prevention across relations</li>
              <li>Transaction safety</li>
            </ul>

            <h4 class="mt-6">Example with Multiple Relation Types</h4>
            <pre class="bg-gray-100 p-4 rounded"><code>class User extends Model implements EvolveModelInterface
{
    use SortableFilterableTrait;

    public function posts()
    {
        return $this->hasMany(Post::class);
    }

    public function profile()
    {
        return $this->hasOne(Profile::class);
    }

    public function roles()
    {
        return $this->belongsToMany(Role::class);
    }

    public static function getAllRelations(): array
    {
        return ['posts', 'profile', 'roles'];
    }
}</code></pre>
          </div>
        </section>

        <!-- Events Section -->
        <section id="events" class="mb-16">
          <h2 class="text-center mx-auto max-w-xl text-2xl sm:text-2xl font-semibold tracking-tight text-gray-900 font-display mb-4 mt-24">
            Events
          </h2>
          <div class="prose max-w-none">
            <p>EvolveAPI dispatches the following events:</p>
            <ul class="list-disc pl-4">
              <li><code>EvolveModelCreated</code> - When a model is created</li>
              <li><code>EvolveModelUpdated</code> - When a model is updated</li>
              <li><code>EvolveModelDeleted</code> - When a model is deleted</li>
            </ul>

            <h4 class="mt-6">Listening for Events</h4>
            <pre class="bg-gray-100 p-4 rounded"><code>protected $listen = [
    'Thinkneverland\Evolve\Core\Events\EvolveModelCreated' => [
        'App\Listeners\HandleModelCreated',
    ],
    'Thinkneverland\Evolve\Core\Events\EvolveModelUpdated' => [
        'App\Listeners\HandleModelUpdated',
    ],
    'Thinkneverland\Evolve\Core\Events\EvolveModelDeleted' => [
        'App\Listeners\HandleModelDeleted',
    ],
];</code></pre>
          </div>
        </section>
      </div>
    </UContainer>
  </div>
</template>
<script setup lang="ts">
import ButtonLink from "../components/ButtonLink.vue";
</script>