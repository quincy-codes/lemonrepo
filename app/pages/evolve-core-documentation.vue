<template>
  <div class="max-w-xl mx-auto">
    <UContainer class="max-w-8xl py-12 space-y-6">

        <h1 class="text-4xl sm:text-6xl lg:text-7xl tracking-tight text-gray-800 font-bold text-center font-display max-w-4xl mx-auto">
          Evolve Documentation
        </h1>

      <div class="w-full columns-3 py-4" >
        <ButtonLink label="Core" size="sm" rounded="sm" href="/evolve-core-documentation" />
        <ButtonLink label="API" size="sm" rounded="sm" href="/evolve-api-documentation" />
        <ButtonLink label="UI" size="sm" rounded="sm" href="/evolve-UI-documentation" />
      </div>

        <h3 class="max-w-xl text-2xl sm:text-2xl font-semibold tracking-tight text-gray-900 font-display mb-4 mt-8">
          EvolveCore (Foundation / Prerelease)
        </h3>

        <h3 class="max-w-xl text-2xl sm:text-2xl font-semibold tracking-tight text-gray-900 font-display mb-4 mt-8">
          Table of Contents
        </h3>

        <div class="bg-gray-100 p-6 rounded-lg mb-8">
          <ul class="space-y-2">
            <li><a href="#installation" class="text-blue-600 hover:underline">1. Installation</a></li>
            <li><a href="#overview" class="text-blue-600 hover:underline">2. Overview</a></li>
            <li>
              <a href="#getting-started" class="text-blue-600 hover:underline">3. Getting Started</a>
              <ul class="ml-4 mt-2">
                <li><a href="#model-setup" class="text-blue-600 hover:underline">3.1 Model Setup</a></li>
                <li><a href="#basic-usage" class="text-blue-600 hover:underline">3.2 Basic Usage</a></li>
              </ul>
            </li>
            <li>
              <a href="#features" class="text-blue-600 hover:underline">4. Features</a>
              <ul class="ml-4 mt-2">
                <li><a href="#validation" class="text-blue-600 hover:underline">4.1 Validation</a></li>
                <li><a href="#filtering" class="text-blue-600 hover:underline">4.2 Filtering</a></li>
                <li><a href="#sorting" class="text-blue-600 hover:underline">4.3 Sorting</a></li>
                <li><a href="#events" class="text-blue-600 hover:underline">4.4 Events</a></li>
                <li><a href="#policies" class="text-blue-600 hover:underline">4.5 Policies</a></li>
              </ul>
            </li>
            <li><a href="#advanced" class="text-blue-600 hover:underline">5. Advanced Usage</a></li>
          </ul>
        </div>

        <h2 class="text-center mx-auto max-w-xl text-2xl sm:text-2xl font-semibold tracking-tight text-gray-900 font-display mb-4 mt-24">
          1. Installation
        </h2>

        <p class="mb-4">Install EvolveCore using Composer:</p>
        <pre class="bg-gray-800 text-white p-4 rounded-lg overflow-x-auto mb-4"><code class="text-green-400">composer require thinkneverland/evolve-core</code></pre>

        <p class="mb-4">The package requires:</p>
        <ul class="list-disc ml-6 mb-4">
          <li>PHP ^7.4|^8.0</li>
          <li>Laravel ^8.0|^9.0</li>
          <li>spatie/laravel-permission ^5.5</li>
          <li>doctrine/dbal ^3.0</li>
        </ul>

        <p class="mb-4">The service provider will be automatically registered. If you need to register it manually, add to config/app.php:</p>
        <pre class="bg-gray-800 text-white p-4 rounded-lg overflow-x-auto"><code class="text-green-400">'providers' => [
    // ...
    Thinkneverland\Evolve\Core\EvolveCoreServiceProvider::class,
];</code></pre>

        <h2 class="text-center mx-auto max-w-xl text-2xl sm:text-2xl font-semibold tracking-tight text-gray-900 font-display mb-4 mt-24">
          2. Overview
        </h2>

        <p class="mb-4">EvolveCore provides enhanced functionality for Laravel models including:</p>
        <ul class="list-disc ml-6 mb-4">
          <li>Automatic validation based on database schema and custom rules</li>
          <li>Advanced filtering capabilities with nested relation support</li>
          <li>Flexible sorting functionality</li>
          <li>Built-in event handling</li>
          <li>Integrated permission management</li>
        </ul>

        <h2 class="text-center mx-auto max-w-xl text-2xl sm:text-2xl font-semibold tracking-tight text-gray-900 font-display mb-4 mt-24">
          3. Getting Started
        </h2>

        <h3 class="max-w-xl text-2xl sm:text-2xl font-semibold tracking-tight text-gray-900 font-display mb-4 mt-8">
          3.1 Model Setup
        </h3>

        <p class="mb-4">To use EvolveCore in your models, implement the interface and use the trait:</p>
        <pre class="bg-gray-800 text-white p-4 rounded-lg overflow-x-auto mb-4"><code class="text-green-400">namespace App\Models;

