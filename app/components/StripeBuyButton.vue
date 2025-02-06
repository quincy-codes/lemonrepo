<template>
  <div>
    <UButton
      color="black"
      :ui="{ rounded: 'rounded-full' }"
      class="shiny-button px-6 py-2"
      @click="isModalOpen = true"
    >
      Buy Now for $149
    </UButton>

    <UModal v-model="isModalOpen">
      <div class="w-full h-[600px]">
        <iframe 
          src="https://buy.stripe.com/7sI9CH0x17bb6aYeUV"
          class="w-full h-full border-0"
          frameborder="0"
          allowpaymentrequest
        ></iframe>
      </div>
    </UModal>
  </div>
</template>

<script setup lang="ts">
import { ref } from 'vue'

const isModalOpen = ref(false)
</script> 