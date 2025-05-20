<script setup>
import { ref, onMounted } from 'vue'
import axios from 'axios'
import CardList from '../components/CardList.vue'
import Loader from '../components/Loader.vue'

const favorites = ref([])
const isLoading = ref(false)

onMounted(async () => {
  try {
    isLoading.value = true

    const { data } = await axios.get('https://512ea0ea576c7076.mokky.dev/favorites')
    favorites.value = data.map(obj => obj.item)

    isLoading.value = false
  } catch (error) {
    console.log(error)
  }
})
</script>

<template>
  <h2 class="text-3xl font-bold">Мои закладки</h2>

  <Loader v-if="isLoading" />
  <div class="mt-10">
    <CardList :items="favorites" is-favorites/>
  </div>
</template>