<script setup>
const props = defineProps({
  id: {
    type: Number
  },
  imageUrl: {
    type: String
  },
  title: {
    type: String
  },
  price: {
    type: Number
  },
  isFavorite: {
    type: Boolean
  },
  isAdded: {
    type: Boolean
  },
  onClickFavorite: {
    type: Function
  },
  onClickAdd: {
    type: Function
  },
})
</script>

<template>
  <div class="relative flex flex-col gap-3 bg-white border border-slate-100 rounded-2xl p-6 cursor-pointer will-change-transform transition hover:-translate-y-2 hover:shadow-xl ">
    <div>
      <img 
        v-if="onClickFavorite"
        :src="!isFavorite ? '/like-1.svg' : 'like-2.svg'" 
        alt="Like 2" 
        class="absolute top-3 left-3"
        @click="onClickFavorite"
      >
      <img :src="imageUrl" alt="Sneaker" class="h-44 mx-auto">
  
      <p class="mt-2">{{ title }}</p>
    </div>

    <div class="flex justify-between mt-auto">
      <div class="flex flex-col">
        <span class="text-slate-400">Цена:</span>
        <b>{{ price }} ₽</b>
      </div>
      
      <img 
        v-if="onClickAdd"
        :src="!isAdded ? '/plus.svg' : '/checked.svg'" 
        alt="plus" 
        @click="onClickAdd"
      >
    </div>
  </div>
</template>