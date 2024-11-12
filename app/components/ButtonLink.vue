<template>
  <NuxtLink
      class="shiny-button bg-black text-white focus:ring-2 focus:ring-black/20 w-full block text-center"
      :href="href"
      :class="[sizeClass, roundedClass]"
  >
    {{ label }}
    <span v-if="strikeThroughPrice" class="relative ml-1">
      <span class="h-0.5 w-[120%] bg-red-500 absolute top-1/2 -translate-y-1/2 left-1/2 -translate-x-1/2 -rotate-12" />
      {{ strikeThroughPrice }}
    </span>
  </NuxtLink>
</template>

<script lang="ts" setup>
import { computed } from 'vue';

const props = defineProps<{
  label: string;
  size: "sm" | "md" | "lg";
  rounded: "sm" | "md" | "lg" | "full";
  strikeThroughPrice?: string;
  href: string; // Add href prop
}>();

const sizeClass = computed(() => {
  return {
    sm: "text-sm px-3 py-2",
    md: "text-md px-4 py-3",
    lg: "text-base sm:text-lg px-5 py-3",
  }[props.size];
});

const roundedClass = computed(() => {
  return {
    sm: "rounded-sm",
    md: "rounded-md",
    lg: "rounded-lg",
    full: "rounded-full",
  }[props.rounded];
});
</script>
