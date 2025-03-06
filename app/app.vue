<template>
  <NuxtRouteAnnouncer />
  <NuxtLoadingIndicator color="#1e293b" />
  <NuxtLayout>
    <NuxtPage />
  </NuxtLayout>
  <Toaster position="top-center" />
</template>

<script setup>
import { Toaster } from "vue-sonner";
useSeoMeta({
  title: "Think Neverland - Shadow | Powerful Modular Laravel CRUD, API, and UI Solutions",
  description: "A Laravel suite that follows your application, adapting seamlessly in the background. Build, refine, and scale your vision with powerful, flexible tools at your fingertips.",
  ogImage: "https://assets.zyrosite.com/Y4LJNpxjPJc2N8ky/banner-dJo4a24WwgTG2lqW.png",
});
</script>

<style>
html {
  background-color: #ffffff;
}

.shiny-button {
  @apply relative ring-black/20 transition-shadow duration-300 before:absolute before:inset-0 before:rounded-[inherit] before:bg-[linear-gradient(45deg,transparent_25%,theme(colors.white/.3)_50%,transparent_75%,transparent_100%)] before:bg-[length:250%_250%,100%_100%] before:bg-[position:200%_0,0_0] before:bg-no-repeat before:[transition:background-position_0s_ease] hover:ring hover:before:bg-[position:-100%_0,0_0] hover:before:duration-[1500ms];
}

.metal-shadow {
  box-shadow: 0 0 0 1px #0e3f7e0f, 0 1px 1px -0.5px #2a334608,
    0 2px 2px -1px #2a33460a, 0 3px 3px -1.5px #2a33460a,
    0 5px 5px -2.5px #2a334608, 0 10px 10px -5px #2a334608,
    0 24px 24px -8px #2a334608;
}
</style>
