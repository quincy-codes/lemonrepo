<template>
  <div>
    <div class="container mx-auto px-4 py-12">
      <h1 class="text-4xl font-bold text-center mb-8">Extending Shadow</h1>
      
      <p class="text-xl text-gray-600 text-center max-w-3xl mx-auto mb-12">
        Shadow is designed to be highly extensible. This guide covers the various ways you can extend and customize Shadow to fit your specific needs.
      </p>
      
      <div class="bg-gray-100 p-6 rounded-lg mb-8">
        <ul class="space-y-2">
          <li><a href="#custom-handlers" class="text-blue-600 hover:underline">1. Custom Request Handlers</a></li>
          <li><a href="#middleware" class="text-blue-600 hover:underline">2. Custom Middleware</a></li>
          <li><a href="#transformers" class="text-blue-600 hover:underline">3. Custom Transformers</a></li>
          <li><a href="#validators" class="text-blue-600 hover:underline">4. Custom Validators</a></li>
          <li><a href="#events" class="text-blue-600 hover:underline">5. Events & Listeners</a></li>
        </ul>
      </div>
      
      <section id="custom-handlers" class="mb-12">
        <h2 class="text-2xl font-semibold mb-4">1. Custom Request Handlers</h2>
        
        <p class="mb-4">
          Shadow allows you to create custom request handlers to implement specialized logic for your API endpoints.
        </p>
        
        <h3 class="text-xl font-semibold mb-2">Creating a Custom Handler</h3>
        <div class="bg-slate-800 rounded-lg p-4 mb-6">
          <pre class="text-green-400">
namespace App\Shadow\Handlers;

use ThinkNeverland\Shadow\Handlers\BaseHandler;
use Illuminate\Http\Request;

class CustomReportHandler extends BaseHandler
{
    public function handle(Request $request)
    {
        // Your custom logic here
        $data = $this->generateReportData($request);
        
        return response()->json([
            'success' => true,
            'data' => $data
        ]);
    }
    
    protected function generateReportData(Request $request)
    {
        // Implementation details
    }
}</pre>
        </div>
        
        <h3 class="text-xl font-semibold mb-2">Registering Your Handler</h3>
        <p class="mb-4">Register your custom handler in a service provider:</p>
        <div class="bg-slate-800 rounded-lg p-4 mb-6">
          <pre class="text-green-400">
// In a service provider
public function boot()
{
    $this->app->make('shadow')->registerHandler(
        'custom-report',
        \App\Shadow\Handlers\CustomReportHandler::class
    );
}</pre>
        </div>
        
        <h3 class="text-xl font-semibold mb-2">Using Your Handler</h3>
        <p class="mb-4">Use your custom handler in routes:</p>
        <div class="bg-slate-800 rounded-lg p-4 mb-6">
          <pre class="text-green-400">
// In routes/api.php
Route::get('/reports/custom', function (Request $request) {
    return app('shadow')->handle('custom-report', $request);
});</pre>
        </div>
      </section>
      
      <section id="middleware" class="mb-12">
        <h2 class="text-2xl font-semibold mb-4">2. Custom Middleware</h2>
        
        <p class="mb-4">
          Create custom middleware to add functionality to the Shadow request pipeline.
        </p>
        
        <h3 class="text-xl font-semibold mb-2">Creating Middleware</h3>
        <div class="bg-slate-800 rounded-lg p-4 mb-6">
          <pre class="text-green-400">
namespace App\Shadow\Middleware;

use Closure;
use Illuminate\Http\Request;

class CustomRateLimiter
{
    public function handle(Request $request, Closure $next)
    {
        // Your rate limiting logic
        
        return $next($request);
    }
}</pre>
        </div>
        
        <h3 class="text-xl font-semibold mb-2">Registering Middleware</h3>
        <p class="mb-4">Register your middleware in the Shadow configuration:</p>
        <div class="bg-slate-800 rounded-lg p-4 mb-6">
          <pre class="text-green-400">
// config/shadow.php
'middleware' => [
    // Global middleware
    'global' => [
        \App\Shadow\Middleware\CustomRateLimiter::class,
    ],
    
    // Route-specific middleware
    'routes' => [
        'users' => [
            \App\Shadow\Middleware\UserSpecificMiddleware::class,
        ],
    ],
],</pre>
        </div>
      </section>
      
      <section id="transformers" class="mb-12">
        <h2 class="text-2xl font-semibold mb-4">3. Custom Transformers</h2>
        
        <p class="mb-4">
          Transformers allow you to customize how your data is presented in API responses.
        </p>
        
        <h3 class="text-xl font-semibold mb-2">Creating a Transformer</h3>
        <div class="bg-slate-800 rounded-lg p-4 mb-6">
          <pre class="text-green-400">
