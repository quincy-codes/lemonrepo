<!-- StripeCheckoutModal.vue -->
<template>
  <UModal v-model="isOpen">
    <div class="w-full h-[600px]">
      <iframe 
        :src="stripeUrl"
        class="w-full h-full border-0"
        frameborder="0"
        allowpaymentrequest
      ></iframe>
    </div>
  </UModal>
</template>

<script setup lang="ts">
import { watch } from 'vue'

const props = defineProps<{
  isOpen: boolean
}>()

const emit = defineEmits(['update:isOpen'])

const stripeUrl = 'https://buy.stripe.com/7sI9CH0x17bb6aYeUV'

watch(() => props.isOpen, (newVal) => {
  emit('update:isOpen', newVal)
})
</script> 