<template>
  <div class="max-w-4xl mx-auto">
    <UContainer class="max-w-8xl py-12 space-y-6">
      <h1 class="text-4xl sm:text-6xl lg:text-7xl tracking-tight text-gray-800 font-bold text-center font-display max-w-4xl mx-auto">
        Security Guide
      </h1>

      <div class="w-full columns-2 py-4">
          <ButtonLink label="Documentation" size="sm" rounded="md" href="/" />
      </div>

      <div class="prose prose-lg max-w-none">
        <div class="bg-gray-100 p-6 rounded-lg mb-8">
          <ul class="space-y-2">
            <li><a href="#overview" class="text-blue-600 hover:underline">1. Overview</a></li>
            <li><a href="#authentication" class="text-blue-600 hover:underline">2. Authentication</a></li>
            <li><a href="#authorization" class="text-blue-600 hover:underline">3. Authorization</a></li>
            <li><a href="#data-protection" class="text-blue-600 hover:underline">4. Data Protection</a></li>
            <li><a href="#api-security" class="text-blue-600 hover:underline">5. API Security</a></li>
            <li><a href="#best-practices" class="text-blue-600 hover:underline">6. Best Practices</a></li>
          </ul>
        </div>

        <section id="overview" class="mb-12">
          <h2 class="text-2xl font-semibold mb-4">1. Overview</h2>
          <p class="text-gray-700 mb-4">
            Shadow provides comprehensive security features to protect your application and its data. This guide covers essential security measures and best practices for securing your Shadow application.
          </p>
          <p class="text-gray-700 mb-4">
            For security policies and vulnerability reporting, please see our <NuxtLink to="/security-policy" class="text-blue-600 hover:underline">Security Policy</NuxtLink>.
          </p>
        </section>

        <section id="authentication" class="mb-12">
          <h2 class="text-2xl font-semibold mb-4">2. Authentication</h2>
          
          <h3 class="text-xl font-semibold mb-4">Token Authentication</h3>
          <div class="bg-gray-800 rounded-lg p-4 mb-6">
            <pre class="text-green-400">
// Configure token authentication
'auth' => [
    'guards' => [
        'api' => [
            'driver' => 'sanctum',
            'provider' => 'users',
        ],
    ],
    
    'tokens' => [
        'lifetime' => env('TOKEN_LIFETIME', 3600),
        'refresh_lifetime' => env('REFRESH_TOKEN_LIFETIME', 86400),
        'rotation' => true,
    ],
];</pre>
          </div>

          <h3 class="text-xl font-semibold mb-4">Multi-Factor Authentication</h3>
          <div class="bg-gray-800 rounded-lg p-4 mb-6">
            <pre class="text-green-400">
// Enable MFA
protected $shadowConfig = [
    'mfa' => [
        'enabled' => true,
        'methods' => ['totp', 'sms'],
        'remember_device' => true,
        'remember_duration' => 30, // days
    ],
];</pre>
          </div>
        </section>

        <section id="authorization" class="mb-12">
          <h2 class="text-2xl font-semibold mb-4">3. Authorization</h2>
          
          <h3 class="text-xl font-semibold mb-4">Role-Based Access Control</h3>
          <div class="bg-gray-800 rounded-lg p-4 mb-6">
            <pre class="text-green-400">
// Define roles and permissions
'roles' => [
    'admin' => [
        'permissions' => ['*'],
    ],
    'editor' => [
        'permissions' => [
            'posts.*',
            'comments.*',
            'users.view',
        ],
    ],
],

// Apply in controllers
public function update(Post $post)
{
    $this->authorize('posts.update');
    // Update logic
}</pre>
          </div>

          <h3 class="text-xl font-semibold mb-4">Policy Implementation</h3>
          <div class="bg-gray-800 rounded-lg p-4 mb-6">
            <pre class="text-green-400">
class PostPolicy
{
    public function update(User $user, Post $post)
    {
        return $user->hasPermission('posts.update') ||
            $post->user_id === $user->id;
    }

