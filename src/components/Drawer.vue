<script setup>
import axios from 'axios'
import DrawerHead from './DrawerHead.vue'
import CartItemList from './CartItemList.vue'
import Infoblock from './InfoBlock.vue'
import { ref, watch, computed, inject } from 'vue'

const isCreating = ref(false)
const orderId = ref(null)

const props = defineProps({
  totalPrice: Number,
  vatPrice: Number
  // isCreatingOrder: Boolean
})
// const buttonDisabled = computed(() =>
//   props.isCreatingOrder ? true : props.totalPrice ? false : true //Это работало, но у него косяк и думал что не работало)
// )

const { cart, closeDrawer } = inject('cart')

const createOrder = async () => {
  try {
    isCreating.value = true
    const { data } = await axios.post('https://238aa2c8cef6eb4c.mokky.dev/orders', {
      items: cart.value,
      totalPrice: props.totalPrice.value
    })

    cart.value = [] //очищаем корзину после заказа

    orderId.value = data.id
  } catch (err) {
    console.log(err)
  } finally {
    isCreating.value = false
  }
}

const cartIsEmpty = computed(() => cart.value.length === 0)
const buttonDisabled = computed(() => isCreating.value || cartIsEmpty.value)
</script>
<template>
  <div>
    <div class="fixed top-0 left-0 h-full w-full bg-black z-10 opacity-70"></div>
    <div class="bg-white w-96 h-full fixed right-0 top-0 z-20 p-8">
      <DrawerHead />

      <div v-if="!totalPrice || orderId" class="flex h-full items-center">
        <Infoblock
          v-if="!totalPrice && !orderId"
          title="Корзина пустая"
          description="Добавьте хотя бы один товар, чтобы сделать заказ"
          image-url="/package-icon.png"
        />
        <Infoblock
          v-if="orderId"
          title="Заказ оформлен!"
          :description="`Ваш заказ #${orderId} скоро будет передан курьерской доставке, но это не точно`"
          image-url="/order-success-icon.png"
        />
      </div>

      <div v-else>
        <CartItemList />

        <div class="flex flex-col gap-4 mt-7">
          <div class="flex gap-2">
            <span>Итого:</span>
            <div class="flex-1 border-b border-dashed"></div>
            <b>{{ totalPrice }} ₽</b>
          </div>

          <div class="flex gap-2">
            <span>Налог 5%</span>
            <div class="flex-1 border-b border-dashed"></div>
            <b>{{ vatPrice }} ₽</b>
          </div>
          <button
            :disabled="buttonDisabled"
            @click="createOrder"
            class="mt-4 transition bg-lime-500 w-full rounded-xl py-3 text-white disabled:bg-slate-300 hover:bg-lime-600 active:bg-lime-700 cursor-pointer"
          >
            Оформить заказ
          </button>
        </div>
      </div>
    </div>
  </div>
</template>
