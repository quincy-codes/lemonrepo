<template>
  <div class="max-w-4xl mx-auto">
    <UContainer class="max-w-8xl py-12 space-y-6">
      <h1 class="text-4xl sm:text-6xl lg:text-7xl tracking-tight text-gray-800 font-bold text-center font-display max-w-4xl mx-auto">
        Evolve Documentation
      </h1>

      <div class="w-full columns-2 py-4">
        <ButtonLink href="/evolve-core-documentation" />
        <ButtonLink href="/evolve-api-documentation" />
        <ButtonLink href="/evolve-dashboard-documentation" />
      </div>

      <h3 class="text-4xl sm:text-6xl lg:text-7xl tracking-tight text-gray-800 font-bold text-center font-display max-w-4xl mx-auto">
        Evolve API (Optional Free Extension)
      </h3>

      <div class="w-full py-4">
        <div class="text-center text-gray-600 mb-8">
          A RESTful API generator for Laravel Evolve models with built-in optimization, validation, and documentation.
        </div>
      </div>

      <div class="bg-gray-100 p-6 rounded-lg mb-8">
        <ul class="space-y-2">
          <li><a href="#installation" class="text-blue-600 hover:underline">1. Installation & Setup</a></li>
          <li><a href="#basic-usage" class="text-blue-600 hover:underline">2. Basic Usage</a></li>
          <li>
            <a href="#endpoints" class="text-blue-600 hover:underline">3. API Endpoints</a>
            <ul class="ml-4 mt-2">
              <li><a href="#listing" class="text-blue-600 hover:underline">3.1 Listing Resources</a></li>
              <li><a href="#retrieval" class="text-blue-600 hover:underline">3.2 Retrieving Resources</a></li>
              <li><a href="#creation" class="text-blue-600 hover:underline">3.3 Creating Resources</a></li>
              <li><a href="#updating" class="text-blue-600 hover:underline">3.4 Updating Resources</a></li>
              <li><a href="#deletion" class="text-blue-600 hover:underline">3.5 Deleting Resources</a></li>
            </ul>
          </li>
          <li>
            <a href="#querying" class="text-blue-600 hover:underline">4. Query Parameters</a>
            <ul class="ml-4 mt-2">
              <li><a href="#filtering" class="text-blue-600 hover:underline">4.1 Filtering</a></li>
              <li><a href="#sorting" class="text-blue-600 hover:underline">4.2 Sorting</a></li>
              <li><a href="#including" class="text-blue-600 hover:underline">4.3 Including Relations</a></li>
              <li><a href="#pagination" class="text-blue-600 hover:underline">4.4 Pagination</a></li>
              <li><a href="#selecting" class="text-blue-600 hover:underline">4.5 Selecting Fields</a></li>
            </ul>
          </li>
          <li>
            <a href="#responses" class="text-blue-600 hover:underline">5. Response Format</a>
            <ul class="ml-4 mt-2">
              <li><a href="#success" class="text-blue-600 hover:underline">5.1 Success Responses</a></li>
              <li><a href="#errors" class="text-blue-600 hover:underline">5.2 Error Responses</a></li>
              <li><a href="#metadata" class="text-blue-600 hover:underline">5.3 Metadata</a></li>
            </ul>
          </li>
          <li>
            <a href="#advanced" class="text-blue-600 hover:underline">6. Advanced Usage</a>
            <ul class="ml-4 mt-2">
              <li><a href="#custom-endpoints" class="text-blue-600 hover:underline">6.1 Custom Endpoints</a></li>
              <li><a href="#hooks" class="text-blue-600 hover:underline">6.2 Request/Response Hooks</a></li>
              <li><a href="#validation" class="text-blue-600 hover:underline">6.3 Custom Validation</a></li>
              <li><a href="#authorization" class="text-blue-600 hover:underline">6.4 Authorization</a></li>
            </ul>
          </li>
        </ul>
      </div>

      <h2 id="installation" class="text-center mx-auto max-w-xl text-2xl sm:text-2xl font-semibold tracking-tight text-gray-900 font-display mb-4 mt-24">
        1. Installation & Setup
      </h2>

      <p class="mb-4">First, install the package via Composer:</p>

      <pre class="bg-gray-800 rounded-lg p-4 overflow-x-auto mb-6">
