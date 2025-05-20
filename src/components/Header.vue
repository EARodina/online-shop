<script setup>
const emit = defineEmits(['openDrawer'])

defineProps({
  totalPrice: Number,
  cartCount: Number
})
</script>

<template>
  <header class="flex flex-wrap items-center gap-4 justify-between border-b border-slate-100 p-4 md:px-10 md:py-8">
    <router-link to="/">
      <div class="flex items-center gap-2">
        <img src="/logo.svg" alt="Logo" class="w-6 sm:w-8">
        <div>
          <h1 class="text-xl font-bold uppercase">PhoneStore</h1>
        </div>
      </div>
    </router-link>

    <ul class="flex items-center gap-4 md:gap-6">
      <li class="flex flex-col items-center gap-1 text-gray-500 hover:text-black transition cursor-pointer" @click="emit('openDrawer')">
        <span class="relative w-5 h-5">
          <span v-if="totalPrice" class="absolute -top-1 -right-1.5 flex items-center justify-center h-[14px] min-w-3 py-1 px-1 text-white rounded-[50px] bg-blue-500 shadow-[0_0_0_2px_rgb(255,255,255)] text-[10px]">{{ cartCount }}</span>
          <svg viewBox="0 0 20 20" width="20" height="20" fill="none" xmlns="http://www.w3.org/2000/svg">
            <path d="M7.54548 18.1818C7.99735 18.1818 8.36366 17.8155 8.36366 17.3636C8.36366 16.9118 7.99735 16.5455 7.54548 16.5455C7.09361 16.5455 6.72729 16.9118 6.72729 17.3636C6.72729 17.8155 7.09361 18.1818 7.54548 18.1818Z" stroke="#9B9B9B" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"/>
            <path d="M16.5455 18.1818C16.9973 18.1818 17.3637 17.8155 17.3637 17.3636C17.3637 16.9118 16.9973 16.5455 16.5455 16.5455C16.0936 16.5455 15.7273 16.9118 15.7273 17.3636C15.7273 17.8155 16.0936 18.1818 16.5455 18.1818Z" stroke="#9B9B9B" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"/>
            <path d="M1 1H4.27273L6.46545 11.9555C6.54027 12.3321 6.7452 12.6705 7.04436 12.9113C7.34351 13.1522 7.71784 13.2801 8.10182 13.2727H16.0545C16.4385 13.2801 16.8129 13.1522 17.112 12.9113C17.4112 12.6705 17.6161 12.3321 17.6909 11.9555L19 5.09091H5.09091" stroke="#9B9B9B" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"/>
          </svg>
        </span>
        <span class="text-sm hidden sm:block">{{ totalPrice ? `${totalPrice} ₽`: 'В корзину' }} </span>
      </li>
      <router-link to="/favorites">
        <li class="flex flex-col items-center gap-1 text-gray-500 hover:text-black transition cursor-pointer">
          <span class="relative w-5 h-5">
            <svg xmlns="http://www.w3.org/2000/svg" width="20" height="20" viewBox="0 0 24 24"><path d="M12 4.36693C10.675 3.27975 9.24521 2.71991 7.72945 2.71991C4.56517 2.71991 2 5.28506 2 8.44934C2 12.7785 5.19725 16.9814 11.4484 21.1143L11.9998 21.4789L12.5513 21.1143C18.8026 16.9814 22 12.7785 22 8.44934C22 5.28505 19.4349 2.71991 16.2706 2.71991C14.7548 2.71991 13.325 3.27975 12 4.36693ZM4 8.44934C4 6.38963 5.66973 4.71991 7.72945 4.71991C8.95344 4.71991 10.132 5.27022 11.296 6.42424L12 7.12225L12.7041 6.42423C13.8681 5.27022 15.0466 4.71991 16.2706 4.71991C18.3303 4.71991 20 6.38962 20 8.44934C20 11.8468 17.3766 15.4058 11.9999 19.0755C6.62334 15.4058 4 11.8468 4 8.44934Z" fill="#9B9B9B"></path></svg>
          </span>
          <span class="text-sm hidden sm:block">Закладки</span>
        </li>
      </router-link>
    </ul>
  </header>
</template>

<style scoped>
.router-link-exact-active li {
  color: #0D6EFD !important;
}
.router-link-exact-active svg path {
  fill:#0D6EFD !important;
}
</style>