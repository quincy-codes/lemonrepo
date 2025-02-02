<template>
  <div class="max-w-4xl mx-auto">
    <UContainer class="max-w-8xl py-12 space-y-6">
      <h1 class="text-4xl sm:text-6xl lg:text-7xl tracking-tight text-gray-800 font-bold text-center font-display max-w-4xl mx-auto">
        Welcome to Evolve Documentation
      </h1>

      <p class="text-center text-gray-600 mb-8">
        Explore the various guides and documentation pages to get the most out of Evolve.
      </p>

      <div class="grid grid-cols-1 sm:grid-cols-2 md:grid-cols-3 gap-4">
        <ButtonLink label="Introduction" size="sm" rounded="md" href="/introduction" />
        <ButtonLink label="Getting Started" size="sm" rounded="md" href="/getting-started" />
        <ButtonLink label="Core Functionality" size="sm" rounded="md" href="/core-functionality" />
        <ButtonLink label="API Reference" size="sm" rounded="md" href="/api-reference" />
        <ButtonLink label="Configuration Guide" size="sm" rounded="md" href="/configuration-guide" />
        <ButtonLink label="Performance Optimization" size="sm" rounded="md" href="/performance-optimization" />
        <ButtonLink label="Caching Guide" size="sm" rounded="md" href="/caching-guide" />
        <ButtonLink label="Authentication" size="sm" rounded="md" href="/authentication" />
        <ButtonLink label="Authorization" size="sm" rounded="md" href="/authorization" />
        <ButtonLink label="Database Management" size="sm" rounded="md" href="/database-management" />
        <ButtonLink label="Security Best Practices" size="sm" rounded="md" href="/security-best-practices" />
        <ButtonLink label="Advanced Usage" size="sm" rounded="md" href="/advanced-usage" />
        <ButtonLink label="Extending Evolve" size="sm" rounded="md" href="/extending-evolve" />
        <ButtonLink label="Contributing" size="sm" rounded="md" href="/contributing" />
        <ButtonLink label="License" size="sm" rounded="md" href="/license" />
      </div>
    </UContainer>
  </div>
</template>

<script setup>
</script>
