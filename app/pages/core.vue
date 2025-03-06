<template>
  <div class="max-w-4xl mx-auto">
    <UContainer class="max-w-8xl py-12 space-y-6">
      <h1 class="text-4xl sm:text-6xl lg:text-7xl tracking-tight text-gray-800 font-bold text-center font-display max-w-4xl mx-auto">
        Core Concepts
      </h1>

      <div class="w-full flex justify-center py-4">
          <ButtonLink label="Documentation" size="sm" rounded="md" href="/docs" class="w-full max-w-md" />
      </div>

      <div class="bg-gray-100 p-6 rounded-lg mb-8">
        <ul class="space-y-2">
          <li><a href="#overview" class="text-blue-600 hover:underline">1. Overview</a></li>
          <li><a href="#architecture" class="text-blue-600 hover:underline">2. Architecture</a></li>
          <li><a href="#services" class="text-blue-600 hover:underline">3. Core Services</a></li>
          <li><a href="#models" class="text-blue-600 hover:underline">4. Model Integration</a></li>
          <li><a href="#api" class="text-blue-600 hover:underline">5. API Features</a></li>
          <li><a href="#lifecycle" class="text-blue-600 hover:underline">6. Request Lifecycle</a></li>
          <li><a href="#patterns" class="text-blue-600 hover:underline">7. Design Patterns</a></li>
        </ul>
      </div>

      <section id="overview" class="mb-12">
        <h2 class="text-2xl font-semibold mb-4">1. Overview</h2>
        <p class="text-gray-700 mb-4">
          Shadow is a powerful Laravel package that provides automatic API generation, model discovery, and advanced query capabilities. This guide explains the core concepts and features that make Shadow an essential tool for rapid API development.
        </p>
      </section>

      <section id="architecture" class="mb-12">
        <h2 class="text-2xl font-semibold mb-4">2. Architecture</h2>
        
        <h3 class="text-xl font-semibold mb-4">Component Structure</h3>
        <div class="bg-gray-800 rounded-lg p-4 mb-6">
          <pre class="text-green-400">
shadow/
├── Services/
│   ├── ShadowService.php
│   ├── Registry/
│   └── Response/
├── Http/
│   ├── Controllers/
│   └── Resources/
├── Contracts/
│   ├── Services/
│   └── Query/
└── Support/</pre>
        </div>

        <h3 class="text-xl font-semibold mb-4">Service Container</h3>
        <div class="bg-gray-800 rounded-lg p-4 mb-6">
          <pre class="text-green-400">
// Service resolution
public function __construct(ShadowService $shadow)
{
    $this->shadow = $shadow;
}

// Basic usage
return $shadow->handleIndex('users');</pre>
        </div>
      </section>

      <section id="services" class="mb-12">
        <h2 class="text-2xl font-semibold mb-4">3. Core Services</h2>
        
        <h3 class="text-xl font-semibold mb-4">Model Registry</h3>
        <div class="bg-gray-800 rounded-lg p-4 mb-6">
          <pre class="text-green-400">
// Models are automatically discovered
class User extends Model
{
    use HasShadow;

    protected $shadowConfig = [
        'searchable' => ['name', 'email'],
        'cache' => true
    ];
}</pre>
        </div>

        <h3 class="text-xl font-semibold mb-4">Query Service</h3>
        <div class="bg-gray-800 rounded-lg p-4 mb-6">
          <pre class="text-green-400">
// API query parameters
GET /shadow-api/users?filter[status]=active
GET /shadow-api/users?sort=-created_at
GET /shadow-api/users?include=posts,profile
GET /shadow-api/users?search=john</pre>
        </div>
      </section>

      <section id="models" class="mb-12">
        <h2 class="text-2xl font-semibold mb-4">4. Model Integration</h2>
        
        <h3 class="text-xl font-semibold mb-4">Model Configuration</h3>
        <div class="bg-gray-800 rounded-lg p-4 mb-6">
          <pre class="text-green-400">
