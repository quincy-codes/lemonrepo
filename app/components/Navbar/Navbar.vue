<template>
  <header class="sticky top-0 z-50 bg-white border-b border-gray-100">
    <UContainer
      class="mx-auto flex h-16 items-center justify-between px-4 transition-all"
    >
      <div class="flex items-center gap-5">
        <NuxtLink to="/" class="flex items-center gap-2">
          <p class="font-bold tracking-tight">Think Neverland : Shadow</p>
        </NuxtLink>
        <NavbarWhatsNew />
      </div>
      <div class="hidden items-center gap-2 sm:flex">
        <UButton to="/#faq" variant="ghost" color="gray">FAQ</UButton>
        <UButton
          href="https://think-neverland.gitbook.io/thinkneverland/"
          target="_blank"
          variant="ghost"
          color="gray"
        >Docs</UButton>
        <StripeBuyButton />
      </div>
      <div class="sm:hidden flex items-center gap-2">
        <StripeBuyButton />
        <button
          aria-label="Toggle menu"
          class="menu-toggle relative flex h-8 w-8 items-center justify-center rounded-full border bg-white hover:bg-gray-100 flex-shrink-0"
          @click="toggleMobileMenu"
          :class="{ active: mobileMenu }"
        >
          <div
            class="menu-bar absolute h-[1.5px] w-[15px] translate-y-[-3px] bg-gray-700 transition-all duration-200 ease-out"
            data-position="top"
          ></div>
          <div
            class="menu-bar absolute h-[1.5px] w-[15px] translate-y-[3px] bg-gray-700 transition-all duration-200 ease-out"
            data-position="bottom"
          ></div>
        </button>
      </div>
    </UContainer>
    <div
      class="block h-[calc(100vh-4rem)] w-full sm:hidden"
      v-show="mobileMenu"
    >
      <div class="flex flex-col gap-4 p-4">
        <UButton
          to="/#faq"
          @click="toggleMobileMenu"
          variant="ghost"
          color="gray"
          >FAQ</UButton
        >
        <UButton
          href="https://think-neverland.gitbook.io/thinkneverland/"
          target="_blank"
          @click="toggleMobileMenu"
          variant="ghost"
          color="gray"
          >Docs</UButton
        >
      </div>
    </div>
  </header>
</template>

<script setup>
import { ref } from 'vue'
import StripeBuyButton from '~/components/StripeBuyButton.vue'

const mobileMenu = ref(false)

const toggleMobileMenu = () => {
  document.body.style.overflow = mobileMenu.value ? "auto" : "hidden"
  mobileMenu.value = !mobileMenu.value
}
</script>

<style scoped>
.menu-toggle.active .menu-bar[data-position="top"] {
  transform: translateY(0) rotate(45deg) scaleX(1.05);
}

.menu-toggle.active .menu-bar[data-position="bottom"] {
  transform: translateY(0) rotate(-45deg) scaleX(1.05);
}
</style>
