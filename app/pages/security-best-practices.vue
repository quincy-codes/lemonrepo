<template>
  <div class="max-w-4xl mx-auto">
    <UContainer class="max-w-8xl py-12 space-y-6">
      <h1 class="text-4xl sm:text-6xl lg:text-7xl tracking-tight text-gray-800 font-bold text-center font-display max-w-4xl mx-auto">
        Security Best Practices
      </h1>

      <h2 class="text-2xl font-semibold text-gray-900">Token Security</h2>
      <p class="text-gray-700">Ensure tokens are securely stored and refreshed regularly.</p>

      <pre class="bg-gray-800 rounded-lg p-4 overflow-x-auto text-green-400">
        <code>
$token = $user->createToken('api-token', ['read', 'write']);
        </code>
      </pre>
    </UContainer>
  </div>
</template>

<script setup>
</script>
