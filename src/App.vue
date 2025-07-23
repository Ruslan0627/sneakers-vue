<script setup>
import Header from "./components/Header.vue";
import CardList from "./components/CardList.vue";
import Drawer from "./components/Drawer.vue";
</script>
<template>
  <!-- <Drawer/> -->
  <div class="w-4/5 m-auto bg-white rounded-xl shadow-xl mt-14">
    <Header />
    <div class="p-10">
      <div class="flex items-center justify-between">
        <h2 class="text-3xl font-bold">Все кросовки</h2>
        <div class=" relative">
          <img class="absolute top-3 left-4" src="/search.svg" alt="">
          <input class="border border-gray-100 focus:border-gray-500 outline-none rounded-md py-2 pl-10 pr-2 " type="text"
            placeholder="Поиск...">
        </div>
      </div>
      <CardList />
    </div>
  </div>
</template>