<code class="text-green-400">composer require thinkneverland/evolve-api</code>
</pre>

      <p class="mb-4">Publish the configuration file:</p>

      <pre class="bg-gray-800 rounded-lg p-4 overflow-x-auto mb-6">
<code class="text-green-400">php artisan vendor:publish --provider="Thinkneverland\Evolve\Api\EvolveApiServiceProvider"</code>
</pre>

      <p class="mb-4">Configure your API settings in <code>config/evolve-api.php</code>:</p>

      <pre class="bg-gray-800 rounded-lg p-4 overflow-x-auto mb-6">
<code class="text-green-400">return [
    'route_prefix' => 'api', // Base URL prefix for API routes

    'responses' => [
        'include_meta' => true,  // Include metadata in responses
        'wrap_response' => true, // Wrap responses in data key
    ],

    'docs' => [
        'enabled' => true,      // Enable OpenAPI documentation
        'route' => 'docs',      // Documentation route
        'title' => 'API Documentation',
    ],
];</code>
</pre>

      <h2 id="basic-usage" class="text-center mx-auto max-w-xl text-2xl sm:text-2xl font-semibold tracking-tight text-gray-900 font-display mb-4 mt-24">
        2. Basic Usage
      </h2>

      <p class="mb-4">Make your models API-accessible by implementing the interface and using the trait:</p>

      <pre class="bg-gray-800 rounded-lg p-4 overflow-x-auto mb-6">
<code class="text-green-400">use Thinkneverland\Evolve\Core\Traits\Evolvable;
use Thinkneverland\Evolve\Core\Contracts\EvolveModelInterface;

class Post extends Model implements EvolveModelInterface
{
    use Evolvable;

    protected $fillable = ['title', 'content', 'status'];

    // Optional: Define validation rules
    public function validationRules(string $action): array
    {
        return [
            'create' => [
                'title' => 'required|string|max:255',
                'content' => 'required|string',
                'status' => 'in:draft,published'
            ],
            'update' => [
                'title' => 'string|max:255',
                'content' => 'string',
                'status' => 'in:draft,published'
            ]
        ][$action] ?? [];
    }
}</code>
</pre>

      <p class="mb-4">The API endpoints will be automatically registered for your model at:</p>

      <pre class="bg-gray-800 rounded-lg p-4 overflow-x-auto mb-6">
<code class="text-green-400">GET    /api/posts          # List all posts
POST   /api/posts          # Create a new post
GET    /api/posts/{id}     # Get a specific post
PUT    /api/posts/{id}     # Update a post
DELETE /api/posts/{id}     # Delete a post</code>
</pre>

      <h2 id="endpoints" class="text-center mx-auto max-w-xl text-2xl sm:text-2xl font-semibold tracking-tight text-gray-900 font-display mb-4 mt-24">
        3. API Endpoints
      </h2>

      <h3 id="listing" class="text-xl mt-8 mb-4 font-semibold">3.1 Listing Resources</h3>

      <p class="mb-4">Get a paginated list of resources:</p>

      <pre class="bg-gray-800 rounded-lg p-4 overflow-x-auto mb-6">
<code class="text-green-400">GET /api/posts

# Optional query parameters:
?page=1              # Page number
?per_page=15         # Items per page
?sort=-created_at    # Sort by creation date (descending)
?filter[status]=published  # Filter by status
?include=author,comments   # Include relations</code>
</pre>

      <p class="mb-4">Example Response:</p>

      <pre class="bg-gray-800 rounded-lg p-4 overflow-x-auto mb-6">
