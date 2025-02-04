<template>
  <div class="max-w-4xl mx-auto">
    <UContainer class="max-w-8xl py-12 space-y-6">
      <h1 class="text-4xl sm:text-6xl lg:text-7xl tracking-tight text-gray-800 font-bold text-center font-display max-w-4xl mx-auto">
        API Documentation
      </h1>

      <div class="w-full columns-2 py-4">
          <ButtonLink label="Documentation" size="sm" rounded="md" href="/docs" />
      </div>

      <div class="bg-gray-100 p-6 rounded-lg mb-8">
        <ul class="space-y-2">
          <li><a href="#introduction" class="text-blue-600 hover:underline">1. Introduction</a></li>
          <li><a href="#authentication" class="text-blue-600 hover:underline">2. Authentication</a></li>
          <li><a href="#request-format" class="text-blue-600 hover:underline">3. Request Format</a></li>
          <li><a href="#response-format" class="text-blue-600 hover:underline">4. Response Format</a></li>
          <li><a href="#endpoints" class="text-blue-600 hover:underline">5. Endpoints</a></li>
          <li><a href="#error-handling" class="text-blue-600 hover:underline">6. Error Handling</a></li>
        </ul>
      </div>

      <section id="introduction" class="mb-12">
        <h2 class="text-2xl font-semibold mb-4">1. Introduction</h2>
        <p class="text-gray-700 mb-4">
          The Evolve API provides a RESTful interface for interacting with your models. This documentation covers all available endpoints and their usage. The API follows REST principles and uses JSON for data exchange.
        </p>

        <h3 class="text-xl font-semibold mb-4">Key Features</h3>
        <ul class="list-disc pl-6 space-y-2 text-gray-700">
          <li>Full CRUD operations for all resources</li>
          <li>Advanced filtering and sorting</li>
          <li>Relationship handling</li>
          <li>Bulk operations</li>
          <li>Rate limiting</li>
          <li>Caching</li>
          <li>Authentication and Authorization</li>
          <li>Comprehensive documentation</li>
        </ul>
      </section>

      <section id="authentication" class="mb-12">
        <h2 class="text-2xl font-semibold mb-4">2. Authentication</h2>
        
        <h3 class="text-xl font-semibold mb-4">Bearer Token (Recommended)</h3>
        <div class="bg-gray-800 rounded-lg p-4 mb-6">
          <pre class="text-green-400">
Authorization: Bearer &lt;your-token&gt;

# Token request example:
POST /api/auth/token
Content-Type: application/json

{
    "grant_type": "password",
    "client_id": "your-client-id",
    "client_secret": "your-client-secret",
    "username": "user@example.com",
    "password": "your-password",
    "scope": "read write"
}</pre>
        </div>

        <h3 class="text-xl font-semibold mb-4">API Key</h3>
        <div class="bg-gray-800 rounded-lg p-4 mb-6">
          <pre class="text-green-400">
X-API-Key: &lt;your-api-key&gt;

# Generate API key:
POST /api/keys/generate
Authorization: Bearer &lt;your-token&gt;

{
    "name": "My API Key",
    "expires_at": "2024-12-31",
    "permissions": ["read", "write"]
}</pre>
        </div>
      </section>

      <section id="request-format" class="mb-12">
        <h2 class="text-2xl font-semibold mb-4">3. Request Format</h2>
        
        <h3 class="text-xl font-semibold mb-4">Headers</h3>
        <div class="bg-gray-800 rounded-lg p-4 mb-6">
          <pre class="text-green-400">
# Required headers
Accept: application/json
Content-Type: application/json

# Optional headers
X-Request-ID: &lt;unique-request-id&gt;        # For request tracking
X-Client-Version: &lt;client-version&gt;       # For version-specific handling
X-Language: en                           # For localization
Cache-Control: no-cache                  # For cache control</pre>
        </div>

        <h3 class="text-xl font-semibold mb-4">Query Parameters</h3>
        <div class="bg-gray-800 rounded-lg p-4 mb-6">
          <pre class="text-green-400">
# Filtering
GET /api/users?filter[status]=active
GET /api/users?filter[role]=admin

# Sorting
GET /api/users?sort=name                 # Ascending
GET /api/users?sort=-name                # Descending

# Pagination
GET /api/users?page=2&per_page=15

# Including Relations
GET /api/users?include=profile,posts</pre>
        </div>

        <h3 class="text-xl font-semibold mb-4">Foreign Key Search</h3>
        <div class="bg-gray-800 rounded-lg p-4 mb-6">
          <pre class="text-green-400">
