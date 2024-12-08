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
    label: "What is Evolve?",
    content:
        "Evolve is a modular Laravel package suite designed to provide powerful, customizable CRUD operations, RESTful API endpoints, and a Livewire-based user interface. Evolve is split into three packages—EvolveCORE, EvolveAPI, and EvolveUI—so you can tailor it to your app’s needs.",
  },
  {
    label: "What are the three packages?",
    content:
        "Evolve consists of three packages: EvolveCORE, EvolveAPI, and EvolveUI. EvolveCORE is the foundational package that provides core CRUD functionality, EvolveAPI adds RESTful endpoints for CRUD actions, and EvolveUI provides a Livewire-powered user interface. EvolveAPI and EvolveUI are optional extensions.",
  },
  {
    label: "What features are provided by each package?",
    content:
        "EvolveCORE: Model-defined configuration, dynamic filtering and sorting, permissions, event hooks, and validation.\n\nEvolveAPI: API routes with standardized JSON responses, batch operations, and optional Swagger documentation.\n\nEvolveUI: A responsive UI with searchable, filterable, and sortable fields, along with a role-based dynamic menu.",
  },
  {
    label: "How do I install the packages?",
    content:
        "Each package can be installed individually via Composer. Start with EvolveCORE as the base package, and add EvolveAPI and/or EvolveUI as needed for additional API or UI functionality.",
  },
  {
    label: "Can I customize fields, filters, and permissions?",
    content:
        "Yes! In EvolveCORE, each model can define its own displayable fields, searchable fields, filterable fields, and validation rules. Permissions can be set up using Spatie Permissions if installed.",
  },
  {
    label: "How do CRUD event hooks work?",
    content:
        "Hooks like `beforeCreate` and `afterUpdate` are defined directly in controllers named after each model, allowing you to insert custom logic at different stages of CRUD operations.",
  },
  {
    label: "What is the tech stack for Evolve?",
    content:
        "Laravel (10+), Livewire (3+), DaisyUI (with Tailwind CSS), Spatie Permissions (Optional), Swagger (Optional)."
  },
  {
    label: "How does filtering work with nested relationships?",
    content:
        "Evolve’s filtering supports infinitely nested relationships using dot notation (e.g., `department.manager.name`). You can define filterable relationships and fields within each model.",
  },
  {
    label: "Is EvolveUI dependent on EvolveAPI?",
    content:
        "No, EvolveUI only requires EvolveCORE. You can use EvolveUI without EvolveAPI if you only need a UI for CRUD operations without API endpoints.",
  },
  {
    label: "What optional packages are supported?",
    content:
        "Evolve integrates with Spatie Permissions for role-based access, DaisyUI for styling, and Swagger for API documentation (optional with EvolveAPI).",
  },
  {
    label: "Where can I find API documentation for EvolveAPI?",
    content:
        "If Swagger is installed, API documentation is generated automatically and can be accessed at `/api/documentation`.",
  },
  {
    label: "Is Evolve suitable for large applications?",
    content:
        "Yes, Evolve is designed to scale. Its modular design and configuration-based control make it suitable for both small projects and large applications with complex relationships and access requirements.",
  },
  {
    label: "Can I use Evolve with my existing models?",
    content:
        "Absolutely. Simply add the EvolveModelInterface and EvolveTrait to your models, and they’ll be ready to interact with the evolve ecosystem, inheriting configuration and CRUD capabilities.",
  },
];
</script>
