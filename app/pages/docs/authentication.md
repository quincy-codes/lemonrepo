# Authentication Guide

## Table of Contents

- [Overview](#overview)
- [Configuration](#configuration)
- [Authentication Methods](#authentication-methods)
- [Token Management](#token-management)
- [Security Best Practices](#security-best-practices)
- [Examples](#examples)

## Overview

Shadow provides flexible authentication options that integrate seamlessly with Laravel's authentication system. This guide covers all authentication methods and their implementation.

## Configuration

### Basic Setup

```php
// config/shadow.php
return [
    'auth' => [
        'enabled' => true,
        'guards' => ['api', 'sanctum'],
        'provider' => 'users',
        'tokens' => [
            'lifetime' => 3600,
            'refresh_lifetime' => 1209600,
        ],
    ],
];
```

### Environment Variables

```env
SHADOW_AUTH_ENABLED=true
SHADOW_AUTH_TOKEN_LIFETIME=3600
SHADOW_AUTH_REFRESH_TOKEN_LIFETIME=1209600
```

## Authentication Methods

### 1. API Token Authentication

```php
// Generate API token
$token = $user->createToken('api-token');

// Use token in requests
$response = $client->withToken($token)->get('/api/users');
```

### 2. OAuth2 Integration

```php
// Configure OAuth provider
'oauth' => [
    'providers' => [
        'github' => [
            'client_id' => env('GITHUB_CLIENT_ID'),
            'client_secret' => env('GITHUB_CLIENT_SECRET'),
            'redirect' => '/auth/github/callback',
        ],
    ],
],
```

### 3. JWT Authentication

```php
// Configure JWT
'jwt' => [
    'secret' => env('JWT_SECRET'),
    'ttl' => 60, // minutes
    'refresh_ttl' => 20160, // minutes
],
```

## Token Management

### Creating Tokens

```php
// Create token with abilities
$token = $user->createToken('api-token', ['read', 'write']);

// Create temporary token
$token = $user->createTemporaryToken(60); // expires in 60 minutes
```

### Revoking Tokens

```php
// Revoke specific token
$user->tokens()->where('id', $tokenId)->delete();

// Revoke all tokens
$user->tokens()->delete();
```

### Token Refresh

```php
// Refresh token
$newToken = $auth->refresh($oldToken);
```

## Security Best Practices

### 1. Token Security

- Use HTTPS only
- Implement token expiration
- Rotate tokens regularly
- Use secure storage methods

### 2. Rate Limiting

```php
// Configure rate limiting
'rate_limiting' => [
    'enabled' => true,
    'max_attempts' => 5,
    'decay_minutes' => 1,
],
```

### 3. Token Scopes

```php
// Define token scopes
protected $tokenScopes = [
    'read:users',
    'write:users',
    'delete:users',
];
```

## Examples

### 1. Basic Authentication Flow

```php
// Login endpoint
public function login(Request $request)
{
    $credentials = $request->validate([
        'email' => 'required|email',
        'password' => 'required',
    ]);

    if (Auth::attempt($credentials)) {
        $token = $request->user()->createToken('api-token');
        return response()->json(['token' => $token->plainTextToken]);
    }

    return response()->json(['error' => 'Invalid credentials'], 401);
}
```

### 2. Protected Routes

```php
// routes/api.php
Route::middleware('auth:sanctum')->group(function () {
    Route::get('/user', function (Request $request) {
        return $request->user();
    });
});
```

### 3. Token Abilities

```php
// Check token abilities
if ($user->tokenCan('write:users')) {
    // Perform action
}
```

### 4. OAuth Authentication

```php
// OAuth login
public function redirectToProvider()
{
    return Socialite::driver('github')->redirect();
}

public function handleProviderCallback()
{
    $user = Socialite::driver('github')->user();
    // Create or update user
    // Generate token
}
```

## Error Handling

```php
// Handle authentication errors
try {
    $token = $auth->authenticate($credentials);
} catch (AuthenticationException $e) {
    return response()->json([
        'error' => 'Authentication failed',
        'message' => $e->getMessage(),
    ], 401);
}
```

## Middleware

### Custom Authentication Middleware

```php
namespace App\Http\Middleware;

class CustomAuthMiddleware
{
    public function handle($request, Closure $next)
    {
        if (!$request->user()->hasValidToken()) {
            return response()->json(['error' => 'Invalid token'], 401);
        }

        return $next($request);
    }
}
```

## Testing

```php
// Testing authentication
public function test_user_can_authenticate()
{
    $user = User::factory()->create();
    $token = $user->createToken('test-token');

    $response = $this->withToken($token->plainTextToken)
        ->get('/api/user');

    $response->assertStatus(200);
}
```

## Related Documentation

- [Security Best Practices](security.md)
- [API Documentation](api.md)
- [Configuration Guide](config.md)
