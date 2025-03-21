<template>
  <div class="max-w-4xl mx-auto">
    <UContainer class="max-w-8xl py-12 space-y-6">
      <h1 class="text-4xl sm:text-6xl lg:text-7xl tracking-tight text-gray-800 font-bold text-center font-display max-w-4xl mx-auto">
        Configuration Guide
      </h1>

      <div class="w-full flex justify-center py-4">
          <ButtonLink label="Documentation" size="sm" rounded="md" href="/docs" class="w-full max-w-md" />
      </div>

      <div class="bg-gray-100 p-6 rounded-lg mb-8">
        <ul class="space-y-2">
          <li><a href="#overview" class="text-blue-600 hover:underline">1. Overview</a></li>
          <li><a href="#basic-config" class="text-blue-600 hover:underline">2. Basic Configuration</a></li>
          <li><a href="#model-config" class="text-blue-600 hover:underline">3. Model Configuration</a></li>
          <li><a href="#api-config" class="text-blue-600 hover:underline">4. API Configuration</a></li>
          <li><a href="#advanced-config" class="text-blue-600 hover:underline">5. Advanced Configuration</a></li>
          <li><a href="#env-variables" class="text-blue-600 hover:underline">6. Environment Variables</a></li>
        </ul>
      </div>

      <section id="overview" class="mb-12">
        <h2 class="text-2xl font-semibold mb-4">1. Overview</h2>
        <p class="text-gray-700 mb-4">
          Shadow's configuration system is designed to be flexible and extensible, allowing you to customize every aspect of the framework to suit your needs.
        </p>
      </section>

      <section id="basic-config" class="mb-12">
        <h2 class="text-2xl font-semibold mb-4">2. Basic Configuration</h2>
        
        <h3 class="text-xl font-semibold mb-4">Publishing Configuration</h3>
        <div class="bg-gray-800 rounded-lg p-4 mb-6">
          <pre class="text-green-400">
# Publish configuration file
php artisan vendor:publish --provider="ThinkNeverland\Shadow\ShadowServiceProvider" --tag="config"</pre>
        </div>

        <h3 class="text-xl font-semibold mb-4">Core Settings</h3>
        <div class="bg-gray-800 rounded-lg p-4 mb-6">
          <pre class="text-green-400">
// config/shadow.php
return [
    'name' => env('SHADOW_NAME', 'Shadow API'),
    'debug' => env('SHADOW_DEBUG', false),
    'timezone' => env('SHADOW_TIMEZONE', 'UTC'),
    'locale' => env('SHADOW_LOCALE', 'en'),
    
    'paths' => [
        'api' => 'api',
        'models' => 'app/Models',
        'controllers' => 'app/Http/Controllers',
    ],
    
    'middleware' => [
        'api' => [
            'throttle:api',
            'auth:sanctum',
        ],
    ],
];</pre>
        </div>
      </section>

      <section id="model-config" class="mb-12">
        <h2 class="text-2xl font-semibold mb-4">3. Model Configuration</h2>
        
        <h3 class="text-xl font-semibold mb-4">Model Settings</h3>
        <div class="bg-gray-800 rounded-lg p-4 mb-6">
          <pre class="text-green-400">
// User Model Configuration
class User extends Model
{
    protected $shadowConfig = [
        'resource' => [
            'type' => 'users',
            'attributes' => ['name', 'email'],
            'relationships' => ['posts', 'profile'],
            'hidden' => ['password'],
        ],
        
        'validation' => [
            'rules' => [
                'name' => 'required|string|max:255',
                'email' => 'required|email|unique:users',
            ],
        ],
        
        'search' => [
            'columns' => ['name', 'email'],
            'algorithm' => 'fulltext',
        ],
    ];
}</pre>
        </div>
      </section>

      <section id="api-config" class="mb-12">
        <h2 class="text-2xl font-semibold mb-4">4. API Configuration</h2>
        
        <h3 class="text-xl font-semibold mb-4">API Settings</h3>
        <div class="bg-gray-800 rounded-lg p-4 mb-6">
          <pre class="text-green-400">
