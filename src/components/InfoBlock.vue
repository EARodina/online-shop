<script setup>
defineProps({
  title: String,
  description: String,
  imageUrl: String
})
</script>

<template>
  <div class="flex flex-col items-center text-center w-72 mx-auto">
    <img height="100" width="100" :src="imageUrl" alt="Info image">
    <h2 class="mt-2 text-2xl font-mwdium">{{ title }}</h2>
    <p class="text-gray-400">{{ description }}</p>
  </div>
</template>