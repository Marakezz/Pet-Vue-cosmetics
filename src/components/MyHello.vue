<!-- <script setup>
import { ref } from 'vue'
const count = ref(0)

function increment() {
  count.value++
}

// function updateCount(event) {
//   count.value = Number(event.target.value)
// }
//  <input @input="updateCount" />
</script> -->

<!-- <template>
  <h1>{{ count }}</h1>
  <button @click="increment">+1</button>
   <input @keyup.enter="increment" /> 

  <input v-model="count" />
</template> -->

<!-- <style scoped>
.text {
  background: red;
}
</style> -->

<script setup>
defineProps({
  title: String,
  price: Number
})
</script>

<template>
  <div class="product">
    <h2>Товар: {{ title }}</h2>
    <h3>Цена: {{ price }}</h3>
  </div>
  <slot></slot>
</template>

<style scoped>
.product {
  border: 1px solid blue;
  padding: 10px;
  margin-bottom: 10px;
}
</style>

<!-- Из основного файла
  <div class="mb-18">
  <MyHello title="Кроссовки Puma" :price="9500" :isAdded="true" />
</div>
<MyHello>
  <button>Тест, добавляем через Slot</button>
</MyHello>
<MyHello />
<MyHello />
<MyHello /> -->
