<template>
  <div class="max-w-4xl mx-auto">
    <UContainer class="max-w-8xl py-12 space-y-6">
      <h1 class="text-4xl sm:text-6xl lg:text-7xl tracking-tight text-gray-800 font-bold text-center font-display max-w-4xl mx-auto">
        Authentication
      </h1>

      <div class="w-full columns-2 py-4">
          <ButtonLink label="Documentation" size="sm" rounded="md" href="/" />
      </div>

      <div class="prose prose-lg max-w-none">
        <div class="bg-gray-100 p-6 rounded-lg mb-8">
          <ul class="space-y-2">
            <li><a href="#overview" class="text-blue-600 hover:underline">1. Overview</a></li>
            <li><a href="#token-auth" class="text-blue-600 hover:underline">2. Token Authentication</a></li>
            <li><a href="#session-auth" class="text-blue-600 hover:underline">3. Session Authentication</a></li>
            <li><a href="#mfa" class="text-blue-600 hover:underline">4. Multi-Factor Authentication</a></li>
            <li><a href="#oauth" class="text-blue-600 hover:underline">5. OAuth Integration</a></li>
            <li><a href="#best-practices" class="text-blue-600 hover:underline">6. Best Practices</a></li>
          </ul>
        </div>

        <section id="overview" class="mb-12">
          <h2 class="text-2xl font-semibold mb-4">1. Overview</h2>
          <p class="text-xl text-gray-600 text-center max-w-3xl mx-auto">
            Shadow provides multiple authentication methods to secure your application. This guide covers all available authentication options and their implementation.
          </p>
          <p class="text-gray-700 mb-4">
            For general security guidelines, see our <NuxtLink to="/security" class="text-blue-600 hover:underline">Security Guide</NuxtLink>.
          </p>
        </section>

        <section id="token-auth" class="mb-12">
          <h2 class="text-2xl font-semibold mb-4">2. Token Authentication</h2>
          <p class="text-gray-700 mb-4">
            Token-based authentication is the recommended method for API authentication.
          </p>
          
          <h3 class="text-xl font-semibold mb-2">Implementation</h3>
          <p class="mb-4">Configure token-based authentication:</p>
          <div class="bg-slate-800 rounded-lg p-4 mb-6">
            <pre class="text-green-400">
// Configure token authentication
use Shadow\Auth\Token;

class AuthConfig extends Config
{
    public function configure(): void
    {
        Token::configure([
            'lifetime' => 3600,
            'refresh_lifetime' => 86400,
            'rotation' => true,
        ]);
    }</pre>
          </div>
        </section>

        <section id="session-auth" class="mb-12">
          <h2 class="text-2xl font-semibold mb-4">3. Session Authentication</h2>
          <p class="text-gray-700 mb-4">
            Session-based authentication is suitable for traditional web applications.
          </p>
          
          <h3 class="text-xl font-semibold mb-2">Configuration</h3>
          <p class="mb-4">Configure session-based authentication:</p>
          <div class="bg-slate-800 rounded-lg p-4 mb-6">
            <pre class="text-green-400">
// Configure session authentication
use Shadow\Auth\Session;

Session::configure([
    'lifetime' => 120,
    'expire_on_close' => true,
    'secure' => true,
    'same_site' => 'lax',
]);</pre>
          </div>
        </section>

        <section id="mfa" class="mb-12">
          <h2 class="text-2xl font-semibold mb-4">4. Multi-Factor Authentication</h2>
          <p class="text-gray-700 mb-4">
            Enhance security with multi-factor authentication support.
          </p>
          
          <h3 class="text-xl font-semibold mb-2">Setup</h3>
          <p class="mb-4">Configure multi-factor authentication:</p>
          <div class="bg-slate-800 rounded-lg p-4 mb-6">
            <pre class="text-green-400">
// Enable MFA
use Shadow\Auth\MFA;

MFA::configure([
    'methods' => ['totp', 'sms'],
    'remember_device' => true,
    'remember_duration' => 30,
]);</pre>
          </div>
        </section>

        <section id="oauth" class="mb-12">
          <h2 class="text-2xl font-semibold mb-4">5. OAuth Integration</h2>
          <p class="text-gray-700 mb-4">
            Integrate with popular OAuth providers.
          </p>
          
          <h3 class="text-xl font-semibold mb-2">Provider Setup</h3>
          <p class="mb-4">Configure OAuth providers:</p>
          <div class="bg-slate-800 rounded-lg p-4 mb-6">
            <pre class="text-green-400">
// Configure OAuth providers
use Shadow\Auth\OAuth;

OAuth::configure([
    'providers' => [
        'github' => [
            'client_id' => env('GITHUB_CLIENT_ID'),
            'client_secret' => env('GITHUB_CLIENT_SECRET'),
            'redirect' => '/auth/github/callback',
        ],
    ],
]);</pre>
          </div>
        </section>

        <section id="best-practices" class="mb-12">
          <h2 class="text-2xl font-semibold mb-4">6. Best Practices</h2>
          <ul class="list-disc pl-6 space-y-2 text-gray-700">
            <li>Always use HTTPS for authentication endpoints</li>
            <li>Implement proper password hashing</li>
            <li>Use secure session configurations</li>
            <li>Implement rate limiting for auth endpoints</li>
            <li>Regular security audits</li>
            <li>Monitor failed login attempts</li>
          </ul>
        </section>
      </div>
    </UContainer>
  </div>
</template>

<script setup lang="ts">
// Component logic can be added here if needed
</script> 