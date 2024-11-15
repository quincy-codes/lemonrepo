<template>
  <div class="max-w-4xl mx-auto">
    <UContainer class="max-w-8xl py-12 space-y-6">
      <h1 class="text-4xl sm:text-6xl lg:text-7xl tracking-tight text-gray-800 font-bold text-center font-display max-w-4xl mx-auto">
        Evolve Documentation
      </h1>

      <div class="w-full columns-3 py-4">
        <ButtonLink label="Core" size="sm" rounded="md" href="/evolve-core-documentation" />
        <ButtonLink label="API" size="sm" rounded="md" href="/evolve-api-documentation" />
        <ButtonLink label="UI" size="sm" rounded="md" href="/evolve-UI-documentation" />
      </div>

      <h3 class="text-center text-2xl sm:text-2xl font-semibold tracking-tight text-gray-900 font-display mb-4 mt-8">
        EvolveCore (Required Foundation)
      </h3>

      <div class="grid grid-cols-1 md:grid-cols-3 gap-6 mb-16">
        <a href="#installation" class="block p-6 bg-white rounded-lg shadow hover:shadow-lg transition-shadow">
          <h3 class="text-xl font-semibold mb-2">Quick Start</h3>
          <p class="text-gray-600">Installation, configuration and core setup</p>
        </a>
        <a href="#features" class="block p-6 bg-white rounded-lg shadow hover:shadow-lg transition-shadow">
          <h3 class="text-xl font-semibold mb-2">Features</h3>
          <p class="text-gray-600">Validation, filtering, sorting and events</p>
        </a>
        <a href="#advanced" class="block p-6 bg-white rounded-lg shadow hover:shadow-lg transition-shadow">
          <h3 class="text-xl font-semibold mb-2">Advanced Usage</h3>
          <p class="text-gray-600">Complex configurations, events and policies</p>
        </a>
      </div>

      <!-- Table of Contents -->
      <div class="bg-gray-100 p-6 rounded-lg mb-12">
        <h3 class="text-2xl sm:text-2xl font-semibold tracking-tight text-gray-900 font-display mb-4">
          Contents
        </h3>
        <ul class="space-y-2">
          <li>
            <a href="#installation" class="text-blue-600 hover:underline">Installation</a>
            <ul class="ml-4 mt-2 space-y-1">
              <li><a href="#github-auth" class="text-blue-600 hover:underline">GitHub Authentication</a></li>
              <li><a href="#composer-setup" class="text-blue-600 hover:underline">Composer Setup</a></li>
              <li><a href="#package-install" class="text-blue-600 hover:underline">Package Installation</a></li>
            </ul>
          </li>
          <li>
            <a href="#getting-started" class="text-blue-600 hover:underline">Getting Started</a>
            <ul class="ml-4 mt-2 space-y-1">
              <li><a href="#model-setup" class="text-blue-600 hover:underline">Model Configuration</a></li>
              <li><a href="#basic-usage" class="text-blue-600 hover:underline">Basic Usage</a></li>
            </ul>
          </li>
          <li>
            <a href="#features" class="text-blue-600 hover:underline">Core Features</a>
            <ul class="ml-4 mt-2 space-y-1">
              <li><a href="#validation" class="text-blue-600 hover:underline">Validation</a></li>
              <li><a href="#filtering" class="text-blue-600 hover:underline">Filtering</a></li>
              <li><a href="#sorting" class="text-blue-600 hover:underline">Sorting</a></li>
              <li><a href="#events" class="text-blue-600 hover:underline">Events</a></li>
              <li><a href="#policies" class="text-blue-600 hover:underline">Policies</a></li>
            </ul>
          </li>
          <li><a href="#advanced" class="text-blue-600 hover:underline">Advanced Usage</a></li>
        </ul>
      </div>

      <h2 id="installation" class="text-center mx-auto max-w-xl text-2xl sm:text-2xl font-semibold tracking-tight text-gray-900 font-display mb-4">
        Installation
      </h2>

      <h3 id="github-auth" class="text-2xl sm:text-2xl font-semibold tracking-tight text-gray-900 font-display mb-4">
        1. Configure GitHub Authentication
      </h3>

      <div class="bg-blue-50 p-6 rounded-lg mb-8">
        <p class="mb-4">Generate a GitHub Personal Access Token with the following scopes:</p>
        <ul class="list-disc ml-6">
          <li>read:packages</li>
          <li>repo</li>
        </ul>
      </div>

      <pre class="bg-gray-800 text-green-400 p-4 rounded-lg overflow-x-auto mb-8">
