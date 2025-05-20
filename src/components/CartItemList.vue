<script setup>
import { inject } from 'vue'
import CartItem from './CartItem.vue'

const { cart, removeFromCart } = inject('cart')
</script>

<template>
  <div class="flex flex-col gap-4 mb-8 pb-6 overflow-y-auto cart-items-list" v-auto-animate>
    <CartItem 
      v-for="item in cart" 
      :key="item.id" 
      :title="item.title"
      :price="item.price"
      :image-url="item.imageUrl"
      @on-click-remove="() => removeFromCart(item)"
    />
  </div>
</template>

<style scoped>
.cart-items-list {
  overscroll-behavior-y: none;
}

.cart-items-list::-webkit-scrollbar {
  width: 0;
  height: 0;
  background-color: transparent;
}

.cart-items-list::-webkit-scrollbar-button {
  width: 0;
  height: 0;
  display: none;
}

.cart-items-list::-webkit-scrollbar-corner {
  background-color: transparent;
}

.cart-items-list::-webkit-scrollbar-track {
  background-color: transparent;
}

.cart-items-list::-webkit-scrollbar-thumb {
  background-color: transparent;
}

.cart-item-list {
  @supports not selector(::-webkit-scrollbar) {
    scrollbar-width: none;
  }
}

</style>