class User extends Model
{
    use HasShadow;

    protected $shadowConfig = [
        'searchable' => ['name', 'email'],
        'filterable' => ['status', 'role'],
        'sortable' => ['created_at'],
        'relationships' => [
            'posts' => [
                'fields' => ['title'],
                'constraints' => [
                    'published' => fn($q) => $q->where('status', 'published')
                ]
            ]
        ],
        'validation' => [
            'rules' => [
                'email' => 'required|email|unique:users'
            ]
        ]
    ];
}</pre>
        </div>
      </section>

      <section id="api" class="mb-12">
        <h2 class="text-2xl font-semibold mb-4">5. API Features</h2>
        
        <h3 class="text-xl font-semibold mb-4">Automatic Endpoints</h3>
        <div class="bg-gray-800 rounded-lg p-4 mb-6">
          <pre class="text-green-400">
// Configuration
'auto_generate_api' => true,
'api_route_prefix' => 'shadow-api',
'api_auth_middleware' => ['api']

// Generated endpoints
GET    /shadow-api/{model}
POST   /shadow-api/{model}
GET    /shadow-api/{model}/{id}
PUT    /shadow-api/{model}/{id}
DELETE /shadow-api/{model}/{id}</pre>
        </div>

        <h3 class="text-xl font-semibold mb-4">Response Format</h3>
        <div class="bg-gray-800 rounded-lg p-4 mb-6">
          <pre class="text-green-400">
{
    "success": true,
    "data": [...],
    "meta": {
        "filters": [...],
        "sorts": [...],
        "includes": [...],
        "pagination": {
            "current_page": 1,
            "per_page": 15,
            "total": 100
        }
    }
}</pre>
        </div>
      </section>

      <section id="lifecycle" class="mb-12">
        <h2 class="text-2xl font-semibold mb-4">6. Request Lifecycle</h2>
        
        <h3 class="text-xl font-semibold mb-4">Request Processing</h3>
        <div class="bg-gray-800 rounded-lg p-4 mb-6">
          <pre class="text-green-400">
1. Route matching
2. Middleware processing
3. Model resolution
4. Query parameter parsing
5. Validation
6. Authorization
7. Data retrieval/mutation
8. Response transformation</pre>
        </div>

        <h3 class="text-xl font-semibold mb-4">Error Handling</h3>
        <div class="bg-gray-800 rounded-lg p-4 mb-6">
          <pre class="text-green-400">
{
    "error": true,
    "message": "Validation failed",
    "errors": {
        "email": ["The email field is required"]
    },
    "code": 422
}</pre>
        </div>
      </section>

      <section id="patterns" class="mb-12">
        <h2 class="text-2xl font-semibold mb-4">7. Design Patterns</h2>
        
        <h3 class="text-xl font-semibold mb-4">Service Pattern</h3>
        <div class="bg-gray-800 rounded-lg p-4 mb-6">
          <pre class="text-green-400">
class ShadowService implements ShadowServiceInterface
{
    public function handleIndex(string $model, array $options = []): JsonResponse
    {
        try {
            $modelClass = $this->resolveModelClass($model);
            $data = $this->getModelData($modelClass, $options);
            return $this->responseService->success($data);
        } catch (\Exception $e) {
            return $this->handleException($e);
        }
    }
}</pre>
        </div>

        <h3 class="text-xl font-semibold mb-4">Query Coordinator</h3>
        <div class="bg-gray-800 rounded-lg p-4 mb-6">
          <pre class="text-green-400">
class QueryCoordinator implements QueryCoordinatorInterface
{
    public function getModelData(string $modelClass, array $options = []): array
    {
        $query = $this->buildQuery($modelClass, $options);
        return $this->executeQuery($query, $options);
    }
}</pre>
        </div>
      </section>
    </UContainer>
  </div>
</template>

<script setup lang="ts">
// Component logic can be added here if needed
</script> 