<code>composer config --global github-oauth.github.com YOUR-TOKEN</code>
      </pre>

      <h3 id="composer-setup" class="text-2xl sm:text-2xl font-semibold tracking-tight text-gray-900 font-display mb-4">
        2. Add Repository to composer.json
      </h3>
      <pre class="bg-gray-800 text-green-400 p-4 rounded-lg overflow-x-auto mb-8">
<code>{
  "repositories": [
    {
      "type": "vcs",
      "url": "https://github.com/thinkneverland/evolve-core"
    }
  ]
}</code>
      </pre>

      <h3 id="package-install" class="text-2xl sm:text-2xl font-semibold tracking-tight text-gray-900 font-display mb-4">
        3. Install Package
      </h3>

      <pre class="bg-gray-800 text-green-400 p-4 rounded-lg overflow-x-auto mb-8">
<code>composer require thinkneverland/evolve-core</code>
      </pre>

      <div class="container mx-auto px-4 py-8 max-w-4xl">
        <h2 id="getting-started" class="text-center mx-auto max-w-xl text-2xl sm:text-2xl font-semibold tracking-tight text-gray-900 font-display mb-4">
          Getting Started
        </h2>

        <div class="bg-yellow-50 border-l-4 border-yellow-400 p-4 mb-8">
          <p class="font-semibold mb-2">Before You Begin:</p>
          <ul class="list-disc ml-6">
            <li>Ensure you have completed the installation steps</li>
            <li>Verify you have the required dependencies installed</li>
            <li>PHP ^7.4|^8.0</li>
            <li>Laravel ^8.0|^9.0</li>
            <li>spatie/laravel-permission ^5.5</li>
            <li>doctrine/dbal ^3.0</li>
          </ul>
        </div>

        <h3 id="model-setup" class="text-2xl sm:text-2xl font-semibold tracking-tight text-gray-900 font-display mb-4">
          Model Configuration
        </h3>

        <pre class="bg-gray-800 text-green-400 p-4 rounded-lg overflow-x-auto mb-8">
<code>namespace App\Models;

use Illuminate\Database\Eloquent\Model;
use Thinkneverland\Evolve\Core\Contracts\EvolveModelInterface;
use Thinkneverland\Evolve\Core\Traits\EvolveModelTrait;

class Post extends Model implements EvolveModelInterface
{
  use EvolveModelTrait;

  public static function excludedFields(): array
  {
    return ['secret_field', 'internal_notes'];
  }

  public static function excludedRelations(): array
  {
    return ['internalComments'];
  }

  public static function validationRules(): array
  {
    return [
      'title' => ['required', 'string', 'max:255'],
      'content' => ['required', 'string']
    ];
  }

  public static function shouldEvolve(): bool
  {
    return true;
  }
}</code>
        </pre>

        <div class="bg-blue-50 p-6 rounded-lg mb-8">
          <h4 class="font-semibold mb-2">Key Configuration Points:</h4>
          <ul class="list-disc ml-6">
            <li>Implement EvolveModelInterface</li>
            <li>Use EvolveModelTrait</li>
            <li>Define excluded fields and relations</li>
            <li>Set up validation rules</li>
            <li>Control evolution status</li>
          </ul>
        </div>

        <h3 id="basic-usage" class="text-2xl sm:text-2xl font-semibold tracking-tight text-gray-900 font-display mb-4">
          Basic Usage
        </h3>

        <pre class="bg-gray-800 text-green-400 p-4 rounded-lg overflow-x-auto mb-8">
<code>// Basic filtering
$posts = Post::filter([
  'status' => 'published',
  'author.id' => 1
])->get();

// Basic sorting
$posts = Post::evolve(null, ['created_at' => 'desc'])->get();

