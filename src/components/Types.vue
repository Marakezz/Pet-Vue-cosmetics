<script setup>
import { ref } from 'vue'
import TypesCard from './TypesCard.vue'

const types = ref([
  { id: 1, type: 'Уходовая косметика', img: '/skincare.png', isActive: false },
  { id: 2, type: 'Декоративная косметика', img: '/decorative.png', isActive: false },
  { id: 3, type: 'Аксессуары', img: '/accesories.svg', isActive: false },
  { id: 4, type: 'Парфюмерия', img: '/perfume.png', isActive: false }
])

const emit = defineEmits(['onChangeType'])
const onTypeClicked = (item) => {
  if (!item.isActive) {
    types.value.forEach((elem) => {
      elem.isActive = false
    })
    item.isActive = true
    emit('onChangeType', item.type)
  } else {
    item.isActive = false
    emit('onChangeType', '')
  }
}
</script>

<template>
  <div class="flex h-20 justify-between w-full">
    <TypesCard
      v-for="item in types"
      :key="item.id"
      :type="item.type"
      :img="item.img"
      :is-active="item.isActive"
      :on-type-clicked="() => onTypeClicked(item)"
    />
  </div>
</template>
