<template>
  <div class="max-w-4xl mx-auto">
    <UContainer class="max-w-8xl py-12 space-y-6">
      <h1 class="text-4xl sm:text-6xl lg:text-7xl tracking-tight text-gray-800 font-bold text-center font-display max-w-4xl mx-auto">
        Advanced Usage Guide
      </h1>

      <div class="w-full flex justify-center py-4">
          <ButtonLink label="Documentation" size="sm" rounded="md" href="/docs" class="w-full max-w-md" />
      </div>

      <div class="bg-gray-100 p-6 rounded-lg mb-8">
        <ul class="space-y-2">
          <li><a href="#custom-filters" class="text-blue-600 hover:underline">1. Custom Filters</a></li>
          <li><a href="#advanced-sorting" class="text-blue-600 hover:underline">2. Advanced Sorting</a></li>
          <li><a href="#complex-relations" class="text-blue-600 hover:underline">3. Complex Relations</a></li>
          <li><a href="#batch-operations" class="text-blue-600 hover:underline">4. Batch Operations</a></li>
          <li><a href="#event-handling" class="text-blue-600 hover:underline">5. Event Handling</a></li>
          <li><a href="#custom-extensions" class="text-blue-600 hover:underline">6. Custom Extensions</a></li>
        </ul>
      </div>

      <section id="custom-filters" class="mb-12">
        <h2 class="text-2xl font-semibold mb-4">1. Custom Filters</h2>
        
        <h3 class="text-xl font-semibold mb-4">Basic Filters</h3>
        <div class="bg-gray-800 rounded-lg p-4 mb-6">
          <pre class="text-green-400">
# Range filters
GET /api/users?filter[age_range]=18,25&filter[status]=active

# Complex filters
GET /api/users?filter[or][0][status]=active&filter[or][1][role]=admin

# Nested filters
GET /api/users?filter[posts][status]=published&filter[posts][category]=tech</pre>
        </div>

        <h3 class="text-xl font-semibold mb-4">Custom Filter Implementation</h3>
        <div class="bg-gray-800 rounded-lg p-4 mb-6">
          <pre class="text-green-400">
class User extends Model
{
    use HasShadow;
    
    protected function setupFilters()
    {
        // Simple filter
        $this->addFilter('status', function($query, $value) {
            return $query->where('status', $value);
        });
        
        // Range filter
        $this->addFilter('age_range', function($query, $value) {
            [$min, $max] = explode(',', $value);
            return $query->whereBetween('age', [$min, $max]);
        });
        
        // Complex filter
        $this->addFilter('active_last_month', function($query) {
            return $query->where('last_active_at', '>=', now()->subMonth());
        });
    }
}</pre>
        </div>
      </section>

      <section id="advanced-sorting" class="mb-12">
        <h2 class="text-2xl font-semibold mb-4">2. Advanced Sorting</h2>
        
        <h3 class="text-xl font-semibold mb-4">Custom Sort Implementation</h3>
        <div class="bg-gray-800 rounded-lg p-4 mb-6">
          <pre class="text-green-400">
class User extends Model
{
    use HasShadow;
    
    protected function setupSorting()
    {
        // Custom sort by distance
        $this->addSort('distance', function($query, $direction, $params) {
            [$lat, $lng] = explode(',', $params);
            return $query->orderByRaw(
                'ST_Distance(location, ST_Point(?, ?)) ?',
                [$lng, $lat, $direction]
            );
        });
        
        // Sort by relationship count
        $this->addSort('posts_count', function($query, $direction) {
            return $query->withCount('posts')
                ->orderBy('posts_count', $direction);
        });
    }
}</pre>
        </div>
      </section>

      <section id="complex-relations" class="mb-12">
        <h2 class="text-2xl font-semibold mb-4">3. Complex Relations</h2>
        
        <h3 class="text-xl font-semibold mb-4">Advanced Relationship Configuration</h3>
        <div class="bg-gray-800 rounded-lg p-4 mb-6">
          <pre class="text-green-400">
