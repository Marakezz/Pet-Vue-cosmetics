<script setup>
import axios from 'axios'
import debounce from 'lodash.debounce'
import { inject, reactive, ref, watch, onMounted } from 'vue'
import CardList from '../components/CardList.vue'
import Types from '@/components/Types.vue'
import Sale from '@/components/Sale.vue'

const { cart, addToCart, removeFromCart } = inject('cart')

const items = ref([])
const saleCheckBox = ref(null)

const filters = reactive({
  sortBy: 'title',
  searchQuery: '',
  typeQuery: '',
  saleQuery: ''
})

const onSaleChecked = (event) => {
  filters.saleQuery = event.target.checked
}

const onSaleClicked = () => {
  filters.saleQuery = true
  // $refs.checked = true
  saleCheckBox.value.checked = true
}

const onClickAddPlus = (item) => {
  if (!item.isAdded) {
    addToCart(item)
  } else {
    removeFromCart(item)
  }
}

const onChangeSelect = (event) => {
  filters.sortBy = event.target.value
}

const onChangeSearchInput = debounce((event) => {
  filters.searchQuery = event.target.value
}, 500)

const onChangeType = (elem) => {
  filters.typeQuery = elem
}

const addToFavorite = async (item) => {
  try {
    if (!item.isFavorite) {
      const obj = {
        item_id: item.id
      }
      item.isFavorite = true
      const { data } = await axios.post('https://238aa2c8cef6eb4c.mokky.dev/favorites', obj)
      item.favoriteId = data.id
    } else {
      item.isFavorite = false
      await axios.delete(`https://238aa2c8cef6eb4c.mokky.dev/favorites/${item.favoriteId}`)
      item.favoriteId = null
    }
  } catch (err) {
    console.log(err)
  }
}

const fetchFavorites = async () => {
  try {
    const { data: favorites } = await axios.get('https://238aa2c8cef6eb4c.mokky.dev/favorites') //переименовали data B favorites
    items.value = items.value.map((item) => {
      const favorite = favorites.find((favorite) => favorite.item_id === item.id)
      if (!favorite) {
        return item
      }
      return {
        ...item,
        isFavorite: true,
        favoriteId: favorite.id
      }
    })
  } catch (err) {
    console.log(err)
  }
}

const fetchItems = async () => {
  try {
    const params = {
      sortBy: filters.sortBy
    }
    if (filters.searchQuery) {
      params.title = `*${filters.searchQuery}*`
    }
    if (filters.typeQuery) {
      params.type = `${filters.typeQuery}`
    }
    if (filters.saleQuery) {
      params.isSale = 1
    }
    const { data } = await axios.get('https://238aa2c8cef6eb4c.mokky.dev/items', {
      params
    })

    items.value = data.map((obj) => ({
      ...obj,
      isFavorite: false,
      favoriteId: null,
      isAdded: false
    }))
  } catch (err) {
    console.log(err)
  }
}

onMounted(async () => {
  const localCart = localStorage.getItem('cart')
  cart.value = localCart ? JSON.parse(localCart) : []

  await fetchItems()
  await fetchFavorites()

  items.value = items.value.map((item) => ({
    ...item,
    isAdded: cart.value.some((cartItem) => cartItem.id === item.id)
  }))
})

watch(filters, fetchItems)
watch(cart, () => {
  items.value = items.value.map((item) => ({
    ...item,
    isAdded: false //Убираем все товары из добавленных после оформления заказа
  }))
})
</script>

<template>
  <div>
    <Types @on-change-type="onChangeType" />
    <Sale @on-sale-clicked="onSaleClicked" />
    <div class="p-10 flex justify-between items-center">
      <h2 class="text-3xl font-bold mb-8">Вся продукция</h2>

      <div class="flex gap-4">
        <label class="py-2 px-3">
          <input ref="saleCheckBox" type="checkbox" @input="onSaleChecked" />
          Товары со скидкой
        </label>

        <select @change="onChangeSelect" class="py-2 px-3 border rounded-md outline-none">
          <option value="name">По названию</option>
          <option value="price">По цене(дешевые)</option>
          <option value="-price">По цене(дорогие)</option>
        </select>
        <div class="relative">
          <img class="absolute left-3 top-3" src="/search.svg" alt="search" />
          <input
            @input="onChangeSearchInput"
            class="border rounded-md py-2 pl-11 pr-4 outline-none focus:border-gray-400"
            placeholder="Поиск..."
          />
        </div>
      </div>
    </div>
    <div class="mt-0">
      <CardList :items="items" @add-to-favorite="addToFavorite" @add-to-cart="onClickAddPlus" />
    </div>
  </div>
</template>
