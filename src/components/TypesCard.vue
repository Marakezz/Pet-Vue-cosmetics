<script setup>
defineProps({
  type: String,
  img: String,
  onTypeClicked: Function,
  isActive: Boolean
})
</script>

<template>
  <div
    class="flex-1 py-6 text-center bg-gray-100 border-x border-b border-solid border-slate-200 cursor-pointer hover:shadow"
    :class="isActive ? 'bg-yellow-400' : 'bg-gray-100'"
    @click="onTypeClicked"
  >
    <h2 class="font-bold">{{ type }}</h2>
    <img class="inline-block h-4" :src="img" alt="image" />
  </div>
</template>
