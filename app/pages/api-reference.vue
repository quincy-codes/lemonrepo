<template>
  <div>
    <div class="container mx-auto px-4 py-12">
      <h1 class="text-4xl font-bold text-center mb-8">API Reference</h1>
      
      <p class="text-xl text-gray-600 text-center max-w-3xl mx-auto mb-12">
        Shadow provides a comprehensive API for interacting with your data. This reference guide covers authentication, endpoints, and response formats.
      </p>
      
      <div class="bg-gray-100 p-6 rounded-lg mb-8">
        <ul class="space-y-2">
          <li><a href="#authentication" class="text-blue-600 hover:underline">1. Authentication</a></li>
          <li><a href="#endpoints" class="text-blue-600 hover:underline">2. API Endpoints</a></li>
          <li><a href="#responses" class="text-blue-600 hover:underline">3. Response Format</a></li>
          <li><a href="#errors" class="text-blue-600 hover:underline">4. Error Handling</a></li>
          <li><a href="#rate-limiting" class="text-blue-600 hover:underline">5. Rate Limiting</a></li>
        </ul>
      </div>
      
      <section id="authentication" class="mb-12">
        <h2 class="text-2xl font-semibold mb-4">1. Authentication</h2>
        
        <p class="mb-4">
          Shadow supports API tokens for authentication. Include your token in the Authorization header with all requests.
        </p>
        
        <h3 class="text-xl font-semibold mb-2">Bearer Token</h3>
        <div class="bg-slate-800 rounded-lg p-4 mb-6">
          <pre class="text-green-400">
Authorization: Bearer &lt;your-token&gt;</pre>
        </div>
        
        <h3 class="text-xl font-semibold mb-2">Obtaining a Token</h3>
        <p class="mb-4">You can generate API tokens through the authentication endpoints:</p>
        <div class="bg-slate-800 rounded-lg p-4 mb-6">
          <pre class="text-green-400">
POST /api/auth/login
{
  "email": "user@example.com",
  "password": "your-password"
}

// Response
{
  "token": "eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9...",
  "expires_at": "2023-12-31T23:59:59Z"
}</pre>
        </div>
      </section>
      
      <section id="endpoints" class="mb-12">
        <h2 class="text-2xl font-semibold mb-4">2. API Endpoints</h2>
        
        <h3 class="text-xl font-semibold mb-2">Resource Endpoints</h3>
        <p class="mb-4">Shadow automatically generates RESTful endpoints for your models:</p>
        <div class="bg-slate-800 rounded-lg p-4 mb-6">
          <pre class="text-green-400">
GET    /api/{resource}           # List resources
POST   /api/{resource}           # Create a resource
GET    /api/{resource}/{id}      # Get a specific resource
PUT    /api/{resource}/{id}      # Update a resource
DELETE /api/{resource}/{id}      # Delete a resource</pre>
        </div>
        
        <h3 class="text-xl font-semibold mb-2">Query Parameters</h3>
        <p class="mb-4">Customize your requests with query parameters:</p>
        <div class="bg-slate-800 rounded-lg p-4 mb-6">
          <pre class="text-green-400">
# Filtering
GET /api/users?filter[status]=active

# Sorting
GET /api/users?sort=-created_at

# Pagination
GET /api/users?page=2&per_page=25

# Including relationships
GET /api/users?include=posts,comments

# Selecting fields
GET /api/users?fields=id,name,email</pre>
        </div>
      </section>
      
      <section id="responses" class="mb-12">
        <h2 class="text-2xl font-semibold mb-4">3. Response Format</h2>
        
        <p class="mb-4">
          Shadow API responses follow a consistent format:
        </p>
        
        <div class="bg-slate-800 rounded-lg p-4 mb-6">
          <pre class="text-green-400">
{
  "data": {
    // Resource data
  },
  "meta": {
    "pagination": {
      "total": 100,
      "count": 25,
      "per_page": 25,
      "current_page": 1,
      "total_pages": 4,
      "links": {
        "next": "https://api.example.com/users?page=2"
      }
    }
  }
}</pre>
        </div>
      </section>
      
      <section id="errors" class="mb-12">
        <h2 class="text-2xl font-semibold mb-4">4. Error Handling</h2>
        
        <p class="mb-4">
          Shadow returns standardized error responses:
        </p>
        
        <div class="bg-slate-800 rounded-lg p-4 mb-6">
          <pre class="text-green-400">
{
  "error": {
    "code": "validation_error",
    "message": "The given data was invalid.",
    "details": {
      "email": [
        "The email field is required."
      ]
    }
  }
}</pre>
        </div>
        
        <h3 class="text-xl font-semibold mb-2">Common Error Codes</h3>
        <ul class="list-disc pl-6 space-y-2 text-gray-700">
          <li><strong>validation_error</strong>: Input validation failed</li>
          <li><strong>authentication_error</strong>: Authentication failed</li>
          <li><strong>authorization_error</strong>: Insufficient permissions</li>
          <li><strong>resource_not_found</strong>: Requested resource not found</li>
          <li><strong>rate_limit_exceeded</strong>: Too many requests</li>
        </ul>
      </section>
      
      <section id="rate-limiting" class="mb-12">
        <h2 class="text-2xl font-semibold mb-4">5. Rate Limiting</h2>
        
        <p class="mb-4">
          Shadow implements rate limiting to protect your API from abuse. Rate limit information is included in response headers:
        </p>
        
        <div class="bg-slate-800 rounded-lg p-4 mb-6">
          <pre class="text-green-400">
X-RateLimit-Limit: 60
X-RateLimit-Remaining: 59
X-RateLimit-Reset: 1609459200</pre>
        </div>
        
        <p class="mb-4">
          When a rate limit is exceeded, the API returns a 429 Too Many Requests response with a Retry-After header.
        </p>
      </section>
    </div>
  </div>
</template>

<script setup>
</script>
