<script setup>
defineProps({
  totalPrice: Number
})
const emit = defineEmits(['openDrawer'])
</script>

<template>
  <header class="flex justify-between border-b border-slate-200 px-10 py-8">
    <router-link to="/">
      <div class="flex items-center gap-4">
        <img src="/logo.png" alt="Logo" class="w-10" />
        <!-- Вит сам найдет картинку в паблике, и не нужно указывать его в пути -->
        <div>
          <h2 class="text-xl font-bold uppercase">It's Skin</h2>
          <p class="text-slate-400">Магазин корейской косметики</p>
        </div>
      </div>
    </router-link>

    <ul class="flex items-center gap-10">
      <img src="/cart.svg" alt="Cart" />
      <li
        @click="() => emit('openDrawer')"
        class="flex items-center cursor-pointer gap-3 text-gray-500 hover:text-black"
      >
        <b>{{ totalPrice }} руб.</b>
      </li>
      <router-link to="/favorites">
        <li class="flex items-center cursor-pointer gap-3 text-gray-500 hover:text-black">
          <img src="/heart.svg" alt="Heart" />
          <span>Закладки</span>
        </li>
      </router-link>
      <!-- <li class="flex items-center cursor-pointer gap-3 text-gray-500 hover:text-black">
        <img src="/profile.svg" alt="Profile" />
        <span>Профиль</span>
      </li> -->
    </ul>
  </header>
</template>
