<template>
  <div class="max-w-4xl mx-auto">
    <UContainer class="max-w-8xl py-12 space-y-6">
      <h1 class="text-4xl sm:text-6xl lg:text-7xl tracking-tight text-gray-800 font-bold text-center font-display max-w-4xl mx-auto">
        Core Concepts
      </h1>

      <div class="w-full py-4">
          <ButtonLink label="Documentation Home" size="md" rounded="md" href="/docs" />
      </div>

      <div class="bg-gray-100 p-6 rounded-lg mb-8">
        <ul class="space-y-2">
          <li><a href="#overview" class="text-blue-600 hover:underline">1. Overview</a></li>
          <li><a href="#architecture" class="text-blue-600 hover:underline">2. Architecture</a></li>
          <li><a href="#services" class="text-blue-600 hover:underline">3. Core Services</a></li>
          <li><a href="#models" class="text-blue-600 hover:underline">4. Model Integration</a></li>
          <li><a href="#lifecycle" class="text-blue-600 hover:underline">5. Request Lifecycle</a></li>
          <li><a href="#patterns" class="text-blue-600 hover:underline">6. Design Patterns</a></li>
        </ul>
      </div>

      <section id="overview" class="mb-12">
        <h2 class="text-2xl font-semibold mb-4">1. Overview</h2>
        <p class="text-gray-700 mb-4">
          Evolve is built on a foundation of robust core concepts that enable powerful API development. This guide explains the fundamental principles and components that make up the Evolve framework.
        </p>
      </section>

      <section id="architecture" class="mb-12">
        <h2 class="text-2xl font-semibold mb-4">2. Architecture</h2>
        
        <h3 class="text-xl font-semibold mb-4">Component Structure</h3>
        <div class="bg-gray-800 rounded-lg p-4 mb-6">
          <pre class="text-green-400">
evolve/
├── Core/
│   ├── Services/
│   ├── Models/
│   └── Contracts/
├── Http/
│   ├── Controllers/
│   ├── Middleware/
│   └── Resources/
├── Database/
│   ├── Repositories/
│   └── Factories/
└── Support/</pre>
        </div>

        <h3 class="text-xl font-semibold mb-4">Service Container</h3>
        <div class="bg-gray-800 rounded-lg p-4 mb-6">
          <pre class="text-green-400">
// Service registration
$this->app->singleton('evolve', function ($app) {
    return new EvolveService($app);
});

// Service resolution
$evolve = app('evolve');
$evolve->model(User::class)->find(1);</pre>
        </div>
      </section>

      <section id="services" class="mb-12">
        <h2 class="text-2xl font-semibold mb-4">3. Core Services</h2>
        
        <h3 class="text-xl font-semibold mb-4">Model Registry</h3>
        <div class="bg-gray-800 rounded-lg p-4 mb-6">
          <pre class="text-green-400">
// Register model
ModelRegistry::register(User::class, [
    'resource' => UserResource::class,
    'policy' => UserPolicy::class,
]);

// Access registered model
$model = ModelRegistry::get(User::class);
$resource = $model->resource;</pre>
        </div>

        <h3 class="text-xl font-semibold mb-4">Query Builder</h3>
        <div class="bg-gray-800 rounded-lg p-4 mb-6">
          <pre class="text-green-400">
// Build complex queries
$users = $evolve->model(User::class)
    ->filter(['status' => 'active'])
    ->sort('-created_at')
    ->include(['posts', 'profile'])
    ->paginate();

// Custom query methods
$posts = $evolve->model(Post::class)
    ->withScope('published')
    ->withCount('comments')
    ->get();</pre>
        </div>
      </section>

      <section id="models" class="mb-12">
        <h2 class="text-2xl font-semibold mb-4">4. Model Integration</h2>
        
        <h3 class="text-xl font-semibold mb-4">Model Configuration</h3>
        <div class="bg-gray-800 rounded-lg p-4 mb-6">
          <pre class="text-green-400">
class User extends Model
{
    use HasEvolve;

    protected $evolveConfig = [
        'resource' => UserResource::class,
        'policy' => UserPolicy::class,
        'validation' => [
            'rules' => [
                'email' => 'required|email|unique:users',
            ],
        ],
        'search' => [
            'columns' => ['name', 'email'],
        ],
    ];
}</pre>
        </div>

        <h3 class="text-xl font-semibold mb-4">Model Events</h3>
        <div class="bg-gray-800 rounded-lg p-4 mb-6">
          <pre class="text-green-400">
protected static function booted()
{
    static::created(function ($model) {
        event(new ModelCreated($model));
    });

    static::updated(function ($model) {
        Cache::tags($model->getCacheTags())->flush();
    });
}</pre>
        </div>
      </section>

      <section id="lifecycle" class="mb-12">
        <h2 class="text-2xl font-semibold mb-4">5. Request Lifecycle</h2>
        
        <h3 class="text-xl font-semibold mb-4">Request Processing</h3>
        <div class="bg-gray-800 rounded-lg p-4 mb-6">
          <pre class="text-green-400">
// Request lifecycle
1. Route matching
2. Middleware processing
3. Authentication
4. Authorization
5. Input validation
6. Query building
7. Model operations
8. Resource transformation
9. Response formatting</pre>
        </div>

        <h3 class="text-xl font-semibold mb-4">Middleware Pipeline</h3>
        <div class="bg-gray-800 rounded-lg p-4 mb-6">
          <pre class="text-green-400">
protected $middleware = [
    \App\Http\Middleware\TrustProxies::class,
    \ThinkNeverland\Evolve\Http\Middleware\HandleCors::class,
    \ThinkNeverland\Evolve\Http\Middleware\AuthenticateRequest::class,
    \ThinkNeverland\Evolve\Http\Middleware\ValidateRequest::class,
];</pre>
        </div>
      </section>

      <section id="patterns" class="mb-12">
        <h2 class="text-2xl font-semibold mb-4">6. Design Patterns</h2>
        
        <h3 class="text-xl font-semibold mb-4">Repository Pattern</h3>
        <div class="bg-gray-800 rounded-lg p-4 mb-6">
          <pre class="text-green-400">
class UserRepository implements Repository
{
    public function find($id)
    {
        return User::findOrFail($id);
    }

    public function create(array $data)
    {
        return User::create($data);
    }

    public function update($id, array $data)
    {
        $user = $this->find($id);
        $user->update($data);
        return $user;
    }
}</pre>
        </div>

        <h3 class="text-xl font-semibold mb-4">Service Layer</h3>
        <div class="bg-gray-800 rounded-lg p-4 mb-6">
          <pre class="text-green-400">
class UserService
{
    protected $repository;

    public function __construct(UserRepository $repository)
    {
        $this->repository = $repository;
    }

    public function createUser(array $data)
    {
        // Business logic
        $user = $this->repository->create($data);
        event(new UserCreated($user));
        return $user;
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