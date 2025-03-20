<template>
  <div class="max-w-4xl mx-auto">
    <UContainer class="max-w-8xl py-12 space-y-6">
      <h1 class="text-4xl sm:text-6xl lg:text-7xl tracking-tight text-gray-800 font-bold text-center font-display max-w-4xl mx-auto">
        Contributing to Shadow
      </h1>

      <p class="text-xl text-gray-600 text-center max-w-3xl mx-auto">
        Thank you for considering contributing to Shadow! This document outlines the process and guidelines for contributing to the project.
      </p>

      <div class="w-full flex justify-center py-4">
          <ButtonLink label="Documentation" size="sm" rounded="md" href="/" class="w-full max-w-md" />
      </div>

      <div class="bg-gray-100 p-6 rounded-lg mb-8">
        <h2>Welcome Contributors!</h2>
        
        <p>By participating in this project, you agree to abide by our <NuxtLink to="/code-of-conduct" class="text-blue-600 hover:underline">Code of Conduct</NuxtLink>.</p>

        <h2>How Can I Contribute?</h2>
        
        <h3>Reporting Bugs</h3>
        <ul>
          <li>Use the GitHub issue tracker</li>
          <li>Check if the bug has already been reported</li>
          <li>Include detailed steps to reproduce the bug</li>
          <li>Include system information and stack traces if applicable</li>
        </ul>

        <h3>Suggesting Enhancements</h3>
        <ul>
          <li>First discuss the enhancement via GitHub issues</li>
          <li>Provide a clear use case for the enhancement</li>
          <li>Consider the scope and impact on existing features</li>
        </ul>

        <h3>Pull Requests</h3>
        <ul>
          <li>Fork the repository and create a branch for your changes</li>
          <li>Follow the coding style and conventions</li>
          <li>Include tests for new functionality</li>
          <li>Update documentation as needed</li>
          <li>Ensure CI passes and maintain code quality</li>
        </ul>

        <h2 class="text-2xl font-semibold mb-4">Setting Up the Development Environment</h2>
        <p class="text-gray-700 mb-4">Clone the repository and install dependencies:</p>
        <pre class="bg-slate-800 rounded-lg p-4 mb-6 text-green-400"><code>git clone https://github.com/yourusername/shadow.git
cd shadow
composer install
npm install</code></pre>

        <h2>Testing</h2>
        
        <p>We use Vitest for unit testing. Run tests with:</p>
        <pre><code>pnpm test</code></pre>

        <h2>Style Guide</h2>
        
        <ul>
          <li>Use TypeScript for all new code</li>
          <li>Follow ESLint and Prettier configurations</li>
          <li>Write meaningful commit messages following conventional commits</li>
          <li>Document new features and changes</li>
        </ul>

        <h2 class="text-2xl font-semibold mb-4">Community</h2>
        <p class="text-gray-700 mb-4">Join our community channels:</p>
        <ul class="list-disc pl-6 mb-6 space-y-2">
          <li>Discord: <a href="#" class="text-blue-600 hover:underline">Shadow Community</a></li>
          <li>Twitter: <a href="#" class="text-blue-600 hover:underline">@ShadowFramework</a></li>
          <li>GitHub Discussions: <a href="#" class="text-blue-600 hover:underline">Shadow Discussions</a></li>
        </ul>

        <h2>Questions?</h2>
        
        <p>If you have questions about contributing, feel free to:</p>
        <ul>
          <li>Open a GitHub Discussion</li>
          <li>Join our Discord community</li>
          <li>Check our documentation</li>
        </ul>
      </div>
    </UContainer>
  </div>
</template>

<script setup lang="ts">
// Component logic can be added here if needed
</script> 