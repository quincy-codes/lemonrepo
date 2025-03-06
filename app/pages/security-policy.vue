<template>
  <div class="max-w-4xl mx-auto">
    <UContainer class="max-w-8xl py-12 space-y-6">
      <h1 class="text-4xl sm:text-6xl lg:text-7xl tracking-tight text-gray-800 font-bold text-center font-display max-w-4xl mx-auto">
        Security Policy
      </h1>

      <div class="w-full columns-2 py-4">
          <ButtonLink label="Documentation" size="sm" rounded="md" href="/" />
      </div>

      <div class="prose prose-lg max-w-none">
        <h2>Security Policy</h2>
        
        <p>We take the security of Shadow framework seriously. If you believe you have found a security vulnerability, please report it to us as described below.</p>

        <h2>Supported Versions</h2>
        
        <table class="min-w-full">
          <thead>
            <tr>
              <th>Version</th>
              <th>Supported</th>
            </tr>
          </thead>
          <tbody>
            <tr>
              <td>1.0.x</td>
              <td>✅</td>
            </tr>
            <tr>
              <td>0.9.x</td>
              <td>✅</td>
            </tr>
            <tr>
              <td>< 0.9.0</td>
              <td>❌</td>
            </tr>
          </tbody>
        </table>

        <h2>Reporting a Vulnerability</h2>
        
        <p>Please follow these steps to report a security vulnerability:</p>
        <ol>
          <li>DO NOT report security vulnerabilities through public GitHub issues.</li>
          <li>Email your findings to security@shadowframework.dev</li>
          <li>Include detailed steps to reproduce the vulnerability</li>
          <li>If possible, provide a proof of concept</li>
          <li>Allow up to 48 hours for an initial response</li>
        </ol>

        <h2>Response Timeline</h2>
        
        <ul>
          <li>Initial Response: Within 48 hours</li>
          <li>Vulnerability Assessment: Within 1 week</li>
          <li>Security Patch: Timeline varies based on severity</li>
        </ul>

        <h2>Security Best Practices</h2>
        
        <p>When using Shadow framework, we recommend following these security practices:</p>
        <ul>
          <li>Keep Shadow and all dependencies up to date</li>
          <li>Use HTTPS for all production deployments</li>
          <li>Implement proper authentication and authorization</li>
          <li>Enable rate limiting for APIs</li>
          <li>Use secure session configurations</li>
          <li>Implement proper input validation</li>
          <li>Enable security headers</li>
          <li>Regular security audits</li>
          <li>Follow the principle of least privilege</li>
          <li>Implement proper logging and monitoring</li>
        </ul>

        <h2>Security Features</h2>
        
        <p>Shadow includes several built-in security features:</p>
        <ul>
          <li>CSRF Protection</li>
          <li>XSS Prevention</li>
          <li>SQL Injection Protection</li>
          <li>Authentication System</li>
          <li>Authorization Framework</li>
          <li>Data Encryption</li>
        </ul>

        <h2>Disclosure Policy</h2>
        
        <p>When we receive a security bug report, we will:</p>
        <ol>
          <li>Confirm the problem and determine affected versions</li>
          <li>Audit code to find any similar problems</li>
          <li>Prepare fixes for all supported versions</li>
          <li>Release new versions and notify users</li>
        </ol>

        <h2>Contact</h2>
        
        <p>For security-related inquiries, please contact:</p>
        <p>Email: security@shadowframework.dev</p>
        <p>PGP Key: <a href="#" class="text-blue-600 hover:underline">Download PGP Key</a></p>
      </div>
    </UContainer>
  </div>
</template>

<script setup lang="ts">
// Component logic can be added here if needed
</script> 