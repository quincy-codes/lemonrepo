<template>
  <div class="max-w-4xl mx-auto">
    <UContainer class="max-w-8xl py-12 space-y-6">
      <h1 class="text-4xl sm:text-6xl lg:text-7xl tracking-tight text-gray-800 font-bold text-center font-display max-w-4xl mx-auto">
        Authorization
      </h1>

      <div class="w-full columns-2 py-4">
          <ButtonLink label="Documentation" size="sm" rounded="md" href="/" />
      </div>

      <div class="prose prose-lg max-w-none">
        <div class="bg-gray-100 p-6 rounded-lg mb-8">
          <ul class="space-y-2">
            <li><a href="#overview" class="text-blue-600 hover:underline">1. Overview</a></li>
            <li><a href="#rbac" class="text-blue-600 hover:underline">2. Role-Based Access Control</a></li>
            <li><a href="#policies" class="text-blue-600 hover:underline">3. Policies</a></li>
            <li><a href="#middleware" class="text-blue-600 hover:underline">4. Authorization Middleware</a></li>
            <li><a href="#gates" class="text-blue-600 hover:underline">5. Gates</a></li>
            <li><a href="#best-practices" class="text-blue-600 hover:underline">6. Best Practices</a></li>
          </ul>
        </div>

        <section id="overview" class="mb-12">
          <h2 class="text-2xl font-semibold mb-4">1. Overview</h2>
          <p class="text-xl text-gray-600 text-center max-w-3xl mx-auto">
            Shadow provides a robust authorization system to control access to your application's resources.
          </p>
          <p class="text-gray-700 mb-4">
            For authentication setup, see our <NuxtLink to="/authentication" class="text-blue-600 hover:underline">Authentication Guide</NuxtLink>.
          </p>
        </section>

        <section id="rbac" class="mb-12">
          <h2 class="text-2xl font-semibold mb-4">2. Role-Based Access Control</h2>
          <p class="text-gray-700 mb-4">
            Implement role-based access control to manage user permissions effectively.
          </p>
          
          <h3 class="text-xl font-semibold mb-2">Role-Based Access Control</h3>
          <p class="mb-4">Implement RBAC with Shadow:</p>
          <div class="bg-slate-800 rounded-lg p-4 mb-6">
            <pre class="text-green-400">
// Define roles and permissions
use Shadow\Auth\RBAC;

RBAC::configure([
    'roles' => [
        'admin' => [
            'permissions' => ['*'],
        ],
        'editor' => [
            'permissions' => [
                'posts.create',
                'posts.edit',
                'posts.delete',
                'comments.moderate',
            ],
        ],
        'user' => [
            'permissions' => [
                'posts.read',
                'comments.create',
            ],
        ],
    ],
]);</pre>
          </div>
        </section>

        <section id="policies" class="mb-12">
          <h2 class="text-2xl font-semibold mb-4">3. Policies</h2>
          <p class="text-gray-700 mb-4">
            Use policies to organize authorization logic around specific models or resources.
          </p>
          
          <h3 class="text-xl font-semibold mb-2">Policy-Based Authorization</h3>
          <p class="mb-4">Define policies for your models:</p>
          <div class="bg-slate-800 rounded-lg p-4 mb-6">
            <pre class="text-green-400">
// Post policy example
use Shadow\Auth\Policy;

class PostPolicy extends Policy
{
    public function view(User $user, Post $post)
    {
        return true; // Public access
    }

    public function create(User $user)
    {
        return $user->hasPermission('posts.create');
    }

    public function update(User $user, Post $post)
    {
        return $user->hasPermission('posts.edit') ||
            $post->user_id === $user->id;
    }

    public function delete(User $user, Post $post)
    {
        return $user->hasPermission('posts.delete') ||
            ($post->user_id === $user->id && 
             $post->created_at->diffInHours() < 24);
    }
}</pre>
          </div>
        </section>

        <section id="middleware" class="mb-12">
          <h2 class="text-2xl font-semibold mb-4">4. Authorization Middleware</h2>
          <p class="text-gray-700 mb-4">
            Protect routes using authorization middleware.
          </p>
          
          <h3 class="text-xl font-semibold mb-2">Route Protection</h3>
          <p class="mb-4">Secure routes with middleware:</p>
          <div class="bg-slate-800 rounded-lg p-4 mb-6">
            <pre class="text-green-400">
// Route middleware
Route::middleware(['auth', 'can:posts.create'])->group(function () {
    Route::post('/posts', [PostController::class, 'store']);
});

// Controller middleware
class PostController extends Controller
{
    public function __construct()
    {
        $this->middleware('can:posts.edit')->only(['edit', 'update']);
        $this->middleware('can:posts.delete')->only('destroy');
    }
}</pre>
          </div>
        </section>

        <section id="gates" class="mb-12">
          <h2 class="text-2xl font-semibold mb-4">5. Gates</h2>
          <p class="text-gray-700 mb-4">
            Define custom authorization rules using gates.
          </p>
          
          <h3 class="text-xl font-semibold mb-2">Gate-Based Authorization</h3>
          <p class="mb-4">Define custom authorization gates:</p>
          <div class="bg-slate-800 rounded-lg p-4 mb-6">
            <pre class="text-green-400">
// Define custom gates
use Shadow\Auth\Gate;

Gate::define('publish-post', function (User $user, Post $post) {
    return $user->hasRole('editor') || 
           ($user->hasRole('author') && $user->posts_count >= 5);
});

// Use gates in controllers
public function publish(Post $post)
{
    if (Gate::allows('publish-post', $post)) {
        $post->publish();
    }
}</pre>
          </div>
        </section>

        <section id="best-practices" class="mb-12">
          <h2 class="text-2xl font-semibold mb-4">6. Best Practices</h2>
          <ul class="list-disc pl-6 space-y-2 text-gray-700">
            <li>Follow the principle of least privilege</li>
            <li>Use role inheritance for complex permission structures</li>
            <li>Implement fine-grained permissions</li>
            <li>Cache authorization results when possible</li>
            <li>Regular audit of roles and permissions</li>
            <li>Document authorization rules</li>
          </ul>
        </section>
      </div>
    </UContainer>
  </div>
</template>

<script setup lang="ts">
// Component logic can be added here if needed
</script> 