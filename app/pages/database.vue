<template>
  <div class="max-w-4xl mx-auto">
    <UContainer class="max-w-8xl py-12 space-y-6">
      <h1 class="text-4xl sm:text-6xl lg:text-7xl tracking-tight text-gray-800 font-bold text-center font-display max-w-4xl mx-auto">
        Database Guide
      </h1>

      <div class="w-full columns-2 py-4">
          <ButtonLink label="Documentation" size="sm" rounded="md" href="/docs" />
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
&lt;?php

use ThinkNeverland\Evolve\Traits\HasEvolve;

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
        
        <h3 class="text-xl font-semibold mb-4">Relationship Discovery</h3>
        <p class="mb-4">Relationships are automatically discovered through reflection. Configuration is optional and only needed for customization.</p>
        
        <h3 class="text-xl font-semibold mb-4">Defining Relationships</h3>
        <div class="bg-gray-800 rounded-lg p-4 mb-6">
          <pre class="text-green-400">
class User extends Model
{
    use HasEvolve;

    // Relationships are automatically discovered
    public function posts()
    {
        return $this->hasMany(Post::class);
    }

    public function profile()
    {
        return $this->hasOne(Profile::class);
    }

    // Optional configuration for customization
    protected $evolveConfig = [
        'relationships' => [
            'posts' => [
                'fields' => ['title', 'content'],  // Only include specific fields
                'type' => 'hasMany',              // Optional - discovered automatically
                'constraints' => [                 // Optional - add query constraints
                    'published' => function($query) {
                        return $query->where('status', 'published');
                    },
                    'recent' => function($query) {
                        return $query->where('created_at', '>=', now()->subDays(7));
                    }
                ]
            ],
            'profile' => [
                'fields' => ['bio', 'avatar']     // Only include specific fields
            ]
        ],
        
        // Exclude specific relationships globally
        'exclude' => [
            'relationships' => ['audits']
        ]
    ];
}</pre>
        </div>

        <h3 class="text-xl font-semibold mb-4">Loading Relationships</h3>
        <div class="bg-gray-800 rounded-lg p-4 mb-6">
          <pre class="text-green-400">
// All discovered relationships are loaded by default unless excluded
$users = User::all();

// Load specific relationships
$users = User::with(['posts', 'profile'])->get();

// Load with specific fields
GET /api/users?include=posts:title,content,profile:bio

// Load with constraints
GET /api/users?include=posts:published:recent

// Exclude specific relationships
GET /api/users?exclude=audits

// Load nested relationships
GET /api/users?include=posts.comments,profile</pre>
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
        
        <h3 class="text-xl font-semibold mb-4">Basic Queries</h3>
        <div class="bg-gray-800 rounded-lg p-4 mb-6">
          <pre class="text-green-400">
// Basic CRUD operations
$users = User::all();
$user = User::find(1);
$users = User::where('status', 'active')->get();</pre>
        </div>

        <h3 class="text-xl font-semibold mb-4">Relationship Queries</h3>
        <div class="bg-gray-800 rounded-lg p-4 mb-6">
          <pre class="text-green-400">
// Automatic relationship loading (default behavior)
$users = User::all();  // Loads all discovered relationships unless excluded

// Specific relationships
$users = User::with(['posts', 'profile'])->get();

// With constraints
$users = User::with(['posts' => function($query) {
    $query->where('status', 'published')
         ->where('created_at', '>=', now()->subDays(7));
}])->get();

// Using predefined constraints
$users = User::with(['posts:published:recent'])->get();

// Nested relationships with constraints
$users = User::with([
    'posts.comments' => function($query) {
        $query->where('approved', true);
    },
    'roles:active'
])->get();</pre>
        </div>

        <h3 class="text-xl font-semibold mb-4">API Queries</h3>
        <div class="bg-gray-800 rounded-lg p-4 mb-6">
          <pre class="text-green-400">
