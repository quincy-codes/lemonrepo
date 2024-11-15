<template>
  <div class="max-w-4xl mx-auto">
    <UContainer class="max-w-8xl py-12 space-y-6">
        <h1 class="text-4xl sm:text-6xl lg:text-7xl tracking-tight text-gray-800 font-bold text-center font-display max-w-4xl mx-auto">
          Evolve Documentation
        </h1>
        <div class="w-full columns-3 py-4" >
          <ButtonLink label="Core" size="sm" rounded="md" href="/evolve-core-documentation" />
          <ButtonLink label="API" size="sm" rounded="md" href="/evolve-api-documentation" />
          <ButtonLink label="UI" size="sm" rounded="md" href="/evolve-UI-documentation" />
        </div>

        <div class="bg-blue-50 p-6 rounded-lg mb-12 mt-8">
          <p class="text-lg text-gray-700 leading-relaxed">
            Evolve is a comprehensive framework built on top of Laravel that streamlines the development of robust web applications. It consists of three main components that work seamlessly together to provide a complete development solution.
          </p>
        </div>

        <div class="grid grid-cols-1 md:grid-cols-3 gap-6 mb-16">
          <a href="/evolve-core-documentation" class="block p-6 bg-white rounded-lg shadow hover:shadow-lg transition-shadow">
            <h3 class="text-xl font-semibold mb-2">Core Package</h3>
            <p class="text-gray-600">Foundation layer providing enhanced model capabilities, validation, filtering, and authorization.</p>
            <div class="mt-4 text-sm text-blue-600">
              View Documentation →
            </div>
          </a>
          <a href="/evolve-api-documentation" class="block p-6 bg-white rounded-lg shadow hover:shadow-lg transition-shadow">
            <h3 class="text-xl font-semibold mb-2">API Package</h3>
            <p class="text-gray-600">RESTful API implementation with automatic endpoint generation and response handling.</p>
            <div class="mt-4 text-sm text-blue-600">
              View Documentation →
            </div>
          </a>
          <a href="/evolve-ui-documentation" class="block p-6 bg-white rounded-lg shadow hover:shadow-lg transition-shadow">
            <h3 class="text-xl font-semibold mb-2">UI Package</h3>
            <p class="text-gray-600">Pre-built UI components and interfaces that integrate with Core and API packages.</p>
            <div class="mt-4 text-sm text-blue-600">
              View Documentation →
            </div>
          </a>
        </div>

        <h2 class="text-center mx-auto max-w-xl text-2xl sm:text-2xl font-semibold tracking-tight text-gray-900 font-display mb-4">
          Package Overview
        </h2>

        <div class="space-y-8">
          <div class="bg-white p-6 rounded-lg shadow">
            <h3 class="text-xl font-semibold mb-4">Evolve Core</h3>
            <p class="text-gray-600 mb-4">
              The foundation of the Evolve ecosystem, providing enhanced model functionality with automatic:
            </p>
            <ul class="list-disc ml-6 text-gray-600">
              <li>Database-aware validation</li>
              <li>Advanced filtering and sorting</li>
              <li>Event handling</li>
              <li>Policy-based authorization</li>
              <li>Relationship management</li>
            </ul>
            <ButtonLink label="View Documentation →" size="sm" rounded="md" href="/evolve-core-documentation" />
          </div>

          <div class="bg-white p-6 rounded-lg shadow">
            <h3 class="text-xl font-semibold mb-4">Evolve API</h3>
            <p class="text-gray-600 mb-4">
              Builds on Core to provide RESTful API functionality with:
            </p>
            <ul class="list-disc ml-6 text-gray-600">
              <li>Automatic endpoint generation</li>
              <li>Standardized response formatting</li>
              <li>Built-in pagination</li>
              <li>Query parameter handling</li>
              <li>Error handling and logging</li>
            </ul>
            <ButtonLink label="View Documentation →" size="sm" rounded="md" href="/evolve-api-documentation" />

          </div>

          <div class="bg-white p-6 rounded-lg shadow">
            <h3 class="text-xl font-semibold mb-4">Evolve UI</h3>
            <p class="text-gray-600 mb-4">
              Complete frontend solution that integrates with Core and API:
            </p>
            <ul class="list-disc ml-6 text-gray-600">
              <li>Pre-built CRUD interfaces</li>
              <li>Form generation and validation</li>
              <li>Data tables with sorting and filtering</li>
              <li>Authentication views</li>
              <li>Customizable components</li>
            </ul>
            <ButtonLink label="View Documentation →" size="sm" rounded="md" href="/evolve-ui-documentation" />
          </div>
        </div>

        <div class="bg-yellow-50 border-l-4 border-yellow-400 p-6 mt-12">
          <h4 class="font-semibold mb-2">Getting Started</h4>
          <p class="text-gray-700">
            We MUST the EvolveCore package installation and then progressively adding the optional API and optional UI packages as needed. Each package can be used independently, but they're designed to work together seamlessly.
          </p>
        </div>
    </UContainer>
  </div>
</template>
<script setup lang="ts">
</script>