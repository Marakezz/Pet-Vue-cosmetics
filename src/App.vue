<script setup>
import { ref, watch, provide, computed } from 'vue'

import Header from './components/Header.vue'
import Drawer from './components/Drawer.vue'

/* Корзина -> */
const cart = ref([])
const drawerOpen = ref(false)

const totalPrice = computed(() => cart.value.reduce((acc, item) => acc + item.price, 0))
const vatPrice = computed(() => Math.round((totalPrice.value * 5) / 100))

const closeDrawer = () => {
  drawerOpen.value = false
}
const openDrawer = () => {
  drawerOpen.value = true
}

const addToCart = (item) => {
  cart.value.push(item)
  item.isAdded = true
}
const removeFromCart = (item) => {
  cart.value.splice(cart.value.indexOf(item), 1) //удаляем 1 объект после индекса
  item.isAdded = false
}

watch(
  cart,
  () => {
    localStorage.setItem('cart', JSON.stringify(cart.value))
  },
  { deep: true }
)

provide('cart', {
  cart,
  closeDrawer,
  openDrawer,
  removeFromCart,
  addToCart
})

/* <- Корзина */
</script>

<template>
  <div>
    <Drawer v-if="drawerOpen" :total-price="totalPrice" :vat-price="vatPrice" />
    <div>
      <div class="bg-white w-4/5 m-auto rounded-xl shadow-xl mt-14">
        <Header :total-price="totalPrice" @open-drawer="openDrawer" />

        <div>
          <router-view></router-view>
        </div>
      </div>
    </div>
  </div>
</template>
<!-- Верстка вне template не отобразится -->

<style scoped></style>
