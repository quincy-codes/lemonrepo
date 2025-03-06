<template>
  <div id="faq" class="py-24 px-4 relative">
    <div class="relative max-w-4xl mx-auto">
      <div class="mb-16 text-center ">
        <h2
          class="mx-auto max-w-xl text-4xl sm:text-5xl font-semibold tracking-tight text-gray-900 font-display mb-4"
        >
          Got Questions? We've got some answers!
        </h2>
        <p class="mx-auto text-center mt-12 max-w-2xl text-gray-600 text-2xl">
          If you have any other questions, feel free to reach out to me at
          <button
            variant="link"
            class="underline"
            :padded="false"
            color="gray"
            @click="copy('hello@thinkneverland.com')"
          >
            {{ copied ? "Copied to clipboard" : "hello@thinkneverland.com" }}
          </button>
          We're happy to help!
        </p>
      </div>
      <UAccordion
        :items="items"
        multiple
        color="gray"
        variant="soft"
        size="xl"
        :ui="{ item: { size: 'text-base leading-relaxed', padding: 'py-4 pl-4', base: 'max-w' } }"
      />
    </div>
  </div>
</template>

<script setup>
import { useClipboard } from "@vueuse/core";
const { copy, copied } = useClipboard();

const items = [
  {
    label: "What features are provided?",
    content:
        "Model-defined configuration, dynamic filtering and sorting, permissions, event hooks, and validation. API routes with standardized JSON responses, batch operations, and optional Swagger documentation.",
  },
  {
    label: "Can I customize fields, filters, and permissions?",
    content:
        "Yes! In Shadow, each model can define its own displayable fields, searchable fields, filterable fields, and validation rules. Permissions can be set up using Spatie Permissions if installed.",
  },
  {
    label: "How do CRUD event hooks work?",
    content:
        "Hooks like `beforeCreate` and `afterUpdate` are defined directly in controllers named after each model, allowing you to insert custom logic at different stages of CRUD operations.",
  },
  {
    label: "What is the tech stack for Shadow?",
    content:
        "Shadow is built on Laravel and follows Laravel's best practices. It integrates seamlessly with Laravel's existing features like Eloquent ORM, validation, and authentication.",
  },
  {
    label: "Is Shadow suitable for large applications?",
    content:
        "Yes, Shadow is designed to scale. Its modular design and configuration-based control make it suitable for both small projects and large applications with complex relationships and access requirements.",
  },
  {
    label: "Can I use Shadow with my existing models?",
    content:
        "Absolutely! Shadow is designed to work with your existing models. Simply add the HasShadow trait and configure the options you need.",
  },
];
</script>