// Combined filtering and sorting
$posts = Post::evolve(
  ['status' => 'published'],
  ['created_at' => 'desc']
)->get();</code>
        </pre>

        <div class="grid grid-cols-1 md:grid-cols-2 gap-6 mb-8">
          <div class="bg-white p-6 rounded-lg shadow">
            <h4 class="font-semibold mb-2">Common Operations</h4>
            <ul class="list-disc ml-6">
              <li>Simple filtering by field</li>
              <li>Relation-based filtering</li>
              <li>Basic sorting</li>
              <li>Combined operations</li>
            </ul>
          </div>
          <div class="bg-white p-6 rounded-lg shadow">
            <h4 class="font-semibold mb-2">Best Practices</h4>
            <ul class="list-disc ml-6">
              <li>Use eager loading for relations</li>
              <li>Keep filters specific</li>
              <li>Combine operations when possible</li>
              <li>Consider pagination for large datasets</li>
            </ul>
          </div>
        </div>
      </div>

      <div class="container mx-auto px-4 py-8 max-w-4xl">
        <h2 id="features" class="text-center mx-auto max-w-xl text-2xl sm:text-2xl font-semibold tracking-tight text-gray-900 font-display mb-4">
          Core Features
        </h2>

        <div class="grid grid-cols-1 md:grid-cols-3 gap-6 mb-16">
          <a href="#validation" class="block p-6 bg-white rounded-lg shadow hover:shadow-lg transition-shadow">
            <h3 class="text-xl font-semibold mb-2">Validation</h3>
            <p class="text-gray-600">Automatic and custom validation rules</p>
          </a>
          <a href="#filtering" class="block p-6 bg-white rounded-lg shadow hover:shadow-lg transition-shadow">
            <h3 class="text-xl font-semibold mb-2">Filtering</h3>
            <p class="text-gray-600">Advanced query filtering capabilities</p>
          </a>
          <a href="#sorting" class="block p-6 bg-white rounded-lg shadow hover:shadow-lg transition-shadow">
            <h3 class="text-xl font-semibold mb-2">Sorting</h3>
            <p class="text-gray-600">Flexible sort operations</p>
          </a>
        </div>

        <h3 id="validation" class="text-2xl sm:text-2xl font-semibold tracking-tight text-gray-900 font-display mb-4">
          Validation System
        </h3>

        <pre class="bg-gray-800 text-green-400 p-4 rounded-lg overflow-x-auto mb-8">
<code>// Get validation rules
$rules = Post::getValidationRules();

// Action-specific validation
$updateRules = Post::getValidationRules('update', $post);

// Custom validation rules
public static function validationRules(): array
{
  return [
    'title' => ['required', 'string', 'max:255'],
    'slug' => ['required', 'string', 'unique:posts,slug'],
    'category_id' => ['required', 'exists:categories,id'],
    'tags' => ['array'],
    'tags.*.id' => ['required', 'exists:tags,id'],
    'content' => ['required', 'string', 'min:100']
  ];
}</code>
        </pre>

        <h3 id="filtering" class="text-2xl sm:text-2xl font-semibold tracking-tight text-gray-900 font-display mb-4">
          Advanced Filtering
        </h3>

        <pre class="bg-gray-800 text-green-400 p-4 rounded-lg overflow-x-auto mb-8">
<code>// Simple filtering
$posts = Post::filter([
  'status' => 'published',
  'featured' => true
])->get();

// Relation filtering
$posts = Post::filter([
  'author.name' => 'John Doe',
  'category.slug' => 'technology',
  'tags.name' => 'Laravel'
])->get();

// Deep nested filtering
$orders = Order::filter([
  'customer.company.country.code' => 'US',
  'items.product.category.name' => 'Electronics'
])->get();</code>
        </pre>

        <div class="bg-blue-50 p-6 rounded-lg mb-8">
          <h4 class="font-semibold mb-2">Filtering Features:</h4>
          <ul class="list-disc ml-6">
            <li>Single field filtering</li>
            <li>Multiple conditions</li>
            <li>Nested relation filtering</li>
            <li>Automatic join handling</li>
            <li>Field exclusion support</li>
          </ul>
        </div>

        <h3 id="sorting" class="text-2xl sm:text-2xl font-semibold tracking-tight text-gray-900 font-display mb-4">
          Sorting Operations
        </h3>

        <pre class="bg-gray-800 text-green-400 p-4 rounded-lg overflow-x-auto mb-8">
