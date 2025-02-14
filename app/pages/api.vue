<template>
  <div class="max-w-4xl mx-auto">
    <UContainer class="max-w-8xl py-12 space-y-6">
      <h1 class="text-4xl sm:text-6xl lg:text-7xl tracking-tight text-gray-800 font-bold text-center font-display max-w-4xl mx-auto">
        API Documentation
      </h1>

      <div class="w-full columns-2 py-4">
          <ButtonLink label="Documentation" size="sm" rounded="md" href="/docs" />
      </div>

      <div class="bg-gray-100 p-6 rounded-lg mb-8">
        <ul class="space-y-2">
          <li><a href="#overview" class="text-blue-600 hover:underline">1. Overview</a></li>
          <li><a href="#configuration" class="text-blue-600 hover:underline">2. Configuration</a></li>
          <li><a href="#endpoints" class="text-blue-600 hover:underline">3. Endpoints</a></li>
          <li><a href="#querying" class="text-blue-600 hover:underline">4. Query Parameters</a></li>
          <li><a href="#responses" class="text-blue-600 hover:underline">5. Response Format</a></li>
          <li><a href="#models" class="text-blue-600 hover:underline">6. Model Configuration</a></li>
        </ul>
      </div>

      <section id="overview" class="mb-12">
        <h2 class="text-2xl font-semibold mb-4">1. Overview</h2>
        <p class="text-gray-700 mb-4">
          Evolve automatically generates RESTful APIs for your Laravel models. It provides powerful querying capabilities, relationship handling, and comprehensive response formatting out of the box.
        </p>
      </section>

      <section id="configuration" class="mb-12">
        <h2 class="text-2xl font-semibold mb-4">2. Configuration</h2>
        
        <h3 class="text-xl font-semibold mb-4">Basic Setup</h3>
        <div class="bg-gray-800 rounded-lg p-4 mb-6">
          <pre class="text-green-400">
// config/evolve.php
return [
    'auto_generate_api' => true,
    'api_route_prefix' => 'evolve-api',
    'api_auth_middleware' => ['api'],
    
    'response' => [
        'format' => 'json',
        'success_key' => 'success',
        'error_key' => 'error'
    ]
];</pre>
        </div>
      </section>

      <section id="endpoints" class="mb-12">
        <h2 class="text-2xl font-semibold mb-4">3. Endpoints</h2>
        
        <h3 class="text-xl font-semibold mb-4">Available Endpoints</h3>
        <div class="bg-gray-800 rounded-lg p-4 mb-6">
          <pre class="text-green-400">
# List resources
GET /evolve-api/{model}

# Create resource
POST /evolve-api/{model}

# Show resource
GET /evolve-api/{model}/{id}

# Update resource
PUT /evolve-api/{model}/{id}

# Delete resource
DELETE /evolve-api/{model}/{id}</pre>
        </div>
      </section>

      <section id="querying" class="mb-12">
        <h2 class="text-2xl font-semibold mb-4">4. Query Parameters</h2>
        
        <h3 class="text-xl font-semibold mb-4">Filtering</h3>
        <div class="bg-gray-800 rounded-lg p-4 mb-6">
          <pre class="text-green-400">
# Simple filters
GET /evolve-api/users?filter[status]=active

# Multiple filters
GET /evolve-api/users?filter[status]=active&filter[role]=admin

# Range filters
GET /evolve-api/users?filter[created_at][from]=2024-01-01</pre>
        </div>

        <h3 class="text-xl font-semibold mb-4">Sorting</h3>
        <div class="bg-gray-800 rounded-lg p-4 mb-6">
          <pre class="text-green-400">
# Ascending sort
GET /evolve-api/users?sort=name

# Descending sort
GET /evolve-api/users?sort=-name

# Multiple fields
GET /evolve-api/users?sort=-created_at,name</pre>
        </div>

        <h3 class="text-xl font-semibold mb-4">Including Relations</h3>
        <div class="bg-gray-800 rounded-lg p-4 mb-6">
          <pre class="text-green-400">
# Single relation
GET /evolve-api/users?include=profile

# Multiple relations
GET /evolve-api/users?include=profile,posts

# Nested relations
GET /evolve-api/users?include=posts.comments</pre>
        </div>

        <h3 class="text-xl font-semibold mb-4">Pagination</h3>
        <div class="bg-gray-800 rounded-lg p-4 mb-6">
          <pre class="text-green-400">
# Page-based pagination
GET /evolve-api/users?page=1&per_page=15</pre>
        </div>

        <h3 class="text-xl font-semibold mb-4">Search</h3>
        <div class="bg-gray-800 rounded-lg p-4 mb-6">
          <pre class="text-green-400">
# Global search
GET /evolve-api/users?search=john

# Field-specific search
GET /evolve-api/users?search[name]=john</pre>
        </div>
      </section>

      <section id="responses" class="mb-12">
        <h2 class="text-2xl font-semibold mb-4">5. Response Format</h2>
        
        <h3 class="text-xl font-semibold mb-4">Success Response</h3>
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
            "total": 100,
            "last_page": 7
        }
    }
}</pre>
        </div>

        <h3 class="text-xl font-semibold mb-4">Error Response</h3>
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

      <section id="models" class="mb-12">
        <h2 class="text-2xl font-semibold mb-4">6. Model Configuration</h2>
        
        <h3 class="text-xl font-semibold mb-4">Model Setup</h3>
        <div class="bg-gray-800 rounded-lg p-4 mb-6">
          <pre class="text-green-400">
use Thinkneverland\Evolve\Traits\HasEvolve;

class User extends Model
{
    use HasEvolve;

    protected $evolveConfig = [
        // Searchable fields
        'searchable' => ['name', 'email'],
        
        // Filterable fields
        'filterable' => ['status', 'role'],
        
        // Sortable fields
        'sortable' => ['created_at', 'name'],
        
        // Relationship configuration
        'relationships' => [
            'posts' => [
                'fields' => ['title', 'content'],
                'constraints' => [
                    'published' => fn($q) => $q->where('status', 'published')
                ]
            ]
        ],
        
        // Validation rules
        'validation' => [
            'rules' => [
                'email' => 'required|email|unique:users',
                'name' => 'required|min:2'
            ]
        ],
        
        // Cache configuration
        'cache' => [
            'enabled' => true,
            'ttl' => 3600
        ]
    ];
}</pre>
        </div>
      </section>
    </UContainer>
  </div>
</template>

<script setup lang="ts">
// Component logic can be added here if needed
</script> 