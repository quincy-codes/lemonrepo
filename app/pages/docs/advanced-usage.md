# Advanced Usage Guide

## Table of Contents

- [Custom Filters](#custom-filters)
- [Advanced Sorting](#advanced-sorting)
- [Complex Relations](#complex-relations)
- [Batch Operations](#batch-operations)
- [Event Handling](#event-handling)
- [Custom Extensions](#custom-extensions)

## Custom Filters

### Basic Filters

```http
# Range filters
GET /api/users?filter[age_range]=18,25&filter[status]=active

# Complex filters
GET /api/users?filter[or][0][status]=active&filter[or][1][role]=admin

# Nested filters
GET /api/users?filter[posts][status]=published&filter[posts][category]=tech
```

### Custom Filter Implementation

```php
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
}
```

## Advanced Sorting

### Custom Sort Implementation

```php
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
}
```

## Complex Relations

### Relation Loading with Constraints

```php
// In your model
protected $shadowConfig = [
    'relationships' => [
        'posts' => [
            'type' => 'hasMany',
            'constraints' => [
                'recent' => function($query) {
                    return $query->where('created_at', '>=', now()->subDays(7));
                },
                'popular' => function($query) {
                    return $query->where('views', '>=', 1000);
                }
            ]
        ]
    ]
];

// In your request
GET /api/users?include=posts:recent:popular
```

### Dynamic Relations

```php
class User extends Model
{
    use HasShadow;
    
    protected function setupRelations()
    {
        // Add dynamic relation
        $this->addRelation('trending_posts', function($query) {
            return $this->hasMany(Post::class)
                ->where('trending', true);
        });
        
        // Add computed relation
        $this->addComputedRelation('engagement_stats', function() {
            return [
                'posts_count' => $this->posts()->count(),
                'average_likes' => $this->posts()->avg('likes'),
                'total_comments' => $this->posts()->sum('comments_count')
            ];
        });
    }
}
```

## Batch Operations

### Transaction Handling

```php
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
                // Handle individual failures
                $results[] = [
                    'error' => $e->getMessage(),
                    'item' => $item
                ];
            }
        }
        
        return $results;
    });
}
```

### Bulk Processing with Progress

```php
// In your command
public function handle()
{
    Shadow::bulk(User::class)
        ->chunk(100)
        ->progress(function($progress) {
            $this->info("Processed {$progress->current} of {$progress->total}");
        })
        ->process(function($users) {
            foreach ($users as $user) {
                // Process each user
            }
        });
}
```

## Event Handling

### Custom Events

```php
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
});
```

### Event Webhooks

```php
// Configure webhook
POST /api/webhooks
{
    "url": "https://your-domain.com/webhook",
    "events": ["users.*", "posts.created"],
    "secret": "your-webhook-secret",
    "retry": {
        "max_attempts": 3,
        "backoff": "exponential"
    },
    "filters": {
        "users.created": {
            "role": ["admin", "moderator"]
        }
    }
}

// Webhook payload
{
    "event": "users.created",
    "timestamp": "2024-01-01T00:00:00Z",
    "data": {
        "id": 1,
        "type": "users",
        "attributes": {
            "name": "John Doe",
            "email": "john@example.com"
        }
    },
    "metadata": {
        "request_id": "req_123",
        "source": "api"
    }
}
```

## Custom Extensions

### Creating Custom Functionality

```php
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
}
```

For more examples and detailed implementation guides, refer to our [Core Documentation](CORE.md).
