<script setup>
const emit = defineEmits(['onClickRemove'])

defineProps({
  id: Number,
  title: String,
  imageUrl: String,
  price: Number
})
</script>

<template>
  <div class="flex items-center border border-slate-100 p-4 rounded-xl gap-4">
    <img class="w-16 h-16" :src="imageUrl" :alt="title">

    <div class="flex flex-col flex-1">
      <p>{{ title }}</p>

      <div class="flex justify-between mt-2">
        <b>{{ price }}</b>
        <img class="opacity-40 hover:opacity-100 cursor-pointer transition" src="/close.svg" @click="emit('onClickRemove')">
      </div>
    </div>
  </div>
</template>