// In your model
protected $shadowConfig = [
    'relationships' => [
        'posts' => [
            'fields' => ['title', 'content', 'status'],
            'type' => 'hasMany',              // Optional - discovered automatically
            'constraints' => [
                'published' => function($query) {
                    return $query->where('status', 'published');
                },
                'recent' => function($query) {
                    return $query->where('created_at', '>=', now()->subDays(7));
                },
                'popular' => function($query) {
                    return $query->where('views', '>=', 1000);
                }
            ]
        ],
        'profile' => [
            'fields' => ['bio', 'avatar'],
            'type' => 'hasOne'               // Optional - discovered automatically
        ],
        'roles' => [
            'fields' => ['name', 'permissions'],
            'type' => 'belongsToMany',       // Optional - discovered automatically
            'constraints' => [
                'active' => function($query) {
                    return $query->where('active', true);
                }
            ]
        ]
    ],
    
    // Global relationship exclusions
    'exclude' => [
        'relationships' => ['audits', 'deleted_records']
    ]
];

// In your API requests
GET /api/users?include=posts:published:recent
GET /api/users?include=posts.comments,profile
GET /api/users?include=roles:active
GET /api/users?exclude=audits,deleted_records

// In your code
$users = User::with(['posts' => function($query) {
    $query->published()->recent();
}])->get();

// With nested constraints
$users = User::with([
    'posts.comments' => function($query) {
        $query->where('approved', true);
    },
    'roles' => function($query) {
        $query->active();
    }
])->get();</pre>
        </div>
      </section>

      <section id="batch-operations" class="mb-12">
        <h2 class="text-2xl font-semibold mb-4">4. Batch Operations</h2>
        
        <h3 class="text-xl font-semibold mb-4">Transaction Handling</h3>
        <div class="bg-gray-800 rounded-lg p-4 mb-6">
          <pre class="text-green-400">
// In your controller
public function batchUpdate(Request $request)
{
    return Shadow::transaction(function($shadow) use ($request) {
        $results = [];
        
        foreach ($request->items as $item) {
            try {
                $results[] = $shadow->model(User::class)
                    ->findOrFail($item['id'])
                    ->update($item['data']);
            } catch (Exception $e) {
                $results[] = [
                    'error' => $e->getMessage(),
                    'item' => $item
                ];
            }
        }
        
        return $results;
    });
}</pre>
        </div>
      </section>

      <section id="event-handling" class="mb-12">
        <h2 class="text-2xl font-semibold mb-4">5. Event Handling</h2>
        
        <h3 class="text-xl font-semibold mb-4">Custom Events</h3>
        <div class="bg-gray-800 rounded-lg p-4 mb-6">
          <pre class="text-green-400">
// Define custom events
class UserEvents
{
    public static function register()
    {
        Shadow::defineEvents([
            'users.verified' => function($user) {
                return $user->email_verified_at !== null;
            },
            'users.became_premium' => function($user, $previous) {
                return !$previous->is_premium && $user->is_premium;
            }
        ]);
    }
}

// Listen for events
Shadow::on('users.verified', function($user) {
    // Send welcome email
});

// Conditional events
Shadow::on('users.became_premium', function($user) {
    // Grant premium benefits
})->when(function() {
    return config('features.premium_enabled');
});</pre>
        </div>
      </section>

      <section id="custom-extensions" class="mb-12">
        <h2 class="text-2xl font-semibold mb-4">6. Custom Extensions</h2>
        
        <h3 class="text-xl font-semibold mb-4">Creating Custom Functionality</h3>
        <div class="bg-gray-800 rounded-lg p-4 mb-6">
          <pre class="text-green-400">
class CustomShadowFunctionality
{
    public static function register()
    {
        // Add custom query method
        Shadow::macro('withCustomLogic', function($params) {
            return $this->query->where(...);
        });
        
        // Add custom response formatter
        Shadow::formatter('custom', function($data, $options) {
            return [
                'data' => $data,
                'custom' => $options
            ];
        });
        
        // Add custom validation rule
        Shadow::rule('custom_rule', function($value, $params) {
            return // validation logic;
        });
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