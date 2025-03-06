<template>
  <div class="max-w-4xl mx-auto">
    <UContainer class="max-w-8xl py-12 space-y-6">
      <h1 class="text-4xl sm:text-6xl lg:text-7xl tracking-tight text-gray-800 font-bold text-center font-display max-w-4xl mx-auto">
        Extending Shadow
      </h1>

      <div class="w-full columns-2 py-4">
          <ButtonLink label="Documentation" size="sm" rounded="md" href="/docs" />
      </div>

      <div class="bg-gray-100 p-6 rounded-lg mb-8">
        <ul class="space-y-2">
          <li><a href="#overview" class="text-blue-600 hover:underline">1. Overview</a></li>
          <li><a href="#custom-services" class="text-blue-600 hover:underline">2. Custom Services</a></li>
          <li><a href="#middleware" class="text-blue-600 hover:underline">3. Middleware</a></li>
          <li><a href="#events" class="text-blue-600 hover:underline">4. Events & Listeners</a></li>
          <li><a href="#resources" class="text-blue-600 hover:underline">5. Custom Resources</a></li>
          <li><a href="#publishing" class="text-blue-600 hover:underline">6. Publishing Packages</a></li>
        </ul>
      </div>

      <section id="overview" class="mb-12">
        <h2 class="text-2xl font-semibold mb-4">1. Overview</h2>
        <p class="text-gray-700 mb-4">
          Shadow is designed to be highly extensible, allowing you to customize and extend its functionality through various extension points. This guide covers the different ways you can extend Shadow to meet your specific needs.
        </p>
      </section>

      <section id="custom-services" class="mb-12">
        <h2 class="text-2xl font-semibold mb-4">2. Custom Services</h2>
        
        <h3 class="text-xl font-semibold mb-4">Service Provider</h3>
        <div class="bg-gray-800 rounded-lg p-4 mb-6">
          <pre class="text-green-400">
namespace App\Providers;

use ThinkNeverland\Shadow\Support\ServiceProvider;

class CustomShadowServiceProvider extends ServiceProvider
{
    public function register()
    {
        $this->app->bind('custom.service', function ($app) {
            return new CustomService($app);
        });
    }

    public function boot()
    {
        $this->registerConfig();
        $this->registerMiddleware();
        $this->registerEvents();
    }
}</pre>
        </div>

        <h3 class="text-xl font-semibold mb-4">Custom Service Implementation</h3>
        <div class="bg-gray-800 rounded-lg p-4 mb-6">
          <pre class="text-green-400">
class CustomService implements CustomServiceInterface
{
    protected $app;

    public function __construct($app)
    {
        $this->app = $app;
    }

    public function customMethod()
    {
        // Custom implementation
    }
}</pre>
        </div>
      </section>

      <section id="middleware" class="mb-12">
        <h2 class="text-2xl font-semibold mb-4">3. Middleware</h2>
        
        <h3 class="text-xl font-semibold mb-4">Custom Middleware</h3>
        <div class="bg-gray-800 rounded-lg p-4 mb-6">
          <pre class="text-green-400">
namespace App\Http\Middleware;

class CustomShadowMiddleware
{
    public function handle($request, Closure $next)
    {
        // Pre-processing
        $response = $next($request);
        // Post-processing
        return $response;
    }
}

// Register middleware
protected $middleware = [
    CustomShadowMiddleware::class,
];

// Register middleware with options
protected $middlewareGroups = [
    'shadow' => [
        CustomShadowMiddleware::class . ':option1,option2',
    ],
];</pre>
        </div>
      </section>

      <section id="events" class="mb-12">
        <h2 class="text-2xl font-semibold mb-4">4. Events & Listeners</h2>
        
        <h3 class="text-xl font-semibold mb-4">Custom Events</h3>
        <div class="bg-gray-800 rounded-lg p-4 mb-6">
          <pre class="text-green-400">
namespace App\Events;

class CustomShadowEvent
{
    public $data;

    public function __construct($data)
    {
        $this->data = $data;
    }
}

// Event listener
class CustomShadowListener
{
    public function handle(CustomShadowEvent $event)
    {
        // Handle the event
    }
}</pre>
        </div>

        <h3 class="text-xl font-semibold mb-4">Event Registration</h3>
        <div class="bg-gray-800 rounded-lg p-4 mb-6">
          <pre class="text-green-400">
protected $listen = [
    CustomShadowEvent::class => [
        CustomShadowListener::class,
    ],
];

// Register in service provider
public function boot()
{
    Event::listen(
        CustomShadowEvent::class,
        [CustomShadowListener::class, 'handle']
    );
}</pre>
        </div>
      </section>

      <section id="resources" class="mb-12">
        <h2 class="text-2xl font-semibold mb-4">5. Custom Resources</h2>
        
        <h3 class="text-xl font-semibold mb-4">Resource Definition</h3>
        <div class="bg-gray-800 rounded-lg p-4 mb-6">
          <pre class="text-green-400">
namespace App\Http\Resources;

use ThinkNeverland\Shadow\Http\Resources\JsonResource;

class CustomResource extends JsonResource
{
    public function toArray($request)
    {
        return [
            'id' => $this->id,
            'custom_field' => $this->custom_field,
            'relationships' => [
                'related' => new RelatedResource($this->related),
            ],
        ];
    }

    public function with($request)
    {
        return [
            'meta' => [
                'custom_meta' => 'value',
            ],
        ];
    }
}</pre>
        </div>
      </section>

      <section id="publishing" class="mb-12">
        <h2 class="text-2xl font-semibold mb-4">6. Publishing Packages</h2>
        
        <h3 class="text-xl font-semibold mb-4">Package Structure</h3>
        <div class="bg-gray-800 rounded-lg p-4 mb-6">
          <pre class="text-green-400">
my-shadow-extension/
├── src/
│   ├── Providers/
│   │   └── ServiceProvider.php
│   ├── Http/
│   │   ├── Middleware/
│   │   └── Resources/
│   └── Services/
├── config/
│   └── my-extension.php
├── composer.json
└── README.md</pre>
        </div>

        <h3 class="text-xl font-semibold mb-4">Composer Configuration</h3>
        <div class="bg-gray-800 rounded-lg p-4 mb-6">
          <pre class="text-green-400">
{
    "name": "vendor/my-shadow-extension",
    "description": "Custom extension for Shadow",
    "type": "library",
    "require": {
        "thinkneverland/shadow": "^1.0"
    },
    "autoload": {
        "psr-4": {
            "Vendor\\MyShadowExtension\\": "src/"
        }
    },
    "extra": {
        "laravel": {
            "providers": [
                "Vendor\\MyShadowExtension\\Providers\\ServiceProvider"
            ]
        }
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