<code>// Basic sorting
$posts = Post::evolve(null, ['created_at' => 'desc'])->get();

// Multiple field sorting
$posts = Post::evolve(null, [
  'featured' => 'desc',
  'created_at' => 'desc'
])->get();

// Relation sorting
$posts = Post::evolve(null, [
  'author.name' => 'asc',
  'category.name' => 'desc'
])->get();

// String format sorting
$posts = Post::evolve(null, '-created_at,author.name')->get();</code>
        </pre>

        <div class="grid grid-cols-1 md:grid-cols-2 gap-6 mb-8">
          <div class="bg-white p-6 rounded-lg shadow">
            <h4 class="font-semibold mb-2">Sorting Features</h4>
            <ul class="list-disc ml-6">
              <li>Single/multiple field sorting</li>
              <li>Ascending/descending support</li>
              <li>Relation-based sorting</li>
              <li>String format syntax</li>
            </ul>
          </div>
          <div class="bg-white p-6 rounded-lg shadow">
            <h4 class="font-semibold mb-2">Best Practices</h4>
            <ul class="list-disc ml-6">
              <li>Index sorted fields</li>
              <li>Limit relation depth</li>
              <li>Use efficient sort combinations</li>
              <li>Consider performance impact</li>
            </ul>
          </div>
        </div>
      </div>

      <div class="container mx-auto px-4 py-8 max-w-4xl">
        <h3 id="events" class="text-2xl sm:text-2xl font-semibold tracking-tight text-gray-900 font-display mb-4">
          Event System
        </h3>

        <div class="grid grid-cols-1 md:grid-cols-2 gap-6 mb-8">
          <div class="bg-white p-6 rounded-lg shadow">
            <h4 class="font-semibold mb-2">Available Events</h4>
            <ul class="list-disc ml-6">
              <li>EvolveModelCreated</li>
              <li>EvolveModelUpdated</li>
              <li>EvolveModelDeleted</li>
              <li>EvolveModelRestored</li>
            </ul>
          </div>
          <div class="bg-white p-6 rounded-lg shadow">
            <h4 class="font-semibold mb-2">Event Features</h4>
            <ul class="list-disc ml-6">
              <li>Automatic dispatch</li>
              <li>Model instance access</li>
              <li>Custom event handling</li>
              <li>Queueable listeners</li>
            </ul>
          </div>
        </div>

        <pre class="bg-gray-800 text-green-400 p-4 rounded-lg overflow-x-auto mb-8">
<code>// In EventServiceProvider.php
protected $listen = [
  EvolveModelCreated::class => [
    HandleModelCreated::class,
  ],
  EvolveModelUpdated::class => [
    HandleModelUpdated::class,
  ],
  EvolveModelDeleted::class => [
    HandleModelDeleted::class,
  ],
  EvolveModelRestored::class => [
    HandleModelRestored::class,
  ],
];</code>
        </pre>

        <pre class="bg-gray-800 text-green-400 p-4 rounded-lg overflow-x-auto mb-8">
<code>// Example event handler
class HandleModelCreated
{
  public function handle(EvolveModelCreated $event)
  {
    $model = $event->model;

    if ($model instanceof Order) {
      // Handle order creation
      $this->processOrder($model);
    }

    // Log the event
    Log::info("Model created: " . get_class($model) . " #{$model->id}");
  }
}</code>
        </pre>

        <h3 id="policies" class="text-2xl sm:text-2xl font-semibold tracking-tight text-gray-900 font-display mb-4">
          Authorization Policies
        </h3>

        <div class="bg-blue-50 p-6 rounded-lg mb-8">
          <h4 class="font-semibold mb-2">Default Policy Actions:</h4>
          <ul class="list-disc ml-6">
            <li>viewAny - List/index permission</li>
            <li>view - Single record view permission</li>
            <li>create - Creation permission</li>
            <li>update - Modification permission</li>
            <li>delete - Deletion permission</li>
          </ul>
        </div>

        <pre class="bg-gray-800 text-green-400 p-4 rounded-lg overflow-x-auto mb-8">
