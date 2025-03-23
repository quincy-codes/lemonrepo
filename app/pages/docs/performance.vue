<template>
  <div class="max-w-4xl mx-auto">
    <UContainer class="max-w-8xl py-12 space-y-6">
      <h1 class="text-4xl sm:text-6xl lg:text-7xl tracking-tight text-gray-800 font-bold text-center font-display max-w-4xl mx-auto">
        Performance
      </h1>

      <div class="w-full flex justify-center py-4">
        <ButtonLink label="Documentation" size="sm" rounded="md" href="/docs" class="w-full max-w-md" />
      </div>

      <div class="prose prose-lg max-w-none">
        <ContentDoc path="/performance">
          <template #not-found>
            <p>This page could not be found</p>
          </template>
          <template #empty>
            <p>This page is empty</p>
          </template>
        </ContentDoc>
      </div>
    </UContainer>
  </div>
</template>

<script setup lang="ts">
// Component logic can be added here if needed
</script> 