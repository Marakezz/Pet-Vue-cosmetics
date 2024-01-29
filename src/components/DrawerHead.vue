<script setup>
import { inject } from 'vue'

const { closeDrawer } = inject('cart')
</script>
<template>
  <div class="flex items-center gap-5 mb-8">
    <img
      @click="closeDrawer"
      class="w-10 opacity-30 cursor-pointer rotate-180 hover:opacity-100 transition hover:-translate-x-1"
      src="/cross.svg"
      alt="X"
    />
    <h2 class="text-2xl font-bold">Корзина</h2>
  </div>
</template>
