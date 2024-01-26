<script setup>
defineProps({
  id: Number,
  imageUrl: String,
  title: String,
  price: Number,
  isFavorite: Boolean,
  isAdded: Boolean,
  onCLickAdd: Function,
  onCLickFavorite: Function,
  isSale: Boolean
})
</script>
<template>
  <div
    class="relative bg-white border border-slate-100 rounded-3xl p-8 cursor-pointer transition hover:-translate-y-2 hover:shadow-xl"
  >
    <img
      v-if="onCLickFavorite"
      :src="!isFavorite ? '/like-1.svg' : '/like-2.svg'"
      alt="Like 1"
      class="absolute top-8 left-8"
      @click="onCLickFavorite"
    />
    <img v-if="isSale" src="/sale.png" alt="Sale" class="absolute bottom-1 left-1 w-8" />

    <img :src="imageUrl" alt="Product" class="h-1/2 lg:h-auto" />
    <p class="mt-2 h-1/4 lg:h-auto">{{ title }}</p>

    <div class="flex justify-between mt-4">
      <div class="flex flex-col">
        <span class="text-slate-400">Цена:</span>
        <b>{{ price }} Руб.</b>
      </div>
      <img
        v-if="onCLickAdd"
        @click="onCLickAdd"
        :src="!isAdded ? '/plus.svg' : '/checked.svg'"
        alt="Plus"
      />
    </div>
  </div>
</template>
