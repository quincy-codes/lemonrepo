# 🏴‍☠️ Setting Sail with Shadow

## 🌊 Introduction

Welcome to the Shadow API, your magical companion for Laravel CRUD operations and API development. Like a skilled navigator, Shadow will guide you through the waters of API development with ease and precision.

## ⚓ Prerequisites

Before embarking on your journey, ensure you have:

- PHP 8.1 or higher (Your ship's engine)
- Laravel 9.0 or higher (Your navigation system)
- Composer (Your cargo manager)
- Database (Your treasure chest - MySQL, PostgreSQL, or SQLite)

## 🗺️ Installation

1. Install via Composer:

```bash
composer require thinkneverland/shadow
```

2. Publish configuration:

```bash
php artisan vendor:publish --provider="ThinkNeverland\\Shadow\\ShadowServiceProvider"
```

3. Run migrations:

```bash
php artisan migrate
```

4. Configure your ship's settings in `.env`:

```env
SHADOW_API_PREFIX=shadow-api  # Your ship's call sign (optional, defaults to 'shadow-api')
SHADOW_DEBUG=false           # Your ship's log level
```

## 🚀 Quick Start

### 1. Prepare Your Model

```php
use ThinkNeverland\Shadow\Traits\HasShadow;

class User extends Model
{
    use HasShadow;
    
    protected $shadowConfig = [
        'searchable' => ['name', 'email'],
        'filterable' => ['status', 'role'],
        'sortable' => ['created_at', 'updated_at']
    ];
}
```

### 2. Chart Your Controller

```php
use ThinkNeverland\Shadow\Services\ShadowService;

class UserController extends Controller
{
    protected $shadow;
    
    public function __construct(ShadowService $shadow)
    {
        $this->shadow = $shadow;
    }
    
    public function index()
    {
        return $this->shadow->handleIndex('users');
    }
}
```

### 3. Plot Your Routes

```php
// routes/api.php
use ThinkNeverland\Shadow\Facades\Shadow;

Shadow::routes();
```

## 🧭 Basic Navigation

### Querying Your Treasure

```php
// Discover all users with filters
GET /shadow-api/users?filter[status]=active&sort=-created_at

// Find specific user
GET /shadow-api/users/1

// Add new user
POST /shadow-api/users

// Update user
PUT /shadow-api/users/1

// Remove user
DELETE /shadow-api/users/1
```

### Exploring Relations

```php
// Include related treasures
GET /shadow-api/users?include=posts,comments

// Filter by relation
GET /shadow-api/users?filter[posts.status]=published
```

### Field Selection

```php
// Select specific treasures
GET /shadow-api/users?fields=id,name,email
```

## 🗺️ Next Steps

- Navigate through the [Core Documentation](core.md) for detailed features
- Consult the [API Documentation](api.md) for endpoint references
- Review the [Configuration Guide](config.md) for customization
- Explore [Advanced Usage](advanced-usage.md) for complex scenarios

## 🚨 Troubleshooting

If you encounter rough waters:

1. Enable detailed logging in your `.env`:

```env
SHADOW_DEBUG=true
SHADOW_API_PREFIX=shadow-api  # Verify your ship's call sign
```

2. Check the ship's log in `storage/logs/laravel.log`

3. Use the built-in help system:

```php
php artisan shadow:help
```

## 🏳️‍🌈 Support

Need a helping hand?

- Send a message in a bottle: <support@thinkneverland.com>
- Consult the ship's manual: <https://docs.thinkneverland.com/shadow>
- Report issues to the crew: <https://github.com/thinkneverland/shadow/issues>
