<template>
  <div class="max-w-4xl mx-auto">
    <UContainer class="max-w-8xl py-12 space-y-6">
      <div class="flex items-center justify-between mb-8">
        <h1 class="text-4xl sm:text-6xl lg:text-7xl tracking-tight text-gray-800 font-bold font-display">
          Advanced Usage Guide
        </h1>
        <ButtonLink 
          label="Back to Documentation" 
          size="sm" 
          rounded="md" 
          href="/docs" 
          class="flex items-center gap-2"
        >
          <Icon name="heroicons:arrow-left" class="w-4 h-4" />
          Back
        </ButtonLink>
      </div>

      <div class="prose prose-lg max-w-none">
        <ContentDoc path="/docs/advanced-usage" />
      </div>
    </UContainer>
  </div>
</template>

<script setup lang="ts">
// Component logic can be added here if needed
</script> 