# Security Best Practices

## Table of Contents

- [Authentication](#authentication)
- [Authorization](#authorization)
- [Data Protection](#data-protection)
- [Rate Limiting](#rate-limiting)
- [Request Validation](#request-validation)
- [Audit Logging](#audit-logging)

## Authentication

### Token Management

```php
// Generate secure tokens
$token = Shadow::createToken([
    'scopes' => ['read', 'write'],
    'expires_in' => 3600,
    'device_name' => 'iPhone 12',
    'permissions' => ['users.read', 'posts.write']
]);

// Configure token settings
'tokens' => [
    'lifetime' => 3600,
    'refresh_lifetime' => 1209600,
    'rotation' => true,
    'single_use' => false
]

// Revoke tokens
Shadow::revokeToken($tokenId);
Shadow::revokeAllTokens($userId);

// Token refresh
POST /api/auth/refresh
Authorization: Bearer <refresh-token>
```

### Multi-Factor Authentication

```php
// Enable MFA
'mfa' => [
    'enabled' => true,
    'methods' => ['totp', 'sms', 'email'],
    'grace_period' => 7200
]

// Verify MFA
POST /api/auth/mfa/verify
{
    "method": "totp",
    "code": "123456"
}

// Recovery codes
POST /api/auth/mfa/recovery
{
    "code": "XXXX-XXXX-XXXX-XXXX"
}
```

## Authorization

### Role-Based Access Control

```php
// Define roles and permissions
'roles' => [
    'admin' => [
        'permissions' => ['*'],
        'scope' => 'global'
    ],
    'moderator' => [
        'permissions' => [
            'users.read',
            'posts.*',
            'comments.moderate'
        ],
        'scope' => 'assigned_teams'
    ]
]

// Check permissions
Shadow::authorize('users.update', $user);
Shadow::authorizeAny(['posts.create', 'posts.update']);
```

### Resource-Level Permissions

```php
class Post extends Model
{
    use HasShadow;
    
    protected function canAccess($user, $action)
    {
        return match($action) {
            'view' => true,
            'edit' => $user->id === $this->user_id || $user->isAdmin(),
            'delete' => $user->isAdmin(),
            default => false
        };
    }
}
```

## Data Protection

### Field-Level Encryption

```php
// Configure encryption
'encryption' => [
    'algorithm' => 'AES-256-GCM',
    'key_rotation' => true,
    'fields' => [
        'users' => [
            'ssn' => true,
            'bank_account' => [
                'algorithm' => 'AES-256-GCM',
                'searchable' => true
            ]
        ]
    ]
]

// Custom encryption
class User extends Model
{
    use HasShadow;
    
    protected function encryptField($field, $value)
    {
        return match($field) {
            'ssn' => Shadow::encrypt($value, ['searchable' => true]),
            'notes' => Shadow::encrypt($value),
            default => $value
        };
    }
}
```

### Data Masking

```php
// Configure masking
'masking' => [
    'patterns' => [
        'email' => '/(.{3}).*(@.*)/i',
        'phone' => '/(\d{3})\d{4}(\d{4})/',
        'card' => '/(\d{4})\d{8}(\d{4})/'
    ],
    'replacements' => [
        'email' => '$1***$2',
        'phone' => '$1****$2',
        'card' => '$1********$2'
    ]
]

// Custom masking
class User extends Model
{
    use HasShadow;
    
    protected function maskField($field, $value)
    {
        return match($field) {
            'email' => Shadow::mask($value, 'email'),
            'phone' => Shadow::mask($value, 'phone'),
            default => $value
        };
    }
}
```

## Rate Limiting

### Basic Rate Limiting

```php
// Configure rate limits
'rate_limits' => [
    'default' => [
        'limit' => 60,
        'window' => 60
    ],
    'auth' => [
        'limit' => 5,
        'window' => 300
    ],
    'search' => [
        'limit' => 30,
        'window' => 60,
        'cost' => 2
    ]
]

// Custom rate limiters
Shadow::rateLimiter('api', function ($request) {
    return [
        'key' => $request->ip(),
        'limit' => 60,
        'window' => 60,
        'headers' => true
    ];
});
```

### Advanced Rate Limiting

```php
// Dynamic rate limiting
Shadow::rateLimiter('dynamic', function ($request) {
    if ($request->user()->isPremium()) {
        return ['limit' => 1000, 'window' => 60];
    }
    return ['limit' => 60, 'window' => 60];
});

// Cost-based rate limiting
Shadow::rateLimiter('search', function ($request) {
    return [
        'limit' => 100,
        'window' => 60,
        'cost' => $request->input('complexity', 1)
    ];
});
```

## Request Validation

### Input Sanitization

```php
// Configure sanitization
'sanitization' => [
    'strip_tags' => true,
    'escape_html' => true,
    'normalize_whitespace' => true
]

// Custom sanitization
class User extends Model
{
    use HasShadow;
    
    protected function sanitizeInput($field, $value)
    {
        return match($field) {
            'bio' => Shadow::sanitize($value, ['allow_basic_html' => true]),
            'name' => Shadow::sanitize($value, ['strip_tags' => true]),
            default => $value
        };
    }
}
```

### Request Signing

```php
// Configure request signing
'signing' => [
    'algorithm' => 'sha256',
    'header' => 'X-Signature',
    'timestamp_header' => 'X-Timestamp',
    'max_age' => 300
]

// Generate signature
$signature = Shadow::signRequest($method, $url, $body, $secret);

// Verify signature
Shadow::verifySignature($request, $secret);
```

## Audit Logging

### Activity Logging

```php
// Configure audit logging
'audit' => [
    'enabled' => true,
    'events' => ['create', 'update', 'delete'],
    'retention' => 90,
    'detailed' => true
]

// Custom audit events
class User extends Model
{
    use HasShadow;
    
    protected function logActivity($action, $data)
    {
        return [
            'action' => $action,
            'user_id' => auth()->id(),
            'ip' => request()->ip(),
            'user_agent' => request()->userAgent(),
            'data' => $data
        ];
    }
}
```

### Security Events

```php
// Listen for security events
Shadow::on('security.*', function ($event) {
    Log::channel('security')->info($event->message, $event->context);
});

// Custom security events
Shadow::defineSecurityEvent('suspicious_login', function ($user, $request) {
    return [
        'severity' => 'high',
        'message' => 'Suspicious login attempt detected',
        'context' => [
            'user_id' => $user->id,
            'ip' => $request->ip(),
            'location' => $request->location()
        ]
    ];
});
```

For more detailed security implementations and best practices, refer to our [Core Documentation](CORE.md).