<code>// Default policy implementation
class EvolvePolicy
{
  public function viewAny(User $user)
  {
    return $user->hasPermissionTo('view any');
  }

  public function view(User $user, $model)
  {
    return $user->hasPermissionTo('view');
  }

  public function create(User $user)
  {
    return $user->hasPermissionTo('create');
  }

  public function update(User $user, $model)
  {
    return $user->hasPermissionTo('update');
  }
}</code>
        </pre>

        <h4 class="text-xl font-semibold mb-4">Custom Policy Example</h4>

        <pre class="bg-gray-800 text-green-400 p-4 rounded-lg overflow-x-auto mb-8">
<code>class OrderPolicy extends EvolvePolicy
{
  public function update(User $user, Order $order)
  {
    // Basic permission check
    if (!$user->hasPermissionTo('update orders')) {
      return false;
    }

    // Order-specific rules
    if ($order->status === 'completed') {
      return false;
    }

    // Organization check
    return $user->organization_id === $order->organization_id;
  }

  public function delete(User $user, Order $order)
  {
    // Only admins can delete orders
    return $user->hasRole('admin');
  }
}</code>
        </pre>

        <div class="grid grid-cols-1 md:grid-cols-2 gap-6 mb-8">
          <div class="bg-white p-6 rounded-lg shadow">
            <h4 class="font-semibold mb-2">Policy Features</h4>
            <ul class="list-disc ml-6">
              <li>Role-based authorization</li>
              <li>Permission integration</li>
              <li>Custom logic support</li>
              <li>Automatic registration</li>
            </ul>
          </div>
          <div class="bg-white p-6 rounded-lg shadow">
            <h4 class="font-semibold mb-2">Best Practices</h4>
            <ul class="list-disc ml-6">
              <li>Use specific permissions</li>
              <li>Implement business rules</li>
              <li>Keep policies focused</li>
              <li>Document custom logic</li>
            </ul>
          </div>
        </div>
      </div>

      <div class="container mx-auto px-4 py-8 max-w-4xl">
        <h2 id="advanced" class="text-center mx-auto max-w-xl text-2xl sm:text-2xl font-semibold tracking-tight text-gray-900 font-display mb-4">
          Advanced Usage
        </h2>

        <div class="grid grid-cols-1 md:grid-cols-3 gap-6 mb-16">
          <a href="#complex-config" class="block p-6 bg-white rounded-lg shadow hover:shadow-lg transition-shadow">
            <h3 class="text-xl font-semibold mb-2">Complex Configuration</h3>
            <p class="text-gray-600">Advanced model setup and validation</p>
          </a>
          <a href="#advanced-events" class="block p-6 bg-white rounded-lg shadow hover:shadow-lg transition-shadow">
            <h3 class="text-xl font-semibold mb-2">Advanced Events</h3>
            <p class="text-gray-600">Complex event handling scenarios</p>
          </a>
          <a href="#complex-queries" class="block p-6 bg-white rounded-lg shadow hover:shadow-lg transition-shadow">
            <h3 class="text-xl font-semibold mb-2">Complex Queries</h3>
            <p class="text-gray-600">Advanced filtering and sorting</p>
          </a>
        </div>

        <h3 id="complex-config" class="text-2xl sm:text-2xl font-semibold tracking-tight text-gray-900 font-display mb-4">
          Complex Model Configuration
        </h3>

        <pre class="bg-gray-800 text-green-400 p-4 rounded-lg overflow-x-auto mb-8">
<code>class Order extends Model implements EvolveModelInterface
{
  use EvolveModelTrait;

  public static function validationRules(): array
  {
    return [
      'items' => ['required', 'array', 'min:1'],
      'items.*.product_id' => ['required', 'exists:products,id'],
      'items.*.quantity' => ['required', 'integer', 'min:1'],
      'shipping_address' => ['required', 'array'],
      'shipping_address.street' => ['required', 'string'],
      'shipping_address.city' => ['required', 'string'],
      'shipping_address.country' => ['required', 'string', 'size:2'],
      'payment_method' => ['required', 'in:credit_card,paypal,bank_transfer'],
      'customer_id' => [
        'required',
        'exists:customers,id',
        function ($attribute, $value, $fail) {
          $customer = Customer::find($value);
          if ($customer && !$customer->can_place_orders) {
            $fail('This customer is not allowed to place orders.');
          }
        }
      ]
    ];
  }

  public static function uniqueFields(): array
  {
    return ['order_number', 'transaction_id'];
  }

  public static function excludedFields(): array
  {
    return [
      'payment_details',
      'internal_notes',
      'risk_score'
    ];
  }
}</code>
        </pre>

        <h3 id="advanced-events" class="text-2xl sm:text-2xl font-semibold tracking-tight text-gray-900 font-display mb-4">
          Advanced Event Handling
        </h3>

        <pre class="bg-gray-800 text-green-400 p-4 rounded-lg overflow-x-auto mb-8">
