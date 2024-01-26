<script setup>
import Card from './Card.vue'

defineProps({
  items: Array,
  isFavorites: Boolean
})

const emit = defineEmits(['addToFavorite', 'addToCart'])
</script>

<template>
  <div class="grid grid-cols-2 md:grid-cols-3 lg:grid-cols-4 gap-5" v-auto-animate>
    <!-- Когда используем v-for, Обязательно нужно указать уникальный ключ -->
    <Card
      v-for="item in items"
      :key="item.id"
      :id="item.id"
      :title="item.title"
      :imageUrl="item.imageUrl"
      :price="item.price"
      :isFavorite="item.isFavorite"
      :isAdded="item.isAdded"
      :isSale="item.isSale"
      :onCLickFavorite="isFavorites ? null : () => emit('addToFavorite', item)"
      :onCLickAdd="isFavorites ? null : () => emit('addToCart', item)"
    />
  </div>
</template>