'api' => [
    'version' => env('SHADOW_API_VERSION', 'v1'),
    'prefix' => env('SHADOW_API_PREFIX', 'api'),
    'domain' => env('SHADOW_API_DOMAIN', null),
    
    'rate_limiting' => [
        'enabled' => true,
        'max_attempts' => 60,
        'decay_minutes' => 1,
    ],
    
    'pagination' => [
        'default_limit' => 15,
        'max_limit' => 100,
    ],
    
    'transforms' => [
        'case' => 'camel',
        'dates' => 'ISO8601',
    ],
]</pre>
        </div>

        <h3 class="text-xl font-semibold mb-4">Response Configuration</h3>
        <div class="bg-gray-800 rounded-lg p-4 mb-6">
          <pre class="text-green-400">
'responses' => [
    'format' => 'json-api',
    'include_meta' => true,
    'wrap_response' => true,
    
    'headers' => [
        'Access-Control-Allow-Origin' => '*',
        'Access-Control-Allow-Methods' => 'GET, POST, PUT, DELETE',
    ],
]</pre>
        </div>
      </section>

      <section id="advanced-config" class="mb-12">
        <h2 class="text-2xl font-semibold mb-4">5. Advanced Configuration</h2>
        
        <h3 class="text-xl font-semibold mb-4">Custom Service Providers</h3>
        <div class="bg-gray-800 rounded-lg p-4 mb-6">
          <pre class="text-green-400">
'providers' => [
    'cache' => CustomCacheProvider::class,
    'auth' => CustomAuthProvider::class,
    'events' => CustomEventProvider::class,
],

// Register custom middleware
'middleware' => [
    'custom' => [
        CustomMiddleware::class,
    ],
],</pre>
        </div>

        <h3 class="text-xl font-semibold mb-4">Event Listeners</h3>
        <div class="bg-gray-800 rounded-lg p-4 mb-6">
          <pre class="text-green-400">
'events' => [
    'listeners' => [
        ModelCreated::class => [
            SendNotification::class,
            UpdateCache::class,
        ],
    ],
],</pre>
        </div>
      </section>

      <section id="env-variables" class="mb-12">
        <h2 class="text-2xl font-semibold mb-4">6. Environment Variables</h2>
        
        <h3 class="text-xl font-semibold mb-4">Available Variables</h3>
        <div class="bg-gray-800 rounded-lg p-4 mb-6">
          <pre class="text-green-400">
# Core Settings
SHADOW_NAME=Shadow API
SHADOW_DEBUG=false
SHADOW_TIMEZONE=UTC
SHADOW_LOCALE=en

# API Settings
SHADOW_API_VERSION=v1
SHADOW_API_PREFIX=api
SHADOW_API_DOMAIN=api.example.com

# Authentication
SHADOW_AUTH_DRIVER=sanctum
SHADOW_AUTH_LIFETIME=3600

# Cache Settings
SHADOW_CACHE_DRIVER=redis
SHADOW_CACHE_PREFIX=shadow
SHADOW_CACHE_TTL=3600

# Database
SHADOW_DB_CONNECTION=mysql
SHADOW_QUEUE_CONNECTION=redis</pre>
        </div>

        <h3 class="text-xl font-semibold mb-4">Environment Specific Configuration</h3>
        <div class="bg-gray-800 rounded-lg p-4 mb-6">
          <pre class="text-green-400">
// config/shadow.php
'environment_specific' => [
    'local' => [
        'debug' => true,
        'cache' => [
            'enabled' => false,
        ],
    ],
    'production' => [
        'debug' => false,
        'cache' => [
            'enabled' => true,
        ],
    ],
],</pre>
        </div>
      </section>
    </UContainer>
  </div>
</template>

<script setup lang="ts">
// Component logic can be added here if needed
</script> 