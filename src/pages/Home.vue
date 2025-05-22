<script setup>
import { ref, computed, reactive,  watch, onMounted, inject } from 'vue'
import axios from 'axios'
import debounce from 'lodash.debounce'
import CardList from '../components/CardList.vue'
import Loader from '../components/Loader.vue'
import Pagination from '@/components/Pagination.vue'

const { cart, addToCart, removeFromCart } = inject('cart')

const items = ref([])
const isLoading = ref(false)
const currentPage = ref(1)
const totalPages = ref(0)
const totalItems = ref(0)
const itemsPerPage = 8

const filters = reactive({
  // sortBy: 'title',
  sortBy: '',
  searchQuery: ''
})

const visibleRange = computed(() => {
  return Math.min(currentPage.value * itemsPerPage, totalItems.value)
})

const onClickAddPlus = (item) => {
  if (!item.isAdded) {
    addToCart(item)
  } else {
    removeFromCart(item)
  }
}

const onChangeSelect = (e) => {
  filters.sortBy = e.target.value
}

const onChangeSearchInput = debounce((e) => {
  filters.searchQuery = e.target.value
}, 500)

const addToFavorite = async (item) => {
  try {
    if (!item.isFavorite) {
      const obj = {
        parentId: item.id,
        item
      }

      item.isFavorite = true

      const { data } = await axios.post('https://512ea0ea576c7076.mokky.dev/favorites', obj)
  
      item.favoriteId = data.id
    } else {
      item.isFavorite = false
      
      await axios.delete(`https://512ea0ea576c7076.mokky.dev/favorites/${item.favoriteId}`)

      item.favoriteId = null
    }
  } catch(e) {
    console.log(e)
  }
}

const fetchFavorites = async () => {
  try {
    const { data: favorites } = await axios.get('https://512ea0ea576c7076.mokky.dev/favorites')

    items.value = items.value.map(item => {
      const favorite = favorites.find(favorite => favorite.parentId === item.id)

      if (!favorite) {
        return item
      }

      return {
        ...item,
        isFavorite: true,
        favoriteId: favorite.id
      }
    })
  } catch (e) {
    console.log(e)
  }
}

const fetchItems = async () => {
  try {
    isLoading.value = true

    const params = {
      sortBy: filters.sortBy,
      page: currentPage.value,
      limit: itemsPerPage
    }

    if (filters.searchQuery) {
      params.title = `*${filters.searchQuery}*`
    }

    const { data } = await axios.get('https://512ea0ea576c7076.mokky.dev/items',
      {
        params
      }
    )

    items.value = data.items.map(obj => ({
      ...obj,
      isFavorite: false,
      favoriteId: null,
      isAdded: false
    }))

    totalPages.value = data.meta.total_pages
    totalItems.value = data.meta.total_items

    isLoading.value = false
  } catch(e) {
    console.log(e)
  }
}

watch(filters, fetchItems)
watch(currentPage, fetchItems)

watch(cart, () => {
  items.value = items.value.map((item) => ({
    ...item,
    isAdded: false
  }))
})

onMounted(async () => {
  const localCart = localStorage.getItem('cart')
  cart.value = localCart ? JSON.parse(localCart) : []
  
  await fetchItems();
  await fetchFavorites();

  items.value = items.value.map((item) => ({
    ...item,
    isAdded: cart.value.some((cartItem) => cartItem.id === item.id)
  }))
})
</script>

<template>
  <div class="flex flex-wrap justify-between items-center gap-6">
    <h2 class="text-2xl md:text-3xl font-bold">Все смартфоны</h2>

    <div class="flex flex-wrap gap-2 md:gap-4">
      <div class="custom-select">
        <select @change="onChangeSelect" class="py-1 md:py-2 px-3 border rounded-md outline-none">
          <option value="name">По названию</option>
          <option value="price">По цене (дорогие)</option>
          <option value="-price">По цене (дешевые)</option>
        </select>
      </div>

      <div class="relative">
        <img src="/search.svg" class="absolute left-3 top-2 md:top-3">
        <input type="text" placeholder="Поиск..." class="border rounded-md py-1 md:py-2 pl-11 pr-4 outline-none focus:border-gray-400" @input="onChangeSearchInput">
      </div>
    </div>
  </div>

  <Loader v-if="isLoading" />

  <div class="mt-10">
    <CardList :items="items" @add-to-favorite="addToFavorite"  @add-to-cart="onClickAddPlus"/>
    <hr class="my-10 border border-slate-100">
    <div v-if="totalPages" class="flex flex-wrap justify-between items-center gap-4">
      <Pagination 
        v-model="currentPage"
        :total-pages="totalPages"
      />
      <div class="text-sm text-gray-500">
        Вы посмотрели {{ visibleRange }} из {{ totalItems }} товаров
      </div>
    </div>
  </div>
</template>

<style scoped>
.custom-select {
  position: relative;
}

.custom-select select {
  width: 200px;
  appearance: none;
  outline: none;
  cursor: pointer;
  transition: all 0.3s ease;
}

.custom-select::after {
  content: "▼";
  position: absolute;
  top: 50%;
  right: 15px;
  transform: translateY(-50%);
  color: #aeaeae;
  pointer-events: none;
  font-size: 12px;
}
</style>