<code>class HandleOrderCreated
{
  protected $notificationService;
  protected $inventoryService;
  protected $analyticsService;

  public function handle(EvolveModelCreated $event)
  {
    if (!($event->model instanceof Order)) {
      return;
    }

    $order = $event->model;

    // Process inventory
    collect($order->items)->each(function($item) {
      $this->inventoryService->decreaseStock(
        $item->product_id,
        $item->quantity
      );
    });

    // Send notifications
    $this->notificationService->sendOrderConfirmation($order);

    if ($order->total > 1000) {
      $this->notificationService->notifyAdmins(
        "High-value order #{$order->id} received"
      );
    }

    // Track analytics
    $this->analyticsService->trackOrder($order);
  }
}</code>
        </pre>

        <h3 id="complex-queries" class="text-2xl sm:text-2xl font-semibold tracking-tight text-gray-900 font-display mb-4">
          Complex Query Operations
        </h3>

        <pre class="bg-gray-800 text-green-400 p-4 rounded-lg overflow-x-auto mb-8">
          <code>// Complex filtering with nested relations
$orders = Order::with(['items.product', 'customer'])
    ->filter([
        'status' => 'processing',
        'customer.type' => 'vip',
        'items.product.category.name' => 'Electronics',
        'created_at' => Carbon::today(),
        'total_amount' => 1000
    ])
    ->evolve(null, [
        'created_at' => 'desc',
        'customer.name' => 'asc'
    ])
    ->paginate(20);

// Advanced sorting with multiple conditions
$posts = Post::evolve(
    ['status' => 'published'],
    [
        'featured' => 'desc',
        'category.priority' => 'asc',
        'author.reputation' => 'desc',
        'created_at' => 'desc'
    ]
)->get();
          </code>
        </pre>

        <div class="bg-yellow-50 border-l-4 border-yellow-400 p-4 mb-8">
          <h4 class="font-semibold mb-2">Performance Considerations:</h4>
          <ul class="list-disc ml-6">
            <li>Use eager loading for nested relations</li>
            <li>Index frequently filtered/sorted columns</li>
            <li>Implement caching for complex queries</li>
            <li>Monitor query execution time</li>
            <li>Consider chunking for large datasets</li>
          </ul>
        </div>
      </div>

      <div class="container mx-auto px-4 py-8 max-w-4xl">
        <h2 class="text-center mx-auto max-w-xl text-2xl sm:text-2xl font-semibold tracking-tight text-gray-900 font-display mb-4">
          Troubleshooting & Resources
        </h2>

        <div class="grid grid-cols-1 md:grid-cols-3 gap-6 mb-16">
          <a href="#common-issues" class="block p-6 bg-white rounded-lg shadow hover:shadow-lg transition-shadow">
            <h3 class="text-xl font-semibold mb-2">Common Issues</h3>
            <p class="text-gray-600">Solutions to frequent problems</p>
          </a>
          <a href="#performance" class="block p-6 bg-white rounded-lg shadow hover:shadow-lg transition-shadow">
            <h3 class="text-xl font-semibold mb-2">Performance</h3>
            <p class="text-gray-600">Optimization guidelines</p>
          </a>
          <a href="#resources" class="block p-6 bg-white rounded-lg shadow hover:shadow-lg transition-shadow">
            <h3 class="text-xl font-semibold mb-2">Resources</h3>
            <p class="text-gray-600">Additional help and support</p>
          </a>
        </div>

        <h3 id="common-issues" class="text-2xl sm:text-2xl font-semibold tracking-tight text-gray-900 font-display mb-4">
          Common Issues
        </h3>

        <div class="bg-white rounded-lg shadow overflow-hidden mb-8">
          <div class="border-b">
            <div class="p-4">
              <h4 class="font-semibold mb-2">Package Installation Fails</h4>
              <pre class="bg-gray-800 text-green-400 p-4 rounded-lg overflow-x-auto mb-4">
