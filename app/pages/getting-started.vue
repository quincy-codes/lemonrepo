<template>
  <div class="max-w-4xl mx-auto">
    <UContainer class="max-w-8xl py-12 space-y-6">
      <h1 class="text-4xl sm:text-6xl lg:text-7xl tracking-tight text-gray-800 font-bold text-center font-display max-w-4xl mx-auto">
        Getting Started Guide
      </h1>

      <div class="w-full columns-2 py-4">
          <ButtonLink label="Documentation" size="sm" rounded="md" href="/docs" />
      </div>

      <div class="bg-gray-100 p-6 rounded-lg mb-8">
        <ul class="space-y-2">
          <li><a href="#quick-start" class="text-blue-600 hover:underline">1. Quick Start</a></li>
          <li><a href="#basic-usage" class="text-blue-600 hover:underline">2. Basic Usage</a></li>
          <li><a href="#model-setup" class="text-blue-600 hover:underline">3. Model Setup</a></li>
          <li><a href="#querying" class="text-blue-600 hover:underline">4. Basic Querying</a></li>
          <li><a href="#relationships" class="text-blue-600 hover:underline">5. Working with Relationships</a></li>
          <li><a href="#next-steps" class="text-blue-600 hover:underline">6. Next Steps</a></li>
        </ul>
      </div>

      <section id="quick-start" class="mb-12">
        <h2 class="text-2xl font-semibold mb-4">1. Quick Start</h2>
        
        <h3 class="text-xl font-semibold mb-4">Installation</h3>
        <div class="bg-gray-800 rounded-lg p-4 mb-6">
          <pre class="text-green-400">
# Install via Composer
composer require thinkneverland/evolve

# Publish configuration
php artisan vendor:publish --provider="Evolve\EvolveServiceProvider" --tag="config"</pre>
        </div>

        <h3 class="text-xl font-semibold mb-4">Basic Setup</h3>
        <div class="bg-gray-800 rounded-lg p-4 mb-6">
          <pre class="text-green-400">
// Add to .env
EVOLVE_DEBUG=true
EVOLVE_API_PREFIX=api/v1

// Register service provider in config/app.php
'providers' => [
    // ...
    Evolve\EvolveServiceProvider::class,
];</pre>
        </div>
      </section>

      <section id="basic-usage" class="mb-12">
        <h2 class="text-2xl font-semibold mb-4">2. Basic Usage</h2>
        
        <h3 class="text-xl font-semibold mb-4">Creating Your First Model</h3>
        <div class="bg-gray-800 rounded-lg p-4 mb-6">
          <pre class="text-green-400">
<?php

namespace App\Models;

use Illuminate\Database\Eloquent\Model;
use Evolve\Traits\HasEvolve;

class Post extends Model
{
    use HasEvolve;

    protected $fillable = [
        'title',
        'content',
        'status',
    ];

    protected $evolveConfig = [
        'searchable' => ['title', 'content'],
        'filterable' => ['status', 'created_at'],
        'sortable' => ['created_at', 'updated_at'],
    ];
}</pre>
        </div>
      </section>

      <section id="model-setup" class="mb-12">
        <h2 class="text-2xl font-semibold mb-4">3. Model Setup</h2>
        
        <h3 class="text-xl font-semibold mb-4">Configuration Options</h3>
        <div class="bg-gray-800 rounded-lg p-4 mb-6">
          <pre class="text-green-400">
protected $evolveConfig = [
    // Define searchable fields
    'searchable' => ['title', 'content'],
    
    // Define filterable fields
    'filterable' => [
        'status',
        'category' => ['type' => 'exact'],
        'created_at' => ['type' => 'date'],
    ],
    
    // Define sortable fields
    'sortable' => ['created_at', 'title'],
    
    // Define relationships
    'relationships' => [
        'author' => ['type' => 'belongsTo'],
        'comments' => ['type' => 'hasMany'],
    ],
];</pre>
        </div>
      </section>

      <section id="querying" class="mb-12">
        <h2 class="text-2xl font-semibold mb-4">4. Basic Querying</h2>
        
        <h3 class="text-xl font-semibold mb-4">API Queries</h3>
        <div class="bg-gray-800 rounded-lg p-4 mb-6">
          <pre class="text-green-400">
# Basic query
GET /api/v1/posts

# With filters
GET /api/v1/posts?filter[status]=published

# With search
GET /api/v1/posts?search=Laravel

# With sorting
GET /api/v1/posts?sort=-created_at

# With pagination
GET /api/v1/posts?page=1&per_page=15</pre>
        </div>

        <h3 class="text-xl font-semibold mb-4">PHP Usage</h3>
        <div class="bg-gray-800 rounded-lg p-4 mb-6">
          <pre class="text-green-400">
// In your controller
use Evolve\Services\EvolveService;

public function index(EvolveService $evolve)
{
    return $evolve->model(Post::class)
        ->filter(['status' => 'published'])
        ->search('Laravel')
        ->sort('-created_at')
        ->paginate(15);
}</pre>
        </div>
      </section>

      <section id="relationships" class="mb-12">
        <h2 class="text-2xl font-semibold mb-4">5. Working with Relationships</h2>
        
        <h3 class="text-xl font-semibold mb-4">Including Relations</h3>
        <div class="bg-gray-800 rounded-lg p-4 mb-6">
          <pre class="text-green-400">
# Via API
GET /api/v1/posts?include=author,comments

# In PHP
$evolve->model(Post::class)
    ->with(['author', 'comments'])
    ->get();</pre>
        </div>

        <h3 class="text-xl font-semibold mb-4">Nested Filters</h3>
        <div class="bg-gray-800 rounded-lg p-4 mb-6">
          <pre class="text-green-400">
# Filter by relationship
GET /api/v1/posts?filter[author.name]=John

# Complex relationship filters
GET /api/v1/posts?filter[comments.created_at][gt]=2024-01-01</pre>
        </div>
      </section>

      <section id="next-steps" class="mb-12">
        <h2 class="text-2xl font-semibold mb-4">6. Next Steps</h2>
        
        <h3 class="text-xl font-semibold mb-4">Advanced Features</h3>
        <ul class="list-disc pl-6 space-y-2 text-gray-700">
          <li>Explore <a href="/advanced-usage" class="text-blue-600 hover:underline">Advanced Usage</a> for custom filters and complex queries</li>
          <li>Learn about <a href="/authentication" class="text-blue-600 hover:underline">Authentication</a> and security</li>
          <li>Configure <a href="/configuration" class="text-blue-600 hover:underline">Advanced Settings</a></li>
          <li>Build <a href="/evolve-extension-documentation" class="text-blue-600 hover:underline">Custom Extensions</a></li>
        </ul>

        <h3 class="text-xl font-semibold mb-4 mt-6">Community Resources</h3>
        <ul class="list-disc pl-6 space-y-2 text-gray-700">
          <li>Join our <a href="https://discord.gg/evolve" class="text-blue-600 hover:underline">Discord Community</a></li>
          <li>Check out the <a href="https://github.com/thinkneverland/evolve" class="text-blue-600 hover:underline">GitHub Repository</a></li>
          <li>Browse <a href="https://github.com/thinkneverland/evolve/examples" class="text-blue-600 hover:underline">Example Projects</a></li>
        </ul>
      </section>
    </UContainer>
  </div>
</template>

<script setup lang="ts">
// Component logic can be added here if needed
</script> 