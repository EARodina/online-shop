<script setup>
import { ref, watch } from 'vue'

const emit = defineEmits(['update:modelValue'])

const props = defineProps({
  modelValue: {
    type: Number,
    default: 1
  },
  totalPages: {
    type: Number,
    required: true
  },
  itemsPerPage: {
    type: Number,
    default: 10
  },
})

const currentPage = ref(props.modelValue)


function prevPage() {
  if (currentPage.value > 1) {
    currentPage.value--
  }
}

function nextPage() {
  if (currentPage.value < props.totalPages) currentPage.value++
}

function goToPage(page) {
  if (page >= 1 && page <= props.totalPages) {
    currentPage.value = page
  }
}

watch(currentPage, (newPage) => {
  emit('update:modelValue', newPage)
})

watch(
  () => props.modelValue,
  (newValue) => {
    currentPage.value = newValue
  }
)
</script>

<template>
  <ul class="pagination">
    <li class="pagination__item">
      <button 
        class="pagination__link" 
        :disabled="currentPage === 1"
        @click="prevPage"
      >&lt;</button>
    </li>
    <li 
      class="pagination__item"
      v-for="page in totalPages" 
      :key="page" 
      @click="goToPage(page)"
    >
      <a class="pagination__link" :class="{active: page === currentPage}">{{ page }}</a>
    </li>
    <li class="pagination__item">
      <button 
        class="pagination__link" 
        :disabled="currentPage === totalPages"
        @click="nextPage"
      >&gt;</button>
    </li>
  </ul>
</template>

<style scoped>
.pagination {
  display: flex;
  align-items: center;
  gap: 8px;
}

.pagination__link {
  display: flex;
  justify-content: center;
  align-items: center;
  min-width: 32px;
  min-height: 32px;
  border: 1px solid transparent;
  border-radius: 0.5rem;
  text-decoration: none;
  transition: border-color .3s ease, background-color .3s ease;
  cursor: pointer;
}

.pagination__link:hover:not(:disabled, .active) {
  background-color: rgb(228 228 231 / 1);
}

.pagination__link.active {
  border-color: #dadada;
}

button:disabled {
  opacity: 0.5;
  cursor: not-allowed;
}
</style>