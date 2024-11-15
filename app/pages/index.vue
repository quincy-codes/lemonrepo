<template>
  <main class="max-w-8xl mx-auto">
    <HomeHero :count="count" />
<!--    <HomeVideoIntro />-->
    <HomeStickyFeatures />
<!--    <HomeMoreFeatures />-->
<!--    <HomeGithubChangelog />-->
<!--    <HomeTestimonials />-->
    <HomeFaq />
  </main>
</template>

<script setup>
const { data: count } = await useFetch("/api/purchase-count", {
  server: false,
});
</script>
