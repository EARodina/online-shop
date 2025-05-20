<script setup>
import Card from './Card.vue'

const emit = defineEmits(['addToFavorite', 'addToCart'])

defineProps({
  items: Array,
  isFavorites: Boolean
})
</script>

<template>
  <div class="grid sm:grid-cols-2 lg:grid-cols-3 xl:grid-cols-4 gap-5" v-auto-animate>
    <Card
      v-for="item in items"
      :key="item.id"
      :id="item.id"
      :title="item.title"
      :image-url="item.imageUrl" 
      :price="item.price"
      :is-added="item.isAdded"
      :is-favorite="item.isFavorite"
      :onClickFavorite="isFavorites ? null : () => emit('addToFavorite', item)"
      :onClickAdd="isFavorites ? null : () => emit('addToCart', item)"
    />
  </div>
</template>