namespace App\Shadow\Transformers;

use ThinkNeverland\Shadow\Transformers\BaseTransformer;

class UserTransformer extends BaseTransformer
{
    public function transform($model)
    {
        return [
            'id' => $model->id,
            'name' => $model->name,
            'email' => $model->email,
            'role' => $model->role->name,
            'last_login' => $model->last_login->format('Y-m-d H:i:s'),
            'profile' => [
                'avatar' => $model->profile->avatar_url,
                'bio' => $model->profile->bio
            ]
        ];
    }
}</pre>
        </div>
        
        <h3 class="text-xl font-semibold mb-2">Using Your Transformer</h3>
        <p class="mb-4">Specify the transformer in your model configuration:</p>
        <div class="bg-slate-800 rounded-lg p-4 mb-6">
          <pre class="text-green-400">
// In your User model
protected $shadowConfig = [
    'transformer' => \App\Shadow\Transformers\UserTransformer::class,
];</pre>
        </div>
      </section>
      
      <section id="validators" class="mb-12">
        <h2 class="text-2xl font-semibold mb-4">4. Custom Validators</h2>
        
        <p class="mb-4">
          Create custom validators to implement specialized validation logic.
        </p>
        
        <h3 class="text-xl font-semibold mb-2">Creating a Validator</h3>
        <div class="bg-slate-800 rounded-lg p-4 mb-6">
          <pre class="text-green-400">
namespace App\Shadow\Validators;

use ThinkNeverland\Shadow\Validators\BaseValidator;

class ProductValidator extends BaseValidator
{
    public function rules(array $data, $id = null)
    {
        $rules = [
            'name' => 'required|string|max:100',
            'price' => 'required|numeric|min:0',
            'sku' => 'required|string|unique:products,sku',
            'category_id' => 'required|exists:categories,id',
        ];
        
        if ($id) {
            $rules['sku'] = 'required|string|unique:products,sku,' . $id;
        }
        
        return $rules;
    }
    
    public function messages()
    {
        return [
            'sku.unique' => 'This SKU is already in use.',
            'category_id.exists' => 'The selected category does not exist.',
        ];
    }
}</pre>
        </div>
        
        <h3 class="text-xl font-semibold mb-2">Using Your Validator</h3>
        <p class="mb-4">Specify the validator in your model configuration:</p>
        <div class="bg-slate-800 rounded-lg p-4 mb-6">
          <pre class="text-green-400">
// In your Product model
protected $shadowConfig = [
    'validator' => \App\Shadow\Validators\ProductValidator::class,
];</pre>
        </div>
      </section>
      
      <section id="events" class="mb-12">
        <h2 class="text-2xl font-semibold mb-4">5. Events & Listeners</h2>
        
        <p class="mb-4">
          Shadow dispatches events at various points in the request lifecycle that you can listen for.
        </p>
        
        <h3 class="text-xl font-semibold mb-2">Available Events</h3>
        <div class="bg-slate-800 rounded-lg p-4 mb-6">
          <pre class="text-green-400">
// Shadow core events
ThinkNeverland\Shadow\Events\ModelCreated
ThinkNeverland\Shadow\Events\ModelUpdated
ThinkNeverland\Shadow\Events\ModelDeleted
ThinkNeverland\Shadow\Events\QueryExecuted
ThinkNeverland\Shadow\Events\RequestProcessed</pre>
        </div>
        
        <h3 class="text-xl font-semibold mb-2">Creating a Listener</h3>
        <p class="mb-4">Create a listener for Shadow events:</p>
        <div class="bg-slate-800 rounded-lg p-4 mb-6">
          <pre class="text-green-400">
namespace App\Listeners;

use ThinkNeverland\Shadow\Events\ModelCreated;

class NotifyAdminOnNewUser
{
    public function handle(ModelCreated $event)
    {
        $model = $event->model;
        
        // Only process User models
        if (!($model instanceof \App\Models\User)) {
            return;
        }
        
        // Notify admin
        \Notification::send(
            \App\Models\User::whereRole('admin')->get(),
            new \App\Notifications\NewUserRegistered($model)
        );
    }
}</pre>
        </div>
        
        <h3 class="text-xl font-semibold mb-2">Registering Listeners</h3>
        <p class="mb-4">Register your listeners in the EventServiceProvider:</p>
        <div class="bg-slate-800 rounded-lg p-4 mb-6">
          <pre class="text-green-400">
// In app/Providers/EventServiceProvider.php
protected $listen = [
    \ThinkNeverland\Shadow\Events\ModelCreated::class => [
        \App\Listeners\NotifyAdminOnNewUser::class,
    ],
];</pre>
        </div>
      </section>
    </div>
  </div>
</template>

<script setup>
</script> 