# Create/Update with foreign key search
POST /api/orders
{
    "company": "Acme Corp",        # Will resolve to company_id
    "customer": "john@example.com" # Will resolve to customer_id
}

# Model Configuration
class Order extends Model {
    use HasEvolveConfig;

    public static function evolveConfig(): array {
        return [
            'foreign_keys' => [
                'company_id' => [
                    'model' => Company::class,
                    'search_fields' => ['name', 'code'],
                    'strategy' => 'first',
                    'fuzzy' => true
                ],
                'customer_id' => [
                    'model' => Customer::class,
                    'search_fields' => ['email', 'name'],
                    'strategy' => 'error_if_multiple',
                    'required' => true
                ]
            ]
        ];
    }
}</pre>
        </div>

        <h4 class="text-lg font-semibold mb-2">Foreign Key Search Options</h4>
        <ul class="list-disc pl-6 space-y-2 text-gray-700 mb-6">
          <li><strong>model</strong>: Target model class to search in</li>
          <li><strong>search_fields</strong>: Fields to search (defaults to ['name', 'title', 'label'])</li>
          <li><strong>strategy</strong>: How to handle multiple matches:
            <ul class="list-disc pl-6 mt-2">
              <li><code>first</code>: Use first match (default)</li>
              <li><code>latest</code>: Use most recent match</li>
              <li><code>error_if_multiple</code>: Error if multiple matches found</li>
            </ul>
          </li>
          <li><strong>fuzzy</strong>: Enable fuzzy matching (default: true)</li>
          <li><strong>required</strong>: Whether the field must resolve to a match</li>
          <li><strong>conditions</strong>: Additional conditions to filter matches</li>
        </ul>
      </section>

      <section id="response-format" class="mb-12">
        <h2 class="text-2xl font-semibold mb-4">4. Response Format</h2>
        
        <h3 class="text-xl font-semibold mb-4">Success Response</h3>
        <div class="bg-gray-800 rounded-lg p-4 mb-6">
          <pre class="text-green-400">
{
    "data": {
        "id": 1,
        "type": "users",
        "attributes": {
            "name": "John Doe",
            "email": "john@example.com",
            "created_at": "2024-01-01T00:00:00Z"
        },
        "relationships": {
            "posts": {
                "data": [
                    { "id": 1, "type": "posts" }
                ]
            }
        }
    },
    "included": [...],
    "meta": {
        "version": "1.0"
    }
}</pre>
        </div>
      </section>

      <section id="endpoints" class="mb-12">
        <h2 class="text-2xl font-semibold mb-4">5. Endpoints</h2>
        
        <h3 class="text-xl font-semibold mb-4">Resource Endpoints</h3>
        <div class="bg-gray-800 rounded-lg p-4 mb-6">
          <pre class="text-green-400">
# List Resources
GET /api/{resource}

# Get Single Resource
GET /api/{resource}/{id}

# Create Resource
POST /api/{resource}

# Update Resource
PUT /api/{resource}/{id}

# Delete Resource
DELETE /api/{resource}/{id}</pre>
        </div>
      </section>

      <section id="error-handling" class="mb-12">
        <h2 class="text-2xl font-semibold mb-4">6. Error Handling</h2>
        
        <h3 class="text-xl font-semibold mb-4">Error Response Format</h3>
        <div class="bg-gray-800 rounded-lg p-4 mb-6">
          <pre class="text-green-400">
{
    "error": {
        "code": "error_code",
        "message": "Human readable message",
        "details": {
            "field": ["Error messages"],
            "trace_id": "unique-error-trace-id"
        }
    }
}</pre>
        </div>

        <h3 class="text-xl font-semibold mb-4">Common Error Codes</h3>
        <ul class="list-disc pl-6 space-y-2 text-gray-700">
          <li><strong>400</strong>: Bad Request</li>
          <li><strong>401</strong>: Unauthorized</li>
          <li><strong>403</strong>: Forbidden</li>
          <li><strong>404</strong>: Not Found</li>
          <li><strong>422</strong>: Validation Error</li>
          <li><strong>429</strong>: Too Many Requests</li>
          <li><strong>500</strong>: Server Error</li>
        </ul>
      </section>
    </UContainer>
  </div>
</template>

<script setup lang="ts">
// Component logic can be added here if needed
</script> 