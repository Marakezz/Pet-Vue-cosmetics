<script setup>
const emit = defineEmits(['onSaleClicked'])
</script>

<template>
  <div
    @click="emit('onSaleClicked')"
    class="text-center font-extrabold cursor-pointer h-20 w-full py-5 bg-gradient-to-r from-pink-500 to-red-500"
  >
    <img src="/sales.png" alt="sales" class="w-20 absolute l-1" />
    <h4 class="text-xl">Скидки до 90% !!</h4>
  </div>
</template>
