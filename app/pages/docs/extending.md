# Extending Shadow: Creating Custom Packages

## Table of Contents

- [Introduction](#introduction)
- [Package Structure](#package-structure)
- [Core Extension Points](#core-extension-points)
- [Best Practices](#best-practices)
- [Examples](#examples)
- [Publishing Your Package](#publishing-your-package)

## Introduction

Shadow is designed to be highly extensible, allowing you to create custom packages that integrate seamlessly with its core functionality. This guide will walk you through the process of creating your own Shadow-based packages.

## Package Structure

A typical Shadow extension package should follow this structure:

```
your-package/
├── config/
│   └── config.php
├── src/
│   ├── Providers/
│   │   └── YourServiceProvider.php
│   ├── Services/
│   │   └── YourService.php
│   ├── Traits/
│   │   └── YourTrait.php
│   └── YourPackageClass.php
├── tests/
│   └── YourTest.php
├── composer.json
└── README.md
```

### Example composer.json

```json
{
    "name": "your-vendor/your-package",
    "description": "Your Shadow extension package",
    "type": "library",
    "require": {
        "php": "^8.0",
        "thinkneverland/shadow": "^1.0",
        "laravel/framework": "^9.0|^10.0"
    },
    "autoload": {
        "psr-4": {
            "YourVendor\\YourPackage\\": "src/"
        }
    },
    "extra": {
        "laravel": {
            "providers": [
                "YourVendor\\YourPackage\\Providers\\YourServiceProvider"
            ]
        }
    }
}
```

## Core Extension Points

### 1. Service Provider Integration

```php
namespace YourVendor\YourPackage\Providers;

use Illuminate\Support\ServiceProvider;
use ThinkNeverland\Shadow\Facades\Shadow;

class YourServiceProvider extends ServiceProvider
{
    public function boot()
    {
        // Register your package with Shadow
        Shadow::extend('your-feature', function ($shadow) {
            return new YourFeatureService($shadow);
        });

        // Register custom commands
        if ($this->app->runningInConsole()) {
            $this->commands([
                YourCommand::class
            ]);
        }

        // Publish configurations
        $this->publishes([
            __DIR__.'/../../config/your-config.php' => config_path('your-config.php'),
        ], 'your-package-config');
    }
}
```

### 2. Custom Traits

```php
namespace YourVendor\YourPackage\Traits;

use ThinkNeverland\Shadow\Traits\HasShadow;

trait YourCustomTrait
{
    use HasShadow;

    public function initializeYourCustomTrait()
    {
        // Initialize your trait functionality
        $this->shadowConfig = array_merge($this->shadowConfig ?? [], [
            'your_feature' => true,
            'your_settings' => [
                // Your custom settings
            ]
        ]);
    }

    // Add your custom methods
    public function yourCustomMethod()
    {
        return $this->shadow()
            ->withYourFeature()
            ->get();
    }
}
```

### 3. Custom Services

```php
namespace YourVendor\YourPackage\Services;

use ThinkNeverland\Shadow\Services\ShadowService;

class YourService
{
    protected $shadow;

    public function __construct(ShadowService $shadow)
    {
        $this->shadow = $shadow;
    }

    public function extendQuery($query)
    {
        return $query->withYourCustomLogic();
    }
}
```

### 4. Custom Resources

```php
namespace YourVendor\YourPackage\Http\Resources;

use ThinkNeverland\Shadow\Http\Resources\ShadowResource;

class YourResource extends ShadowResource
{
    public function toArray($request)
    {
        $data = parent::toArray($request);
        
        return array_merge($data, [
            'your_custom_field' => $this->calculateCustomField(),
        ]);
    }
}
```

### 3. Custom Controllers

```php
namespace YourVendor\YourPackage\Http\Controllers;

use Illuminate\Routing\Controller;
use Illuminate\Http\Request;
use ThinkNeverland\Shadow\Services\ShadowService;

class YourController extends Controller
{
    protected $shadowService;

    public function __construct(ShadowService $shadowService)
    {
        $this->shadowService = $shadowService;
    }

    public function index(Request $request)
    {
        return $this->shadowService->handleRequest($request);
    }
}
```

## Best Practices

### 1. Configuration Management

```php
// config/your-config.php
return [
    'feature_enabled' => env('YOUR_FEATURE_ENABLED', true),
    'custom_settings' => [
        'option_1' => env('YOUR_OPTION_1', 'default'),
        'option_2' => env('YOUR_OPTION_2', 'default'),
    ]
];
```

### 2. Event Integration

```php
namespace YourVendor\YourPackage\Events;

use ThinkNeverland\Shadow\Events\ShadowEvent;

class YourEvent extends ShadowEvent
{
    public function handle()
    {
        // Your event handling logic
    }
}
```

### 3. Middleware Extension

```php
namespace YourVendor\YourPackage\Http\Middleware;

use ThinkNeverland\Shadow\Http\Middleware\ShadowMiddleware;

class YourMiddleware extends ShadowMiddleware
{
    public function handle($request, $next)
    {
        // Your middleware logic
        return parent::handle($request, $next);
    }
}
```

## Examples

### 1. Adding Custom Query Capabilities

```php
namespace YourVendor\YourPackage;

use ThinkNeverland\Shadow\Services\ShadowService;

class CustomQueryBuilder
{
    public function extend(ShadowService $shadow)
    {
        $shadow->macro('withCustomFeature', function ($params) use ($shadow) {
            return $shadow->query->where('custom_field', $params);
        });
    }
}
```

### 2. Creating a Custom Cache Strategy

```php
namespace YourVendor\YourPackage\Cache;

use ThinkNeverland\Shadow\Cache\CacheStrategy;

class YourCacheStrategy extends CacheStrategy
{
    public function getCacheKey($query)
    {
        return 'your_cache_prefix:' . $this->generateKey($query);
    }

    public function shouldCache($query)
    {
        return config('your-config.cache_enabled') && parent::shouldCache($query);
    }
}
```

### 3. Adding Custom API Endpoints

```php
namespace YourVendor\YourPackage\Http\Controllers;

use ThinkNeverland\Shadow\Http\Controllers\ApiController;

class YourController extends ApiController
{
    public function customEndpoint()
    {
        return $this->shadow
            ->model(YourModel::class)
            ->withCustomFeature()
            ->paginate();
    }
}
```

## Publishing Your Package

1. **Prepare Your Package**

   ```bash
   composer validate
   php vendor/bin/phpunit
   php vendor/bin/php-cs-fixer fix
   ```

2. **Update Documentation**
   - Ensure README.md is comprehensive
   - Include installation instructions
   - Document all features and configurations
   - Provide usage examples

3. **Version Control**

   ```bash
   git tag v1.0.0
   git push origin v1.0.0
   ```

4. **Publish to Packagist**
   - Create an account on packagist.org
   - Submit your package
   - Set up GitHub webhooks

5. **Maintenance**
   - Monitor issues and pull requests
   - Keep dependencies updated
   - Maintain compatibility with Shadow core
   - Regular security updates

## Integration Testing

```php
namespace YourVendor\YourPackage\Tests;

use ThinkNeverland\Shadow\Tests\TestCase;

class YourTest extends TestCase
{
    public function test_your_feature()
    {
        $result = YourModel::query()
            ->shadow()
            ->withYourFeature()
            ->get();

        $this->assertYourFeatureWorks($result);
    }
}
```

Remember to follow Laravel's and Shadow's coding standards and best practices when creating your package. This ensures compatibility and maintainability of your extension.
