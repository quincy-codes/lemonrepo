<template>
  <div class="max-w-xl mx-auto">
    <UContainer class="max-w-8xl py-12 space-y-6">
      <h1 class="text-4xl sm:text-6xl lg:text-7xl tracking-tight text-gray-800 font-bold text-center font-display max-w-4xl mx-auto">
        Evolve Documentation
      </h1>

      <div class="w-full columns-3 py-4" >
        <ButtonLink label="Core" size="sm" rounded="md" href="/evolve-core-documentation" />
        <ButtonLink label="API" size="sm" rounded="md" href="/evolve-api-documentation" />
        <ButtonLink label="UI" size="sm" rounded="md" href="/evolve-UI-documentation" />
      </div>

      <h3 class="text-center max-w-xl text-2xl sm:text-2xl font-semibold tracking-tight text-gray-900 font-display mb-4 mt-8">
        EvolveCore Extensible Laravel Model Enhancement Package (Foundation / Prerelease)
        </h3>

        <!-- Navigation Buttons -->
        <div class="grid grid-cols-1 md:grid-cols-3 gap-4 mb-12 mt-8">
          <a href="#installation" class="bg-gray-800 text-white px-6 py-3 rounded-lg text-center hover:bg-gray-700">
            Quick Start
          </a>
          <a href="#features" class="bg-gray-800 text-white px-6 py-3 rounded-lg text-center hover:bg-gray-700">
            Features
          </a>
          <a href="#advanced" class="bg-gray-800 text-white px-6 py-3 rounded-lg text-center hover:bg-gray-700">
            Advanced Usage
          </a>
        </div>

        <!-- Table of Contents -->
        <div class="bg-gray-100 p-6 rounded-lg mb-12">
          <h3 class="text-2xl sm:text-2xl font-semibold tracking-tight text-gray-900 font-display mb-4">
            Contents
          </h3>
          <ul class="space-y-2">
            <li><a href="#installation" class="text-blue-600 hover:underline">Installation</a></li>
            <li><a href="#getting-started" class="text-blue-600 hover:underline">Getting Started</a>
              <ul class="ml-4 mt-2 space-y-1">
                <li><a href="#model-setup" class="text-blue-600 hover:underline">Model Configuration</a></li>
                <li><a href="#basic-usage" class="text-blue-600 hover:underline">Basic Usage</a></li>
              </ul>
            </li>
            <li><a href="#features" class="text-blue-600 hover:underline">Core Features</a>
              <ul class="ml-4 mt-2 space-y-1">
                <li><a href="#validation" class="text-blue-600 hover:underline">Validation</a></li>
                <li><a href="#filtering" class="text-blue-600 hover:underline">Filtering</a></li>
                <li><a href="#sorting" class="text-blue-600 hover:underline">Sorting</a></li>
                <li><a href="#events" class="text-blue-600 hover:underline">Events</a></li>
                <li><a href="#policies" class="text-blue-600 hover:underline">Policies</a></li>
              </ul>
            </li>
            <li><a href="#advanced" class="text-blue-600 hover:underline">Advanced Topics</a></li>
          </ul>
        </div>

        <h2 class="text-center mx-auto max-w-xl text-2xl sm:text-2xl font-semibold tracking-tight text-gray-900 font-display mb-4">
          Installation
        </h2>

        <div class="bg-gray-800 text-green-400 p-4 rounded-lg overflow-x-auto mb-8">
          <code>composer require thinkneverland/evolve-core</code>
        </div>

        <h3 class="text-2xl sm:text-2xl font-semibold tracking-tight text-gray-900 font-display mb-4">
          Requirements
        </h3>

        <ul class="list-disc ml-6 mb-8">
          <li>PHP ^7.4|^8.0</li>
          <li>Laravel ^8.0|^9.0</li>
          <li>spatie/laravel-permission ^5.5</li>
          <li>doctrine/dbal ^3.0</li>
        </ul>

        <div class="container mx-auto px-4 py-8 max-w-4xl">
          <h2 class="text-center mx-auto max-w-xl text-2xl sm:text-2xl font-semibold tracking-tight text-gray-900 font-display mb-4">
            Getting Started
          </h2>

          <h3 class="text-2xl sm:text-2xl font-semibold tracking-tight text-gray-900 font-display mb-4">
            Model Configuration
          </h3>

          <div class="bg-gray-800 text-green-400 p-4 rounded-lg overflow-x-auto mb-8">
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
              }</code></div>

          <h3 class="text-2xl sm:text-2xl font-semibold tracking-tight text-gray-900 font-display mb-4">
            Basic Usage
          </h3>

          <div class="bg-gray-800 text-green-400 p-4 rounded-lg overflow-x-auto mb-8">
            <code>// Filtering
              $posts = Post::filter([
              'status' => 'published',
              'author.id' => 1
              ])->get();

              // Sorting
              $posts = Post::evolve(null, ['created_at' => 'desc'])->get();

              // Combined filtering and sorting
              $posts = Post::evolve(
              ['status' => 'published'],
              ['created_at' => 'desc']
              )->get();</code></div>

          <h2 class="text-center mx-auto max-w-xl text-2xl sm:text-2xl font-semibold tracking-tight text-gray-900 font-display mb-4 mt-12">
            Core Features
          </h2>

          <h3 class="text-2xl sm:text-2xl font-semibold tracking-tight text-gray-900 font-display mb-4">
            Validation
          </h3>

          <div class="bg-gray-800 text-green-400 p-4 rounded-lg overflow-x-auto mb-8">
            <code>// Get validation rules
              $rules = Post::getValidationRules();

              // Get rules for specific action
              $updateRules = Post::getValidationRules('update', $post);

              // Custom validation rules example
              public static function validationRules(): array
              {
              return [
              'title' => ['required', 'string', 'max:255'],
              'slug' => ['required', 'string', 'unique:posts,slug'],
              'category_id' => ['required', 'exists:categories,id'],
              'tags' => ['array'],
              'tags.*.id' => ['required', 'exists:tags,id']
              ];
              }</code></div>

          <div class="bg-blue-50 p-6 rounded-lg mb-8">
            <h4 class="font-semibold mb-2">Automatic Features</h4>
            <ul class="list-disc ml-6">
              <li>Database constraint enforcement</li>
              <li>Nested relation validation</li>
              <li>Custom rule integration</li>
              <li>Action-specific validation</li>
            </ul>
          </div>
        </div>
        <div class="container mx-auto px-4 py-8 max-w-4xl">
          <h3 class="text-2xl sm:text-2xl font-semibold tracking-tight text-gray-900 font-display mb-4">
            Filtering
          </h3>

          <div class="bg-gray-800 text-green-400 p-4 rounded-lg overflow-x-auto mb-8">
            <code>// Simple filtering
              $posts = Post::filter([
              'status' => 'published',
              'featured' => true
              ])->get();

              // Relation filtering
              $posts = Post::filter([
              'author.name' => 'John Doe',
              'category.slug' => 'technology'
              ])->get();

              // Deep nested filtering
              $orders = Order::filter([
              'customer.company.country.code' => 'US',
              'items.product.category.name' => 'Electronics'
              ])->get();</code></div>

          <h3 class="text-2xl sm:text-2xl font-semibold tracking-tight text-gray-900 font-display mb-4">
            Sorting
          </h3>

          <div class="bg-gray-800 text-green-400 p-4 rounded-lg overflow-x-auto mb-8">
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

              // String format
              $posts = Post::evolve(null, '-created_at,author.name')->get();</code></div>

          <h3 class="text-2xl sm:text-2xl font-semibold tracking-tight text-gray-900 font-display mb-4">
            Events
          </h3>

          <div class="bg-gray-800 text-green-400 p-4 rounded-lg overflow-x-auto mb-8">
            <code>// In EventServiceProvider
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
              ];

              // Event handler example
              class HandleModelCreated
              {
              public function handle(EvolveModelCreated $event)
              {
              $model = $event->model;
              Log::info("Model created: " . get_class($model) . " #{$model->id}");
              }
              }</code></div>
        </div>
        <div class="container mx-auto px-4 py-8 max-w-4xl">
          <h3 class="text-2xl sm:text-2xl font-semibold tracking-tight text-gray-900 font-display mb-4">
            Policies
          </h3>

          <div class="bg-gray-800 text-green-400 p-4 rounded-lg overflow-x-auto mb-8">
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
              }

              // Custom policy example
              class OrderPolicy extends EvolvePolicy
              {
              public function update(User $user, Order $order)
              {
              if (!$user->hasPermissionTo('update orders')) {
              return false;
              }

              if ($order->status === 'completed') {
              return false;
              }

              return $user->organization_id === $order->organization_id;
              }
              }</code></div>

          <h2 class="text-center mx-auto max-w-xl text-2xl sm:text-2xl font-semibold tracking-tight text-gray-900 font-display mb-4 mt-12">
            Advanced Usage
          </h2>

          <h3 class="text-2xl sm:text-2xl font-semibold tracking-tight text-gray-900 font-display mb-4">
            Complex Model Configuration
          </h3>

          <div class="bg-gray-800 text-green-400 p-4 rounded-lg overflow-x-auto mb-8">
            <code>class Order extends Model implements EvolveModelInterface
              {
              use EvolveModelTrait;

              public static function validationRules(): array
              {
              return [
              'items' => ['required', 'array', 'min:1'],
              'items.*.product_id' => ['required', 'exists:products,id'],
              'shipping_address' => ['required', 'array'],
              'shipping_address.country' => ['required', 'string', 'size:2'],
              'customer_id' => [
              'required',
              'exists:customers,id',
              function ($attribute, $value, $fail) {
              $customer = Customer::find($value);
              if ($customer && !$customer->can_place_orders) {
              $fail('Customer cannot place orders.');
              }
              }
              ]
              ];
              }

              public static function excludedFields(): array
              {
              return ['payment_details', 'internal_notes'];
              }

              public static function excludedRelations(): array
              {
              return ['paymentLogs', 'systemNotes'];
              }
              }</code></div>

          <h3 class="text-2xl sm:text-2xl font-semibold tracking-tight text-gray-900 font-display mb-4">
            Advanced Event Handling
          </h3>

          <div class="bg-gray-800 text-green-400 p-4 rounded-lg overflow-x-auto mb-8">
            <code>class HandleOrderCreated
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
              if (!($event->model instanceof Order)) {
              return;
              }

              $order = $event->model;

              // Update inventory
              foreach ($order->items as $item) {
              $this->inventoryService->decreaseStock(
              $item->product_id,
              $item->quantity
              );
              }

              // Notifications
              $this->notificationService->sendOrderConfirmation($order);

              if ($order->total > 1000) {
              $this->notificationService->notifyAdmins(
              "High-value order #{$order->id} received"
              );
              }
              }
              }</code></div>
        </div>
        <div class="container mx-auto px-4 py-8 max-w-4xl">
          <h2 class="text-center mx-auto max-w-xl text-2xl sm:text-2xl font-semibold tracking-tight text-gray-900 font-display mb-4">
            Best Practices
          </h2>

          <div class="grid grid-cols-1 md:grid-cols-2 gap-6 mb-12">
            <div class="bg-gray-100 p-6 rounded-lg">
              <h3 class="font-semibold mb-3">Model Organization</h3>
              <ul class="list-disc ml-6 space-y-2">
                <li>Group related validation rules</li>
                <li>Keep excluded fields updated</li>
                <li>Document custom implementations</li>
              </ul>
            </div>

            <div class="bg-gray-100 p-6 rounded-lg">
              <h3 class="font-semibold mb-3">Performance</h3>
              <ul class="list-disc ml-6 space-y-2">
                <li>Use eager loading for relations</li>
                <li>Limit nested relation depth</li>
                <li>Index filtered/sorted fields</li>
              </ul>
            </div>
          </div>

          <div class="bg-gray-800 text-green-400 p-4 rounded-lg overflow-x-auto mb-8">
            <code>// Efficient querying example
              $posts = Post::with(['author', 'category'])
              ->filter([
              'status' => 'published',
              'author.role' => 'editor'
              ])
              ->evolve(null, ['created_at' => 'desc'])
              ->paginate(20);</code></div>

          <h3 class="text-2xl sm:text-2xl font-semibold tracking-tight text-gray-900 font-display mb-4">
            Security Considerations
          </h3>

          <div class="bg-yellow-50 border-l-4 border-yellow-400 p-4 mb-8">
            <ul class="list-disc ml-6 space-y-2">
              <li>Always exclude sensitive fields</li>
              <li>Implement proper policy checks</li>
              <li>Validate all user input</li>
              <li>Use custom validation for complex rules</li>
            </ul>
          </div>

          <h2 class="text-center mx-auto max-w-xl text-2xl sm:text-2xl font-semibold tracking-tight text-gray-900 font-display mb-4">
            Resources
          </h2>

          <div class="grid grid-cols-1 md:grid-cols-3 gap-4 mb-8">
            <a href="https://github.com/thinkneverland/evolve-core"
               class="bg-gray-100 p-4 rounded-lg hover:bg-gray-200 text-center">
              GitHub Repository
            </a>
            <a href="https://evolve.thinkneverland.com"
               class="bg-gray-100 p-4 rounded-lg hover:bg-gray-200 text-center">
              Documentation
            </a>
            <a href="https://github.com/thinkneverland/evolve-core/issues"
               class="bg-gray-100 p-4 rounded-lg hover:bg-gray-200 text-center">
              Issue Tracker
            </a>
          </div>

          <div class="bg-blue-50 p-6 rounded-lg">
            <h3 class="font-semibold mb-3">Support</h3>
            <p class="mb-4">For additional support and updates:</p>
            <ul class="list-disc ml-6">
              <li>Join our community on Discord</li>
              <li>Check out the FAQ section</li>
              <li>Subscribe to release notifications</li>
            </ul>
          </div>
        </div>
    </UContainer>
  </div>
</template>