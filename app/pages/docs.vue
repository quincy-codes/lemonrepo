<template>
  <div class="max-w-4xl mx-auto">
    <UContainer class="max-w-8xl py-12 space-y-6">
      <h1 class="text-4xl sm:text-6xl lg:text-7xl tracking-tight text-gray-800 font-bold text-center font-display max-w-4xl mx-auto">
        Shadow Documentation
      </h1>

      <p class="text-xl text-gray-600 text-center max-w-3xl mx-auto">
        A powerful and flexible API development framework for Laravel, designed to accelerate your API development while maintaining best practices and scalability.
      </p>

      <div class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3 gap-6 mt-12">
        <!-- Core Documentation -->
        <div class="bg-white rounded-lg shadow-md p-6">
          <h2 class="text-2xl font-semibold mb-4">Getting Started</h2>
          <ul class="space-y-3">
            <li>
              <NuxtLink to="/docs/getting-started" class="text-blue-600 hover:underline">Getting Started</NuxtLink>
              <p class="text-sm text-gray-600">Begin your journey with Shadow</p>
            </li>
            <li>
              <NuxtLink to="/docs/core" class="text-blue-600 hover:underline">Core Concepts</NuxtLink>
              <p class="text-sm text-gray-600">Fundamental principles and architecture</p>
            </li>
            <li>
              <NuxtLink to="/docs/config" class="text-blue-600 hover:underline">Configuration</NuxtLink>
              <p class="text-sm text-gray-600">Configure Shadow for your needs</p>
            </li>
          </ul>
        </div>

        <!-- Features -->
        <div class="bg-white rounded-lg shadow-md p-6">
          <h2 class="text-2xl font-semibold mb-4">Features</h2>
          <ul class="space-y-3">
            <li>
              <NuxtLink to="/docs/api" class="text-blue-600 hover:underline">API Development</NuxtLink>
              <p class="text-sm text-gray-600">Build robust and scalable APIs</p>
            </li>
            <li>
              <NuxtLink to="/docs/database" class="text-blue-600 hover:underline">Database Integration</NuxtLink>
              <p class="text-sm text-gray-600">Database features and optimization</p>
            </li>
            <li>
              <NuxtLink to="/docs/caching" class="text-blue-600 hover:underline">Caching System</NuxtLink>
              <p class="text-sm text-gray-600">Efficient caching strategies</p>
            </li>
          </ul>
        </div>

        <!-- Security -->
        <div class="bg-white rounded-lg shadow-md p-6">
          <h2 class="text-2xl font-semibold mb-4">Security</h2>
          <ul class="space-y-3">
            <li>
              <NuxtLink to="/docs/security" class="text-blue-600 hover:underline">Security Guide</NuxtLink>
              <p class="text-sm text-gray-600">Comprehensive security overview</p>
            </li>
            <li>
              <NuxtLink to="/docs/authentication" class="text-blue-600 hover:underline">Authentication</NuxtLink>
              <p class="text-sm text-gray-600">User authentication methods</p>
            </li>
            <li>
              <NuxtLink to="/docs/authorization" class="text-blue-600 hover:underline">Authorization</NuxtLink>
              <p class="text-sm text-gray-600">Access control and permissions</p>
            </li>
            <li>
              <NuxtLink to="/docs/security-policy" class="text-blue-600 hover:underline">Security Policy</NuxtLink>
              <p class="text-sm text-gray-600">Security guidelines and reporting</p>
            </li>
          </ul>
        </div>

        <!-- Advanced Topics -->
        <div class="bg-white rounded-lg shadow-md p-6">
          <h2 class="text-2xl font-semibold mb-4">Advanced Topics</h2>
          <ul class="space-y-3">
            <li>
              <NuxtLink to="/docs/advanced-usage" class="text-blue-600 hover:underline">Advanced Usage</NuxtLink>
              <p class="text-sm text-gray-600">Advanced features and patterns</p>
            </li>
            <li>
              <NuxtLink to="/docs/performance" class="text-blue-600 hover:underline">Performance</NuxtLink>
              <p class="text-sm text-gray-600">Optimization and tuning</p>
            </li>
            <li>
              <NuxtLink to="/docs/extending" class="text-blue-600 hover:underline">Extending Shadow</NuxtLink>
              <p class="text-sm text-gray-600">Customize and extend functionality</p>
            </li>
          </ul>
        </div>

        <!-- Community -->
        <div class="bg-white rounded-lg shadow-md p-6">
          <h2 class="text-2xl font-semibold mb-4">Community</h2>
          <ul class="space-y-3">
            <li>
              <a href="https://github.com/thinkneverland/shadow" target="_blank" class="text-blue-600 hover:underline">GitHub Repository</a>
              <p class="text-sm text-gray-600">Contribute to Shadow</p>
            </li>
            <li>
              <a href="https://discord.gg/shadow" target="_blank" class="text-blue-600 hover:underline">Discord Community</a>
              <p class="text-sm text-gray-600">Join our community</p>
            </li>
            <li>
              <a href="https://shadow.thinkneverland.com/blog" target="_blank" class="text-blue-600 hover:underline">Blog</a>
              <p class="text-sm text-gray-600">Latest news and tutorials</p>
            </li>
          </ul>
        </div>

        <!-- Support -->
        <div class="bg-white rounded-lg shadow-md p-6">
          <h2 class="text-2xl font-semibold mb-4">Support</h2>
          <ul class="space-y-3">
            <li>
              <a href="https://shadow.thinkneverland.com/support" target="_blank" class="text-blue-600 hover:underline">Professional Support</a>
              <p class="text-sm text-gray-600">Enterprise support options</p>
            </li>
            <li>
              <a href="https://github.com/thinkneverland/shadow/issues" target="_blank" class="text-blue-600 hover:underline">Issue Tracker</a>
              <p class="text-sm text-gray-600">Report bugs and request features</p>
            </li>
            <li>
              <a href="https://stackoverflow.com/questions/tagged/shadow" target="_blank" class="text-blue-600 hover:underline">Stack Overflow</a>
              <p class="text-sm text-gray-600">Community Q&A</p>
            </li>
          </ul>
        </div>

        <!-- Project Information -->
        <div class="bg-white rounded-lg shadow-md p-6">
          <h2 class="text-2xl font-semibold mb-4">Project Information</h2>
          <ul class="space-y-3">
            <li>
              <NuxtLink to="/docs/license" class="text-blue-600 hover:underline">License</NuxtLink>
              <p class="text-sm text-gray-600">License terms and conditions</p>
            </li>
            <li>
              <NuxtLink to="/docs/contributing" class="text-blue-600 hover:underline">Contributing</NuxtLink>
              <p class="text-sm text-gray-600">Guidelines for contributing</p>
            </li>
            <li>
              <NuxtLink to="/docs/changelog" class="text-blue-600 hover:underline">Changelog</NuxtLink>
              <p class="text-sm text-gray-600">Version history and updates</p>
            </li>
            <li>
              <NuxtLink to="/docs/code-of-conduct" class="text-blue-600 hover:underline">Code of Conduct</NuxtLink>
              <p class="text-sm text-gray-600">Community guidelines</p>
            </li>
          </ul>
        </div>
      </div>

      <div class="mt-12 text-center">
        <h2 class="text-2xl font-semibold mb-4">Quick Links</h2>
        <div class="flex flex-wrap justify-center gap-4">
          <NuxtLink to="/docs/getting-started" class="inline-flex items-center px-4 py-2 border border-transparent text-base font-medium rounded-md shadow-sm text-white bg-blue-600 hover:bg-blue-700">
            Get Started
          </NuxtLink>
          <a href="https://github.com/thinkneverland/shadow" target="_blank" class="inline-flex items-center px-4 py-2 border border-gray-300 text-base font-medium rounded-md shadow-sm text-gray-700 bg-white hover:bg-gray-50">
            View on GitHub
          </a>
        </div>
      </div>
    </UContainer>
  </div>
</template>

<script setup lang="ts">
// Component logic can be added here if needed
</script> 