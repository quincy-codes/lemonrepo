<template>
  <div class="max-w-4xl mx-auto">
    <UContainer class="max-w-8xl py-12 space-y-6">
      <h1 class="text-4xl sm:text-6xl lg:text-7xl tracking-tight text-gray-800 font-bold text-center font-display max-w-4xl mx-auto">
        Authorization
      </h1>

      <h2 class="text-2xl font-semibold text-gray-900">Role-Based Access Control</h2>
      <p class="text-gray-700">Evolve provides robust role-based access control.</p>

      <pre class="bg-gray-800 rounded-lg p-4 overflow-x-auto text-green-400">
        <code>
$role = Role::create(['name' => 'admin']);
$user->assignRole('admin');
        </code>
      </pre>
    </UContainer>
  </div>
</template>

<script setup>
</script>