// Basic filtering
GET /api/users?filter[status]=active

// Include relationships
GET /api/users?include=posts,profile

// Include with specific fields
GET /api/users?include=posts:title,content,profile:bio

// Include with constraints
GET /api/users?include=posts:published:recent

// Nested relationships
GET /api/users?include=posts.comments:approved,roles:active

// Exclude relationships
GET /api/users?exclude=audits,deleted_records

// Combining filters
GET /api/users?filter[status]=active&include=posts:published&exclude=audits</pre>
        </div>

        <h3 class="text-xl font-semibold mb-4">Advanced Queries</h3>
        <div class="bg-gray-800 rounded-lg p-4 mb-6">
          <pre class="text-green-400">
// Complex relationship queries
$users = User::whereHas('posts', function($query) {
    $query->published()->recent();
})->with([
    'posts' => function($query) {
        $query->select(['id', 'title', 'content'])
              ->published()
              ->recent();
    },
    'profile' => function($query) {
        $query->select(['id', 'user_id', 'bio']);
    }
])->get();

// API equivalent
GET /api/users?filter[has_posts]=true&include=posts:published:recent:fields(title|content),profile:fields(bio)</pre>
        </div>
      </section>

      <section id="optimization" class="mb-12">
        <h2 class="text-2xl font-semibold mb-4">6. Optimization</h2>
        
        <h3 class="text-xl font-semibold mb-4">Query Optimization</h3>
        <div class="bg-gray-800 rounded-lg p-4 mb-6">
          <pre class="text-green-400">
// Enable query logging for debugging
DB::enableQueryLog();

// Use configurable pagination
$users = User::paginate(
    config('evolve.pagination.per_page', 15),
    ['*'],
    'page',
    request('page', 1)
);

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

        <h3 class="text-xl font-semibold mb-4">Relationship Optimization</h3>
        <div class="bg-gray-800 rounded-lg p-4 mb-6">
          <pre class="text-green-400">
// Selective relationship loading
protected $evolveConfig = [
    'relationships' => [
        'posts' => [
            'fields' => ['id', 'title'],  // Select only needed fields
            'constraints' => [
                'summary' => function($query) {
                    return $query->select(['id', 'title', 'created_at'])
                                ->latest()
                                ->limit(5);
                }
            ]
        ]
    ]
];

// Efficient nested relationship loading
$users = User::with([
    'posts' => function($query) {
        $query->select(['id', 'user_id', 'title'])  // Select only needed fields
              ->latest()
              ->limit(10);
    },
    'posts.comments' => function($query) {
        $query->select(['id', 'post_id', 'content'])
              ->latest()
              ->limit(5);
    }
])->get();

// API optimization examples
GET /api/users?include=posts:fields(id|title):limit(10)
GET /api/users?include=posts.comments:fields(content):limit(5)

// Batch relationship loading
User::with(['posts', 'profile'])
    ->chunk(100, function($users) {
        foreach ($users as $user) {
            // Process user and relationships
        }
    });</pre>
        </div>

        <h3 class="text-xl font-semibold mb-4">Performance Tips</h3>
        <ul class="list-disc pl-6 space-y-2 text-gray-700">
          <li>Use appropriate indexes for frequently queried columns</li>
          <li>Implement database caching for frequently accessed data</li>
          <li>Use eager loading to prevent N+1 query problems</li>
          <li>Configure pagination settings in config/evolve.php</li>
          <li>Optimize JSON column queries with indexes</li>
          <li>Select only needed fields in relationships to reduce memory usage</li>
          <li>Use relationship constraints to limit data fetched</li>
          <li>Consider caching frequently accessed relationship data</li>
          <li>Use batch processing for large relationship operations</li>
          <li>Monitor relationship query performance with query logging</li>
        </ul>
      </section>
    </UContainer>
  </div>
</template>

<script setup lang="ts">
// Component logic can be added here if needed
</script> 