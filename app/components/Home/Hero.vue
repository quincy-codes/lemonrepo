<template>
  <section class="pt-20 pb-12">
    <Logo class="h-8 w-auto" role="img" alt="Thing Neverland logo" />
    <h1
      class="text-4xl sm:text-6xl lg:text-7xl tracking-tight text-gray-800 font-bold text-center font-display max-w-4xl mx-auto"
    >
      Your site will change why shouldn't your
      <span
        class="inline-flex flex-col h-[calc(theme(fontSize.4xl)*theme(lineHeight.tight))] sm:h-[calc(theme(fontSize.6xl)*theme(lineHeight.tight))] lg:h-[calc(theme(fontSize.7xl)*theme(lineHeight.tight))] overflow-hidden"
      >
        <ul
          class="block text-left leading-tight [&_li]:block animate-text-slide-5"
        >
          <li>API</li>
          <li>Docs</li>
          <li>UI</li>
        </ul>
      </span>
      <span class="block"> Evolve Too?</span>
    </h1>
    <div class="mt-8">
      <div
        class="max-w-max mx-auto rounded-xl bg-gray-100 ring-4 ring-gray-500/10 border p-1 space-y-2"
      >
        <BuyButton
          label="Buy Now for $149"
          class="flex items-center justify-center w-full"
          size="md"
          rounded="lg"
        />
      </div>
    </div>
<!--    <HomeTestimonial />-->
  </section>
</template>

<script setup>
defineProps({
  count: {
    type: Number,
    default: 0,
  },
});
</script>

<style>
.animate-text-slide-5 {
  animation: text-slide 12.5s cubic-bezier(0.83, 0, 0.17, 1) infinite;
}

@keyframes text-slide {
  0%,
  16% {
    transform: translateY(0%);
  }
  20%,
  36% {
    transform: translateY(-16.66%);
  }
  40%,
  56% {
    transform: translateY(-33.33%);
  }
  60%,
  76% {
    transform: translateY(-50%);
  }
  80%,
  96% {
    transform: translateY(-66.66%);
  }
  100% {
    transform: translateY(-83.33%);
  }
}
</style>