use Illuminate\Database\Eloquent\Model;
use Thinkneverland\Evolve\Core\Contracts\EvolveModelInterface;
use Thinkneverland\Evolve\Core\Traits\EvolveModelTrait;

class Post extends Model implements EvolveModelInterface
{
    use EvolveModelTrait;

    // Define fields that should be excluded from operations
    public static function excludedFields(): array
    {
        return ['secret_field', 'internal_notes'];
    }

    // Define relations that should be excluded
    public static function excludedRelations(): array
    {
        return ['internalComments'];
    }

    // Define custom validation rules
    public static function validationRules(): array
    {
        return [
            'title' => ['required', 'string', 'max:255'],
            'content' => ['required', 'string']
        ];
    }

    // Control if model should be included in evolution
    public static function shouldEvolve(): bool
    {
        return true;
    }
}</code></pre>

        <h3 class="max-w-xl text-2xl sm:text-2xl font-semibold tracking-tight text-gray-900 font-display mb-4 mt-8">
          3.2 Basic Usage
        </h3>

        <p class="mb-4">Once your model is set up, you can use the enhanced functionality:</p>
        <pre class="bg-gray-800 text-white p-4 rounded-lg overflow-x-auto mb-4"><code class="text-green-400">// Basic filtering
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
)->get();</code></pre>

        <h2 class="text-center mx-auto max-w-xl text-2xl sm:text-2xl font-semibold tracking-tight text-gray-900 font-display mb-4 mt-24">
          4. Features
        </h2>

        <h3 class="max-w-xl text-2xl sm:text-2xl font-semibold tracking-tight text-gray-900 font-display mb-4 mt-8">
          4.1 Validation
        </h3>

        <p class="mb-4">EvolveCore provides automatic validation based on your database schema and custom rules:</p>
        <pre class="bg-gray-800 text-white p-4 rounded-lg overflow-x-auto mb-4"><code class="text-green-400">// Get validation rules for a model
$rules = Post::getValidationRules();

// Get validation rules for specific action
$updateRules = Post::getValidationRules('update', $post);

// Example of custom validation rules in your model
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
}</code></pre>

        <p class="mb-4">The validation system automatically:</p>
        <ul class="list-disc ml-6 mb-4">
          <li>Enforces database constraints (nullable, unique, foreign keys)</li>
          <li>Handles nested relations validation</li>
          <li>Merges custom rules with database rules</li>
        </ul>

        <h3 class="max-w-xl text-2xl sm:text-2xl font-semibold tracking-tight text-gray-900 font-display mb-4 mt-8">
          4.2 Filtering
        </h3>

        <p class="mb-4">The filtering system supports simple and complex queries:</p>
        <pre class="bg-gray-800 text-white p-4 rounded-lg overflow-x-auto mb-4"><code class="text-green-400">// Simple filtering
$posts = Post::filter([
    'status' => 'published',
    'featured' => true
])->get();

// Filtering with relations
$posts = Post::filter([
    'author.name' => 'John Doe',
    'category.slug' => 'technology',
    'tags.name' => 'Laravel'
])->get();

// Complex filtering
$posts = Post::filter([
    'created_at' => '2024-01-01',
    'author.role.name' => 'admin',
    'comments.rating' => 5
])->get();</code></pre>

        <p class="mb-4">To exclude fields from filtering:</p>
        <pre class="bg-gray-800 text-white p-4 rounded-lg overflow-x-auto mb-4"><code class="text-green-400">public static function excludedFields(): array
{
    return ['internal_notes', 'secret_key'];
}