<code>composer config --global github-oauth.github.com YOUR-TOKEN
composer clear-cache
composer update</code>
              </pre>
              <p class="text-gray-600">Ensure your GitHub token has the correct permissions and your composer.json is properly configured.</p>
            </div>
          </div>
          <div class="border-b">
            <div class="p-4">
              <h4 class="font-semibold mb-2">Validation Rules Not Working</h4>
              <pre class="bg-gray-800 text-green-400 p-4 rounded-lg overflow-x-auto mb-4">
<code>// Check implementation
class YourModel extends Model implements EvolveModelInterface
{
    use EvolveModelTrait;

    public static function validationRules(): array
    {
        return [
            // Your rules here
        ];
    }
}
                </code>
              </pre>
            </div>
          </div>
          <div class="p-4">
            <h4 class="font-semibold mb-2">Relationship Filtering Issues</h4>
            <pre class="bg-gray-800 text-green-400 p-4 rounded-lg overflow-x-auto mb-4">
<code>// Ensure proper relationship definition
public function author()
{
  return $this->belongsTo(User::class);
}

// Use proper dot notation
Post::filter(['author.name' => 'John'])->get();</code>
            </pre>
          </div>
        </div>

        <h3 id="performance" class="text-2xl sm:text-2xl font-semibold tracking-tight text-gray-900 font-display mb-4">
          Performance Optimization
        </h3>

        <div class="bg-blue-50 p-6 rounded-lg mb-8">
          <h4 class="font-semibold mb-2">Best Practices:</h4>
          <div class="grid grid-cols-1 md:grid-cols-2 gap-4">
            <ul class="list-disc ml-6">
              <li>Use selective eager loading</li>
              <li>Implement database indexes</li>
              <li>Cache frequently accessed data</li>
              <li>Use chunking for large datasets</li>
            </ul>
            <ul class="list-disc ml-6">
              <li>Optimize validation rules</li>
              <li>Minimize nested relations depth</li>
              <li>Monitor query performance</li>
              <li>Use appropriate field types</li>
            </ul>
          </div>
        </div>

        <h3 id="resources" class="text-2xl sm:text-2xl font-semibold tracking-tight text-gray-900 font-display mb-4">
          Additional Resources
        </h3>

        <div class="grid grid-cols-1 md:grid-cols-2 gap-6 mb-8">
          <div class="bg-white p-6 rounded-lg shadow">
            <h4 class="font-semibold mb-2">Support Channels</h4>
            <ul class="list-disc ml-6">
              <li><a href="https://github.com/thinkneverland/evolve-core/issues" class="text-blue-600 hover:underline">GitHub Issues</a></li>
              <li><a href="https://evolve.thinkneverland.com" class="text-blue-600 hover:underline">Official Documentation</a></li>
              <li>Support Email: <a href="mailto:hello@thinkneverland.com">support@thinkneverland.com</a></li>
            </ul>
          </div>
          <div class="bg-white p-6 rounded-lg shadow">
            <h4 class="font-semibold mb-2">Contributing</h4>
            <ul class="list-disc ml-6">
              <li><a href="mailto:hello@thinkneverland.com">Submit bug reports</a></li>
              <li><a href="mailto:hello@thinkneverland.com">Propose new features</a></li>
              <li><a href="mailto:hello@thinkneverland.com">Create pull requests</a></li>
              <li><a href="mailto:hello@thinkneverland.com">Share your experience</a></li>
            </ul>
          </div>
        </div>

        <div class="bg-gray-100 p-6 rounded-lg text-center">
          <p class="mb-4">Need additional help? Contact us at:</p>
          <a href="mailto:hello@thinkneverland.com" class="text-blue-600 hover:underline">hello@thinkneverland.com</a>
        </div>
      </div>
    </UContainer>
  </div>
</template>

<script setup lang="ts">
</script>