<template>
  <div class="max-w-4xl mx-auto">
    <UContainer class="max-w-8xl py-12 space-y-6">
      <h1 class="text-4xl sm:text-6xl lg:text-7xl tracking-tight text-gray-800 font-bold text-center font-display max-w-4xl mx-auto">
        Changelog
      </h1>

      <div class="w-full flex justify-center py-4">
          <ButtonLink label="Documentation" size="sm" rounded="md" href="/" class="w-full max-w-md" />
      </div>

      <div class="bg-gray-100 p-6 rounded-lg mb-8">
        <div class="prose prose-lg max-w-none">
          <p class="lead">All notable changes to the Shadow framework will be documented in this file.</p>

          <h2>[1.0.0] - 2024-03-20</h2>
          
          <h3>Added</h3>
          <ul>
            <li>Initial release of Shadow framework</li>
            <li>Core framework architecture and components</li>
            <li>Basic authentication and authorization system</li>
            <li>Database integration and ORM</li>
            <li>RESTful API support</li>
            <li>Caching system</li>
            <li>Event system</li>
            <li>Middleware support</li>
            <li>Configuration management</li>
            <li>Comprehensive documentation</li>
          </ul>

          <h2>[0.9.0] - 2024-02-15</h2>
          
          <h3>Added</h3>
          <ul>
            <li>Beta release with core features</li>
            <li>Testing framework integration</li>
            <li>Performance optimization tools</li>
          </ul>

          <h3>Changed</h3>
          <ul>
            <li>Improved error handling</li>
            <li>Enhanced security features</li>
            <li>Updated documentation</li>
          </ul>

          <h3>Fixed</h3>
          <ul>
            <li>Various bug fixes and improvements</li>
            <li>Performance issues in database queries</li>
            <li>Security vulnerabilities</li>
          </ul>

          <h2>[0.8.0] - 2024-01-10</h2>
          
          <h3>Added</h3>
          <ul>
            <li>Alpha release for early adopters</li>
            <li>Basic framework structure</li>
            <li>Initial documentation</li>
          </ul>

          <h3>Changed</h3>
          <ul>
            <li>Refactored core components</li>
            <li>Updated dependency requirements</li>
          </ul>

          <h2>Earlier Versions</h2>
          
          <p>Earlier versions were internal development releases and are not documented here.</p>

          <h2>Version Format</h2>
          
          <p>We follow <a href="https://semver.org/" target="_blank" class="text-blue-600 hover:underline">Semantic Versioning</a> for version numbers:</p>
          <ul>
            <li>MAJOR version for incompatible API changes</li>
            <li>MINOR version for backwards-compatible functionality additions</li>
            <li>PATCH version for backwards-compatible bug fixes</li>
          </ul>

          <h2>Contributing to the Changelog</h2>
          
          <p>When contributing to the project, please update the changelog following these guidelines:</p>
          <ul>
            <li>Use the present tense ("Add feature" not "Added feature")</li>
            <li>Use imperative mood ("Move cursor to..." not "Moves cursor to...")</li>
            <li>Categorize changes under Added, Changed, Deprecated, Removed, Fixed, or Security</li>
            <li>Version numbers should be linked when possible</li>
          </ul>
        </div>
      </div>
    </UContainer>
  </div>
</template>

<script setup lang="ts">
// Component logic can be added here if needed
</script> 