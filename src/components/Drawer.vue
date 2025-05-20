<script setup>
import { ref, computed, inject } from 'vue'
import axios from 'axios'
import DrawerHead from './DrawerHead.vue'
import CartItemList from './CartItemList.vue'
import InfoBlock from './InfoBlock.vue'

const props = defineProps({
  totalPrice: Number,
  vatPrice: Number
})

const { cart } = inject('cart')

const isCreating = ref(false)
const orderId = ref(null)

const createOrder = async () => {
  try {
    isCreating.value = true
    const { data } = await axios.post('https://512ea0ea576c7076.mokky.dev/orders', {
      items: cart.value,
      totalPrice: props.totalPrice.value
    })

    cart.value = []
    orderId.value = data.id

    return data
  } catch (error) {
    console.log(error)
    isCreating.value = false
  }
}

const cartIsEmpty = computed(() => cart.value === 0)
const buttonDisabled = computed(() => isCreating.value || cartIsEmpty.value)
</script>

<template>
  <!-- <div class="fixed top-0 left-0 h-full w-full bg-black z-10 opacity-70"></div> -->

  <div class="flex flex-col bg-white w-full sm:w-96 h-full sm:shadow-xl fixed right-0 top-0 z-20 p-4 sm:p-8">
    <DrawerHead />

    <div v-if="!totalPrice || orderId" class="flex items-center h-full">
      <InfoBlock 
        v-if="!totalPrice && !orderId"
        title="Корзина пустая" 
        description="Добавьте хотя бы один товар, чтобы сделать заказ" 
        image-url="/package-icon.png"
      />

      <InfoBlock 
        v-if ="orderId"
        title="Заказ оформлен" 
        description="Ваш заказа #18 скоро будет передан курьерской доставке" 
        image-url="/order-success-icon.png"
      />
    </div>

    <div v-else class="flex flex-col cart-items">
      <CartItemList />
  
      <div class="flex flex-col gap-4 mt-auto cart-items__summary">
        <div class="flex gap-2">
          <span>Итого:</span>
          <div class="flex-1 border-b border-dashed"></div>
          <b>{{ totalPrice }} Р</b>
        </div>
        <div class="flex gap-2">
          <span>Налог 5%:</span>
          <div class="flex-1 border-b border-dashed"></div>
          <b>{{ vatPrice }} Р</b>
        </div>
  
        <button 
          class="mt-4 bg-blue-500 w-full rounded-xl py-3 text-white transition hover:bg-blue-600 active:bg-blue-700 disabled:bg-slate-400 disabled:cursor-not-allowed cursor-pointer"
          :disabled="buttonDisabled"
          @click="createOrder"  
        >Оформить заказ</button>
      </div>
    </div>
  </div>
</template>

<style scoped>
.cart-items {
  height: calc(100% - 64px);
}

.cart-items__summary {
  position: relative;
}

.cart-items__summary::before {
  content: '';
  width: 100%;
  height: 20px;
  background: linear-gradient(to bottom, rgba(255,255,255,0), #FFF);
  position: absolute;
  top: -48px;
  left: 0;
}
</style>