<code class="text-green-400">{
    "data": [
        {
            "id": 1,
            "title": "First Post",
            "content": "Hello world!",
            "status": "published",
            "created_at": "2024-01-01T12:00:00Z",
            "updated_at": "2024-01-01T12:00:00Z",
            "author": {
                "id": 1,
                "name": "John Doe"
            },
            "comments": [
                {
                    "id": 1,
                    "content": "Great post!"
                }
            ]
        }
        // ... more posts
    ],
    "meta": {
        "current_page": 1,
        "per_page": 15,
        "total": 50,
        "total_pages": 4
    },
    "links": {
        "first": "/api/posts?page=1",
        "last": "/api/posts?page=4",
        "prev": null,
        "next": "/api/posts?page=2"
    }
}</code>
</pre>
      ```html
      <h3 id="retrieval" class="text-xl mt-8 mb-4 font-semibold">3.2 Retrieving Resources</h3>

      <p class="mb-4">Get a single resource by ID:</p>

      <pre class="bg-gray-800 rounded-lg p-4 overflow-x-auto mb-6">
<code class="text-green-400">GET /api/posts/1

# Optional parameters:
?include=author,comments     # Include related resources
?fields=id,title,content    # Select specific fields</code>
</pre>

      <p class="mb-4">Example Response:</p>

      <pre class="bg-gray-800 rounded-lg p-4 overflow-x-auto mb-6">
<code class="text-green-400">{
    "data": {
        "id": 1,
        "title": "First Post",
        "content": "Hello world!",
        "status": "published",
        "created_at": "2024-01-01T12:00:00Z",
        "updated_at": "2024-01-01T12:00:00Z",
        "author": {
            "id": 1,
            "name": "John Doe"
        }
    }
}</code>
</pre>

      <h3 id="creation" class="text-xl mt-8 mb-4 font-semibold">3.3 Creating Resources</h3>

      <p class="mb-4">Create a new resource:</p>

      <pre class="bg-gray-800 rounded-lg p-4 overflow-x-auto mb-6">
<code class="text-green-400">POST /api/posts
Content-Type: application/json

{
    "title": "New Post",
    "content": "This is the content",
    "status": "draft",
    "category_id": 1,
    "tags": [1, 2, 3]
}

# Optional parameters:
?include=author,category    # Include relations in response</code>
</pre>

      <p class="mb-4">Example Response (201 Created):</p>

      <pre class="bg-gray-800 rounded-lg p-4 overflow-x-auto mb-6">
<code class="text-green-400">{
    "data": {
        "id": 2,
        "title": "New Post",
        "content": "This is the content",
        "status": "draft",
        "created_at": "2024-01-02T15:30:00Z",
        "updated_at": "2024-01-02T15:30:00Z",
        "category": {
            "id": 1,
            "name": "Technology"
        }
    },
    "meta": {
        "message": "Resource created successfully"
    }
}</code>
</pre>

      <h3 id="updating" class="text-xl mt-8 mb-4 font-semibold">3.4 Updating Resources</h3>

      <p class="mb-4">Update an existing resource:</p>

      <pre class="bg-gray-800 rounded-lg p-4 overflow-x-auto mb-6">
<code class="text-green-400">PUT /api/posts/2
Content-Type: application/json

{
    "title": "Updated Title",
    "status": "published"
}

# Optional: PATCH method for partial updates
PATCH /api/posts/2
Content-Type: application/json

{
    "status": "published"
}</code>
</pre>

      <p class="mb-4">Example Response (200 OK):</p>

      <pre class="bg-gray-800 rounded-lg p-4 overflow-x-auto mb-6">
<code class="text-green-400">{
    "data": {
        "id": 2,
        "title": "Updated Title",
        "content": "This is the content",
        "status": "published",
        "created_at": "2024-01-02T15:30:00Z",
        "updated_at": "2024-01-02T15:35:00Z"
    },
    "meta": {
        "message": "Resource updated successfully"
    }
}</code>
</pre>

      <h3 id="deletion" class="text-xl mt-8 mb-4 font-semibold">3.5 Deleting Resources</h3>

      <p class="mb-4">Delete a resource:</p>

      <pre class="bg-gray-800 rounded-lg p-4 overflow-x-auto mb-6">
<code class="text-green-400">DELETE /api/posts/2

# Optional: Soft delete behavior is respected if model uses SoftDeletes
# Force delete with query parameter:
DELETE /api/posts/2?force=true</code>
</pre>

      <p class="mb-4">Example Response (200 OK):</p>

      <pre class="bg-gray-800 rounded-lg p-4 overflow-x-auto mb-6">
<code class="text-green-400">{
    "meta": {
        "message": "Resource deleted successfully"
    }
}</code>
</pre>

      <h2 id="querying" class="text-center mx-auto max-w-xl text-2xl sm:text-2xl font-semibold tracking-tight text-gray-900 font-display mb-4 mt-24">
        4. Query Parameters
      </h2>

      <h3 id="filtering" class="text-xl mt-8 mb-4 font-semibold">4.1 Filtering</h3>

      <p class="mb-4">Filter resources using various operators:</p>

      <pre class="bg-gray-800 rounded-lg p-4 overflow-x-auto mb-6">
<code class="text-green-400"># Basic equality
GET /api/posts?filter[status]=published

# Multiple conditions
GET /api/posts?filter[status]=published&filter[category_id]=1

# Comparison operators
GET /api/posts?filter[views][gt]=1000
GET /api/posts?filter[rating][gte]=4.5
GET /api/posts?filter[created_at][lt]=2024-01-01

# Between range
GET /api/posts?filter[created_at][between]=2024-01-01,2024-12-31

# IN operator
GET /api/posts?filter[status][in]=draft,published

# Like operator (SQL LIKE)
GET /api/posts?filter[title][like]=%keyword%

# Null check
GET /api/posts?filter[deleted_at][null]=true

# Relation filters
GET /api/posts?filter[author.name]=John
GET /api/posts?filter[comments_count][gt]=5</code>
</pre>

      <h3 id="sorting" class="text-xl mt-8 mb-4 font-semibold">4.2 Sorting</h3>

      <p class="mb-4">Sort resources by one or multiple fields:</p>

      <pre class="bg-gray-800 rounded-lg p-4 overflow-x-auto mb-6">
<code class="text-green-400"># Single field ascending
GET /api/posts?sort=title

# Single field descending
GET /api/posts?sort=-created_at

# Multiple fields
GET /api/posts?sort=-status,created_at

# Sort by relation
GET /api/posts?sort=author.name,-comments_count</code>
</pre>

      <h3 id="including" class="text-xl mt-8 mb-4 font-semibold">4.3 Including Relations</h3>

      <p class="mb-4">Include related resources in the response:</p>

      <pre class="bg-gray-800 rounded-lg p-4 overflow-x-auto mb-6">
<code class="text-green-400"># Include single relation
GET /api/posts?include=author

# Include multiple relations
GET /api/posts?include=author,comments

# Include nested relations
GET /api/posts?include=author.profile,comments.replies

# Include specific fields from relations
GET /api/posts?include=author:id,name;comments:id,content</code>
</pre>

      <h3 id="pagination" class="text-xl mt-8 mb-4 font-semibold">4.4 Pagination</h3>

      <p class="mb-4">Control pagination of list results:</p>

      <pre class="bg-gray-800 rounded-lg p-4 overflow-x-auto mb-6">
<code class="text-green-400"># Basic pagination
GET /api/posts?page=2&per_page=15

# Cursor pagination
GET /api/posts?after=eyJpZCI6MTAwfQ==&limit=15

# Simple pagination (no total count)
GET /api/posts?simple_page=2&per_page=15</code>
</pre>

      <h3 id="selecting" class="text-xl mt-8 mb-4 font-semibold">4.5 Selecting Fields</h3>

      <p class="mb-4">Select specific fields to include in the response:</p>

      <pre class="bg-gray-800 rounded-lg p-4 overflow-x-auto mb-6">
<code class="text-green-400"># Select specific fields
GET /api/posts?fields=id,title,created_at

# Select fields from relations
GET /api/posts?fields=id,title&include=author:id,name

# Select all fields from main resource but limit relation fields
GET /api/posts?fields=*&include=comments:id,content</code>
</pre>

      <h2 id="responses" class="text-center mx-auto max-w-xl text-2xl sm:text-2xl font-semibold tracking-tight text-gray-900 font-display mb-4 mt-24">
        5. Response Format
      </h2>

      <h3 id="success" class="text-xl mt-8 mb-4 font-semibold">5.1 Success Responses</h3>

      <p class="mb-4">Successful responses follow this structure:</p>

      <pre class="bg-gray-800 rounded-lg p-4 overflow-x-auto mb-6">
<code class="text-green-400">{
    "data": {
        // Resource data
    },
    "meta": {
        "message": "Operation successful",
        // Additional metadata
    },
    "links": {
        // HATEOAS links when applicable
    }
}</code>
</pre>

      <h3 id="errors" class="text-xl mt-8 mb-4 font-semibold">5.2 Error Responses</h3>

      <p class="mb-4">Error responses follow a consistent format:</p>

      <pre class="bg-gray-800 rounded-lg p-4 overflow-x-auto mb-6">
<code class="text-green-400">{
   "error": {
       "type": "ValidationError",
       "message": "The given data was invalid.",
       "details": {
           "title": ["The title field is required."],
           "status": ["The selected status is invalid."]
       }
   },
   "meta": {
       "debug_id": "7b4f3c19", // If debug mode enabled
       "timestamp": "2024-01-02T15:35:00Z"
   }
}</code>
</pre>

      <p class="mb-4">Common HTTP status codes:</p>

      <pre class="bg-gray-800 rounded-lg p-4 overflow-x-auto mb-6">
<code class="text-green-400">200 OK           - Successful request
201 Created      - Resource created successfully
400 Bad Request  - Invalid request parameters
401 Unauthorized - Missing or invalid authentication
403 Forbidden    - Authentication valid but insufficient permissions
404 Not Found    - Resource not found
422 Unprocessable Entity - Validation failed
429 Too Many Requests   - Rate limit exceeded
500 Server Error       - Unexpected server error</code>
</pre>

      <h3 id="metadata" class="text-xl mt-8 mb-4 font-semibold">5.3 Metadata</h3>

      <p class="mb-4">Response metadata includes useful information about the request:</p>

      <pre class="bg-gray-800 rounded-lg p-4 overflow-x-auto mb-6">
<code class="text-green-400">{
   "data": [...],
   "meta": {
       // Pagination info
       "current_page": 1,
       "per_page": 15,
       "total": 50,
       "total_pages": 4,

       // Query info
       "filters_applied": {
           "status": "published",
           "category_id": 1
       },
       "sort_applied": "-created_at",

       // Performance metrics
       "query_time": 125, // milliseconds
       "cached": true,

       // Additional context
       "timezone": "UTC",
       "api_version": "1.0"
   }
}</code>
</pre>

      <h2 id="advanced" class="text-center mx-auto max-w-xl text-2xl sm:text-2xl font-semibold tracking-tight text-gray-900 font-display mb-4 mt-24">
        6. Advanced Usage
      </h2>

      <h3 id="custom-endpoints" class="text-xl mt-8 mb-4 font-semibold">6.1 Custom Endpoints</h3>

      <p class="mb-4">Add custom endpoints by extending the base controller:</p>

      <pre class="bg-gray-800 rounded-lg p-4 overflow-x-auto mb-6">
<code class="text-green-400">use Thinkneverland\Evolve\Api\Http\Controllers\EvolveApiController;

class PostController extends EvolveApiController
{
   public function publish($id)
   {
       $post = Post::findOrFail($id);

       $this->authorize('publish', $post);

       $post->update(['status' => 'published']);

       return $this->respondWithResource($post, [
           'message' => 'Post published successfully'
       ]);
   }

   public function featured()
   {
       $posts = Post::featured()
           ->withOptimizedQueries()
           ->paginate();

       return $this->respondWithCollection($posts);
   }
}

// Register custom routes
Route::put('/api/posts/{id}/publish', [PostController::class, 'publish']);
Route::get('/api/posts/featured', [PostController::class, 'featured']);</code>
</pre>

      <h3 id="hooks" class="text-xl mt-8 mb-4 font-semibold">6.2 Request/Response Hooks</h3>

      <p class="mb-4">Customize request handling and response formatting with hooks:</p>

      <pre class="bg-gray-800 rounded-lg p-4 overflow-x-auto mb-6">
<code class="text-green-400">class PostController extends EvolveApiController
{
   // Modify data before creation
   protected function beforeCreate(Request $request, array $data)
   {
       $data['user_id'] = auth()->id();
       $data['slug'] = Str::slug($data['title']);
       return $data;
   }

   // Additional processing after update
   protected function afterUpdate(Request $request, $model)
   {
       Cache::tags('posts')->flush();
       event(new PostUpdated($model));
   }

   // Customize resource transformation
   protected function transformResource($model)
   {
       $data = parent::transformResource($model);
       $data['read_time'] = $this->calculateReadTime($data['content']);
       return $data;
   }

   // Add custom metadata
   protected function addResponseMetadata($response)
   {
       return array_merge(parent::addResponseMetadata($response), [
           'last_updated' => Cache::get('posts.last_updated'),
           'total_posts' => Post::count()
       ]);
   }
}</code>
</pre>

      <h3 id="validation" class="text-xl mt-8 mb-4 font-semibold">6.3 Custom Validation</h3>

      <p class="mb-4">Implement custom validation logic:</p>

      <pre class="bg-gray-800 rounded-lg p-4 overflow-x-auto mb-6">
<code class="text-green-400">class Post extends Model implements EvolveModelInterface
{
   use Evolvable;

   public function validationRules(string $action): array
   {
       $rules = [
           'create' => [
               'title' => ['required', 'string', 'max:255', 'unique:posts'],
               'content' => ['required', 'string', 'min:100'],
               'status' => ['in:draft,published'],
               'category_id' => ['required', 'exists:categories,id'],
               'tags' => ['array'],
               'tags.*' => ['exists:tags,id'],
               'publish_at' => ['nullable', 'date', 'after:today']
           ],
           'update' => [
               'title' => ['string', 'max:255', Rule::unique('posts')->ignore($this->id)],
               'content' => ['string', 'min:100'],
               'status' => ['in:draft,published'],
               'category_id' => ['exists:categories,id'],
               'tags' => ['array'],
               'tags.*' => ['exists:tags,id'],
               'publish_at' => ['nullable', 'date', 'after:today']
           ]
       ];

       return $rules[$action] ?? [];
   }

   public function validationMessages(): array
   {
       return [
           'content.min' => 'Posts must be at least 100 characters long.',
           'publish_at.after' => 'Schedule date must be in the future.'
       ];
   }

   // Custom validation logic
   public function validate(array $data, string $action = 'create'): bool
   {
       $isValid = parent::validate($data, $action);

       if ($isValid && ($data['status'] ?? null) === 'published') {
           // Additional validation for publishing
           return $this->validateForPublishing($data);
       }

       return $isValid;
   }
}</code>
</pre>

      <h3 id="authorization" class="text-xl mt-8 mb-4 font-semibold">6.4 Authorization</h3>

      <p class="mb-4">Implement fine-grained authorization:</p>

      <pre class="bg-gray-800 rounded-lg p-4 overflow-x-auto mb-6">
<code class="text-green-400">class PostPolicy
{
   public function viewAny(User $user): bool
   {
       return true;
   }

   public function view(User $user, Post $post): bool
   {
       return $post->status === 'published' || $user->id === $post->user_id;
   }

   public function create(User $user): bool
   {
       return $user->hasVerifiedEmail();
   }

   public function update(User $user, Post $post): bool
   {
       return $user->id === $post->user_id || $user->isAdmin();
   }

   public function delete(User $user, Post $post): bool
   {
       return $user->id === $post->user_id || $user->isAdmin();
   }

   public function publish(User $user, Post $post): bool
   {
       return $user->id === $post->user_id &&
              $user->hasVerifiedEmail() &&
              !$user->isBanned();
   }
}</code>
</pre>

      <p class="mb-4">These policies are automatically enforced by the API endpoints.</p>

    </UContainer>
  </div>
</template>
<script setup lang="ts">
</script>