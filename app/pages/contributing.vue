<template>
  <div class="max-w-4xl mx-auto">
    <UContainer class="max-w-8xl py-12 space-y-6">
      <h1 class="text-4xl sm:text-6xl lg:text-7xl tracking-tight text-gray-800 font-bold text-center font-display max-w-4xl mx-auto">
        Contributing to Evolve
      </h1>

      <div class="w-full columns-2 py-4">
          <ButtonLink label="Documentation" size="sm" rounded="md" href="/" />
      </div>

      <div class="prose prose-lg max-w-none">
        <h2>Welcome Contributors!</h2>
        
        <p>Thank you for considering contributing to Evolve! This document outlines the process and guidelines for contributing to the project.</p>

        <h2>Code of Conduct</h2>
        
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

        <h2>Development Setup</h2>
        
        <h3>Prerequisites</h3>
        <ul>
          <li>Node.js (v18 or higher)</li>
          <li>pnpm package manager</li>
          <li>Git</li>
        </ul>

        <h3>Local Development</h3>
        <pre><code>git clone https://github.com/yourusername/evolve.git
cd evolve
pnpm install
pnpm dev</code></pre>

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

        <h2>Community</h2>
        
        <p>Join our community:</p>
        <ul>
          <li>Discord: <a href="#" class="text-blue-600 hover:underline">Join our server</a></li>
          <li>Twitter: <a href="#" class="text-blue-600 hover:underline">@EvolveFramework</a></li>
          <li>GitHub Discussions: For questions and ideas</li>
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