public static function excludedRelations(): array
{
    return ['internalComments', 'systemLogs'];
}</code></pre>

        <h3 class="max-w-xl text-2xl sm:text-2xl font-semibold tracking-tight text-gray-900 font-display mb-4 mt-8">
          4.3 Sorting
        </h3>

        <p class="mb-4">Multiple sorting methods are supported:</p>
        <pre class="bg-gray-800 text-white p-4 rounded-lg overflow-x-auto mb-4"><code class="text-green-400">// Simple sorting
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
$posts = Post::evolve(null, '-created_at,author.name')->get();

// Combined filtering and sorting
$posts = Post::evolve(
    ['status' => 'published'],
    ['comments_count' => 'desc']
)->get();</code></pre>

        <h3 class="max-w-xl text-2xl sm:text-2xl font-semibold tracking-tight text-gray-900 font-display mb-4 mt-8">
          4.4 Events
        </h3>

        <p class="mb-4">EvolveCore automatically dispatches events for model operations:</p>
        <pre class="bg-gray-800 text-white p-4 rounded-lg overflow-x-auto mb-4"><code class="text-green-400">// Listen for events in your EventServiceProvider
use Thinkneverland\Evolve\Core\Events\EvolveModelCreated;
use Thinkneverland\Evolve\Core\Events\EvolveModelUpdated;
use Thinkneverland\Evolve\Core\Events\EvolveModelDeleted;
use Thinkneverland\Evolve\Core\Events\EvolveModelRestored;

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
];</code></pre>

        <p class="mb-4">Example event handler:</p>
        <pre class="bg-gray-800 text-white p-4 rounded-lg overflow-x-auto mb-4"><code class="text-green-400">class HandleModelCreated
{
    public function handle(EvolveModelCreated $event)
    {
        $model = $event->model;
        // Handle the event
        Log::info("Model created: " . get_class($model) . " #{$model->id}");
    }
}</code></pre>

      <div class="container mx-auto px-4 py-8 max-w-4xl">
        <h3 class="max-w-xl text-2xl sm:text-2xl font-semibold tracking-tight text-gray-900 font-display mb-4 mt-8">
          4.5 Policies
        </h3>

        <p class="mb-4">EvolveCore automatically registers policies for your models:</p>
        <pre class="bg-gray-800 text-white p-4 rounded-lg overflow-x-auto mb-4"><code class="text-green-400">// The default policy checks permissions using spatie/laravel-permission
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

public function delete(User $user, $model)
{
    return $user->hasPermissionTo('delete');
}</code></pre>

        <p class="mb-4">To customize permissions, extend the EvolvePolicy:</p>
        <pre class="bg-gray-800 text-white p-4 rounded-lg overflow-x-auto mb-4"><code class="text-green-400">namespace App\Policies;

use Thinkneverland\Evolve\Core\Policies\EvolvePolicy;

class PostPolicy extends EvolvePolicy
{
    public function update(User $user, Post $post)
    {
        return $user->hasPermissionTo('update') && $post->user_id === $user->id;
    }
}</code></pre>

        <h2 class="text-center mx-auto max-w-xl text-2xl sm:text-2xl font-semibold tracking-tight text-gray-900 font-display mb-4 mt-24">
          5. Advanced Usage
        </h2>

        <h3 class="max-w-xl text-2xl sm:text-2xl font-semibold tracking-tight text-gray-900 font-display mb-4 mt-8">
          5.1 Complex Filtering Scenarios
        </h3>

        <pre class="bg-gray-800 text-white p-4 rounded-lg overflow-x-auto mb-4"><code class="text-green-400">// Deep nested relations
$orders = Order::filter([
    'customer.company.country.code' => 'US',
    'items.product.category.name' => 'Electronics',
    'payments.status' => 'completed'
])->get();

// Multiple conditions on related models
$posts = Post::evolve(
    [
        'category.type' => 'featured',
        'tags.name' => 'important',
        'author.role.name' => 'editor'
    ],
    ['published_at' => 'desc']
)->get();</code></pre>

        <h3 class="max-w-xl text-2xl sm:text-2xl font-semibold tracking-tight text-gray-900 font-display mb-4 mt-8">
          5.2 Custom Validation Implementation
        </h3>

        <pre class="bg-gray-800 text-white p-4 rounded-lg overflow-x-auto mb-4"><code class="text-green-400">class Order extends Model implements EvolveModelInterface
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

    // Custom fields to exclude from operations
    public static function excludedFields(): array
    {
        return [
            'payment_details',
            'internal_notes',
            'risk_score'
        ];
    }

    // Exclude sensitive relations
    public static function excludedRelations(): array
    {
        return [
            'paymentLogs',
            'systemNotes',
            'internalAudits'
        ];
    }
}</code></pre>

        <h3 class="max-w-xl text-2xl sm:text-2xl font-semibold tracking-tight text-gray-900 font-display mb-4 mt-8">
          5.3 Advanced Event Handling
        </h3>

        <pre class="bg-gray-800 text-white p-4 rounded-lg overflow-x-auto mb-4"><code class="text-green-400">// Complex event handler example