    public function delete(User $user, Post $post)
    {
        return $user->hasPermission('posts.delete') ||
            ($post->user_id === $user->id && $post->created_at->diffInHours() < 24);
    }
}</pre>
          </div>
        </section>

        <section id="data-protection" class="mb-12">
          <h2 class="text-2xl font-semibold mb-4">4. Data Protection</h2>
          
          <h3 class="text-xl font-semibold mb-4">Encryption</h3>
          <div class="bg-gray-800 rounded-lg p-4 mb-6">
            <pre class="text-green-400">
// Model encryption
protected $encryptable = [
    'social_security_number',
    'credit_card',
];

// Custom encryption
'encryption' => [
    'algorithm' => 'AES-256-CBC',
    'key_rotation' => true,
    'key_rotation_interval' => 30, // days
];</pre>
          </div>

          <h3 class="text-xl font-semibold mb-4">Data Sanitization</h3>
          <div class="bg-gray-800 rounded-lg p-4 mb-6">
            <pre class="text-green-400">
// Input sanitization
protected $sanitize = [
    'name' => 'trim|strip_tags',
    'bio' => 'trim|strip_tags|allowed_tags:p,br,strong',
    'email' => 'trim|lowercase',
];

// Output sanitization
protected $escape = [
    'description',
    'comments',
];</pre>
          </div>
        </section>

        <section id="api-security" class="mb-12">
          <h2 class="text-2xl font-semibold mb-4">5. API Security</h2>
          
          <h3 class="text-xl font-semibold mb-4">Rate Limiting</h3>
          <div class="bg-gray-800 rounded-lg p-4 mb-6">
            <pre class="text-green-400">
// Configure rate limiting
'rate_limiting' => [
    'enabled' => true,
    'max_attempts' => 60,
    'decay_minutes' => 1,
    'by_ip' => true,
    'by_user' => true,
    'response' => [
        'message' => 'Too many requests',
        'status' => 429,
    ],
];</pre>
          </div>

          <h3 class="text-xl font-semibold mb-4">CORS & Headers</h3>
          <div class="bg-gray-800 rounded-lg p-4 mb-6">
            <pre class="text-green-400">
// Security headers
'headers' => [
    'X-Frame-Options' => 'DENY',
    'X-XSS-Protection' => '1; mode=block',
    'X-Content-Type-Options' => 'nosniff',
    'Strict-Transport-Security' => 'max-age=31536000; includeSubDomains',
    'Content-Security-Policy' => "default-src 'self'",
],

// CORS configuration
'cors' => [
    'allowed_origins' => ['https://example.com'],
    'allowed_methods' => ['GET', 'POST', 'PUT', 'DELETE'],
    'allowed_headers' => ['*'],
    'exposed_headers' => [],
    'max_age' => 0,
    'supports_credentials' => false,
];</pre>
          </div>
        </section>

        <section id="best-practices" class="mb-12">
          <h2 class="text-2xl font-semibold mb-4">6. Best Practices</h2>
          
          <h3 class="text-xl font-semibold mb-4">Security Checklist</h3>
          <ul class="list-disc pl-6 space-y-2 text-gray-700">
            <li>Enable HTTPS only</li>
            <li>Implement proper authentication</li>
            <li>Use role-based access control</li>
            <li>Enable rate limiting</li>
            <li>Implement data encryption</li>
            <li>Configure security headers</li>
            <li>Regular security audits</li>
            <li>Keep dependencies updated</li>
          </ul>

          <h3 class="text-xl font-semibold mb-4">Security Configuration</h3>
          <div class="bg-gray-800 rounded-lg p-4 mb-6">
            <pre class="text-green-400">
// Recommended security configuration
'security' => [
    'ssl_only' => true,
    'password_policy' => [
        'min_length' => 12,
        'require_numbers' => true,
        'require_symbols' => true,
        'require_mixed_case' => true,
    ],
    'session' => [
        'secure' => true,
        'http_only' => true,
        'same_site' => 'lax',
    ],
    'audit_logging' => [
        'enabled' => true,
        'events' => ['login', 'failed_login', 'password_reset'],
    ],
];</pre>
          </div>
        </section>
      </div>
    </UContainer>
  </div>
</template>

<script setup lang="ts">
// Component logic can be added here if needed
</script> 