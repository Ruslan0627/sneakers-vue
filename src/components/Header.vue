<script setup>
import { RouterLink } from "vue-router";
defineProps({
  totalPrice: Number
})

const emit = defineEmits(['openDrawer'])
</script>
<template>
	<header class="flex justify-between border-b border-slate-300 px-10 py-8">
        <RouterLink to="/">      
          <div class="flex items-center gap-4">
        <img src="/logo.png" alt="Logo" class="w-10">
        <div>
          <h2 class="text-xl font-bold uppercase">Vue Sneakers</h2>
          <p class="text-slate-400">Магазин кросовок</p>
        </div>
      </div>
    </RouterLink>
      <ul class="flex items-center gap-10">
        <li @click="() => emit('openDrawer')" class="flex items-center gap-3 text-gray-500 hover:text-black cursor-pointer">
          <img src="/cart.svg" alt="">
          <span>{{ totalPrice }} руб</span>
        </li>

         <RouterLink to="/favorites">      
        <li class="flex items-center gap-3 text-gray-500 hover:text-black cursor-pointer" >
          <img src="/heart.svg" alt="">
          <span>Закладки</span>
        </li>
    </RouterLink>
          <li class="flex items-center gap-3 text-gray-500 hover:text-black cursor-pointer">
          <img src="/profile.svg" alt="">
          <span>Профиль</span>
        </li>
      </ul>
    </header>
</template>