class HandleOrderCreated
{
    protected $notificationService;
    protected $inventoryService;

    public function __construct(
        NotificationService $notificationService,
        InventoryService $inventoryService
    ) {
        $this->notificationService = $notificationService;
        $this->inventoryService = $inventoryService;
    }

    public function handle(EvolveModelCreated $event)
    {
        $order = $event->model;

        // Only handle Order models
        if (!($order instanceof Order)) {
            return;
        }

        // Update inventory
        foreach ($order->items as $item) {
            $this->inventoryService->decreaseStock(
                $item->product_id,
                $item->quantity
            );
        }

        // Send notifications
        $this->notificationService->sendOrderConfirmation($order);

        // Notify admins if high-value order
        if ($order->total > 1000) {
            $this->notificationService->notifyAdmins(
                "High-value order #{$order->id} received"
            );
        }
    }
}</code></pre>
      </div>
      <div class="container mx-auto px-4 py-8 max-w-4xl">
        <h3 class="max-w-xl text-2xl sm:text-2xl font-semibold tracking-tight text-gray-900 font-display mb-4 mt-8">
          5.4 Advanced Policy Implementation
        </h3>

        <pre class="bg-gray-800 text-white p-4 rounded-lg overflow-x-auto mb-4"><code class="text-green-400">class OrderPolicy extends EvolvePolicy
{
    public function view(User $user, Order $order)
    {
        // Basic permission check
        if (!$user->hasPermissionTo('view orders')) {
            return false;
        }

        // Allow if user is admin
        if ($user->hasRole('admin')) {
            return true;
        }

        // Allow if user is the customer
        if ($user->id === $order->customer_id) {
            return true;
        }

        // Allow if user is in the same organization
        if ($user->organization_id === $order->organization_id) {
            return true;
        }

        return false;
    }

    public function update(User $user, Order $order)
    {
        // Basic permission check
        if (!$user->hasPermissionTo('update orders')) {
            return false;
        }

        // Only allow updates for non-completed orders
        if ($order->status === 'completed') {
            return false;
        }

        // Additional business logic...
        return true;
    }

    public function delete(User $user, Order $order)
    {
        // Only admins can delete orders
        return $user->hasRole('admin');
    }
}</code></pre>

        <h2 class="text-center mx-auto max-w-xl text-2xl sm:text-2xl font-semibold tracking-tight text-gray-900 font-display mb-4 mt-24">
          Additional Resources
        </h2>

        <p class="mb-4">For more information and support:</p>
        <ul class="list-disc ml-6 mb-4">
          <li>GitHub Repository: <a href="https://github.com/thinkneverland/evolve-core" class="text-blue-600 hover:underline">thinkneverland/evolve-core</a></li>
          <li>Documentation: <a href="https://evolve.thinkneverland.com" class="text-blue-600 hover:underline">evolve.thinkneverland.com</a></li>
          <li>Issues & Bug Reports: <a href="https://github.com/thinkneverland/evolve-core/issues" class="text-blue-600 hover:underline">GitHub Issues</a></li>
        </ul>

        <h3 class="max-w-xl text-2xl sm:text-2xl font-semibold tracking-tight text-gray-900 font-display mb-4 mt-8">
          Requirements Summary
        </h3>

        <ul class="list-disc ml-6 mb-4">
          <li>PHP 7.4 or higher</li>
          <li>Laravel 8.x or 9.x</li>
          <li>Spatie Laravel Permission package</li>
          <li>Doctrine DBAL</li>
        </ul>

        <div class="bg-blue-50 p-6 rounded-lg mt-8">
          <h3 class="max-w-xl text-xl font-semibold tracking-tight text-blue-900 font-display mb-2">
            Note
          </h3>
          <p class="text-blue-800">
            EvolveCore is designed to be extensible. While it provides robust default implementations, you can override any of its components to match your specific requirements. Always refer to the latest documentation for updates and best practices.
          </p>
        </div>
      </div>
    </UContainer>
  </div>
</template>