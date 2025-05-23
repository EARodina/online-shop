<script setup>
import { ref, computed, watch, provide } from 'vue'
import Header from './components/Header.vue'
import Drawer from './components/Drawer.vue'

const cart = ref([])
const drawerOpen = ref(false)

const cartCount = computed(() => cart.value.length)

const totalPrice = computed(() => {
  return cart.value.reduce((acc, item) => acc + item.price, 0)
})

const vatPrice = computed(() => {
  return Math.round((totalPrice.value * 5) / 100)
})

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
  cart.value.splice(cart.value.indexOf(item), 1)
  item.isAdded = false
}

watch(cart, () => {
  localStorage.setItem('cart', JSON.stringify(cart.value))
}, {
  deep: true}
)

provide('cart', {
  cart,
  closeDrawer,
  openDrawer,
  addToCart,
  removeFromCart
})
</script>

<template>
  <transition name="slide">
    <Drawer 
      v-if="drawerOpen" 
      :total-price="totalPrice" 
      :vat-price="vatPrice"
    />
  </transition>

  <div class="w-11/12 md:w-4/5 max-w-7xl m-auto bg-white rounded-xl shadow-xl my-3 md:my-14">
    <Header 
      :total-price="totalPrice"
      :cart-count="cartCount"
      @open-drawer="openDrawer" 
    />
    
    <div class="p-6 md:p-10">
      <router-view></router-view>
    </div>
  </div>
</template>

<style scoped>
.slide-enter-from,
.slide-leave-to {
  transform: translateX(100%);
}
.slide-enter-active,
.slide-leave-active {
  transition: all .3s;
}
.slide-enter-to {
  transform: translateX(0px);
}
</style>
