<template>
  <div class="w-full">
    <UContainer class="py-12 space-y-6">
      <h1 class="text-4xl sm:text-6xl lg:text-7xl tracking-tight text-gray-800 font-bold text-center font-display mx-auto">
        Database Guide
      </h1>

      <div class="w-full columns-2 py-4">
          <ButtonLink label="Documentation Home" size="md" rounded="md" href="/docs" />
      </div>

      <div class="bg-gray-100 p-6 rounded-lg mb-8">
        <ul class="space-y-2">
          <li><a href="#overview" class="text-blue-600 hover:underline">1. Overview</a></li>
          <li><a href="#model-setup" class="text-blue-600 hover:underline">2. Model Setup</a></li>
          <li><a href="#relationships" class="text-blue-600 hover:underline">3. Relationships</a></li>
          <li><a href="#migrations" class="text-blue-600 hover:underline">4. Migrations</a></li>
          <li><a href="#querying" class="text-blue-600 hover:underline">5. Querying</a></li>
          <li><a href="#optimization" class="text-blue-600 hover:underline">6. Optimization</a></li>
        </ul>
      </div>

      <section id="overview" class="mb-12">
        <h2 class="text-2xl font-semibold mb-4">1. Overview</h2>
        <p class="text-gray-700 mb-4">
          Evolve extends Laravel's database functionality with additional features for API-focused applications, including advanced querying, relationship handling, and performance optimizations.
        </p>
      </section>

      <section id="model-setup" class="mb-12">
        <h2 class="text-2xl font-semibold mb-4">2. Model Setup</h2>
        
        <h3 class="text-xl font-semibold mb-4">Basic Model Configuration</h3>
        <div class="bg-gray-800 rounded-lg p-4 mb-6">
          <pre class="text-green-400">
use ThinkNeverland\Evolve\Traits\HasEvolve;

&lt;?php

class User extends Model
{
    use HasEvolve;

    protected $fillable = [
        'name', 'email', 'password',
    ];

    protected $hidden = [
        'password', 'remember_token',
    ];

    protected $casts = [
        'email_verified_at' => 'datetime',
        'settings' => 'array',
    ];
}</pre>
        </div>

        <h3 class="text-xl font-semibold mb-4">Model Attributes</h3>
        <div class="bg-gray-800 rounded-lg p-4 mb-6">
          <pre class="text-green-400">
protected $evolveConfig = [
    'attributes' => [
        'computed' => [
            'full_name' => fn() => $this->first_name . ' ' . $this->last_name,
        ],
        'virtual' => [
            'age' => fn() => Carbon::parse($this->birth_date)->age,
        ],
        'protected' => [
            'ssn', 'password',
        ],
    ],
];</pre>
        </div>
      </section>

      <section id="relationships" class="mb-12">
        <h2 class="text-2xl font-semibold mb-4">3. Relationships</h2>
        
        <h3 class="text-xl font-semibold mb-4">Defining Relationships</h3>
        <div class="bg-gray-800 rounded-lg p-4 mb-6">
          <pre class="text-green-400">
class User extends Model
{
    use HasEvolve;

    public function posts()
    {
        return $this->hasMany(Post::class)
            ->withEvolve()
            ->defaultSort('-created_at');
    }

    public function profile()
    {
        return $this->hasOne(Profile::class)
            ->withEvolve()
            ->withDefault();
    }

    public function roles()
    {
        return $this->belongsToMany(Role::class)
            ->withEvolve()
            ->withTimestamps();
    }
}</pre>
        </div>

        <h3 class="text-xl font-semibold mb-4">Relationship Loading</h3>
        <div class="bg-gray-800 rounded-lg p-4 mb-6">
          <pre class="text-green-400">
// Eager loading with constraints
$users = User::with(['posts' => function($query) {
    $query->published()->recent();
}])->get();

// Lazy eager loading
$user->load(['profile', 'roles']);

// Conditional loading
$user->loadWhen($condition, ['posts', 'profile']);</pre>
        </div>
      </section>

      <section id="migrations" class="mb-12">
        <h2 class="text-2xl font-semibold mb-4">4. Migrations</h2>
        
        <h3 class="text-xl font-semibold mb-4">Creating Migrations</h3>
        <div class="bg-gray-800 rounded-lg p-4 mb-6">
          <pre class="text-green-400">
php artisan evolve:make:migration create_users_table

// Migration file
public function up()
{
    Schema::create('users', function (Blueprint $table) {
        $table->id();
        $table->string('name');
        $table->string('email')->unique();
        $table->timestamp('email_verified_at')->nullable();
        $table->string('password');
        $table->json('settings')->nullable();
        $table->timestamps();
        $table->softDeletes();
    });
}</pre>
        </div>

        <h3 class="text-xl font-semibold mb-4">Indexes and Performance</h3>
        <div class="bg-gray-800 rounded-lg p-4 mb-6">
          <pre class="text-green-400">
Schema::table('users', function (Blueprint $table) {
    // Add indexes for frequently queried columns
    $table->index(['email', 'status']);
    
    // Full-text search index
    $table->fullText(['name', 'email']);
    
    // JSON column index
    $table->index('settings->theme');
});</pre>
        </div>
      </section>

      <section id="querying" class="mb-12">
        <h2 class="text-2xl font-semibold mb-4">5. Querying</h2>
        
        <h3 class="text-xl font-semibold mb-4">Advanced Queries</h3>
        <div class="bg-gray-800 rounded-lg p-4 mb-6">
          <pre class="text-green-400">
// Complex where clauses
User::query()
    ->where('status', 'active')
    ->whereHas('posts', function($query) {
        $query->published();
    })
    ->whereJsonContains('settings->permissions', 'admin')
    ->get();

// Dynamic scopes
User::withScope('active')
    ->withScope('hasVerifiedEmail')
    ->paginate();</pre>
        </div>

        <h3 class="text-xl font-semibold mb-4">Query Builders</h3>
        <div class="bg-gray-800 rounded-lg p-4 mb-6">
          <pre class="text-green-400">
// Custom query builder
class UserQueryBuilder extends EvolveQueryBuilder
{
    public function withActiveSubscription()
    {
        return $this->whereHas('subscription', function($query) {
            $query->active();
        });
    }

    public function searchByName($name)
    {
        return $this->where('name', 'like', "%{$name}%");
    }
}</pre>
        </div>
      </section>

      <section id="optimization" class="mb-12">
        <h2 class="text-2xl font-semibold mb-4">6. Optimization</h2>
        
        <h3 class="text-xl font-semibold mb-4">Query Optimization</h3>
        <div class="bg-gray-800 rounded-lg p-4 mb-6">
          <pre class="text-green-400">
// Enable query logging for debugging
DB::enableQueryLog();

// Chunk results for large datasets
User::chunk(100, function($users) {
    foreach ($users as $user) {
        // Process user
    }
});

// Lazy collection for memory efficiency
User::lazy()->each(function($user) {
    // Process user
});</pre>
        </div>

        <h3 class="text-xl font-semibold mb-4">Performance Tips</h3>
        <ul class="list-disc pl-6 space-y-2 text-gray-700">
          <li>Use appropriate indexes for frequently queried columns</li>
          <li>Implement database caching for frequently accessed data</li>
          <li>Use eager loading to prevent N+1 query problems</li>
          <li>Consider using cursor pagination for large datasets</li>
          <li>Optimize JSON column queries with indexes</li>
        </ul>
      </section>
    </UContainer>
  </div>
</template>

<script setup lang="ts">
// Component logic can be added here if needed
</script> 