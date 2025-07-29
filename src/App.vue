<script setup>
import Header from "./components/Header.vue";
import CardList from "./components/CardList.vue";
import Drawer from "./components/Drawer.vue";
import { onMounted, reactive, ref, watch } from "vue";
import axios from "axios";

const items = ref([]);


const filters = reactive({
  sortBy: 'title',
  searchQuary: ''
});

function onChangeSelect(e) {
  filters.sortBy = e.target.value;
}

function onChangeSearch(e) {
  filters.searchQuary = e.target.value;
}

const fetchItems = async () => {
  try {
    const params = {
      sortBy: filters.sortBy,
    };

    if (filters.searchQuary) {
      params.title = `*${filters.searchQuary}*`;
    }
    const { data } = await axios.get(`https://35b9c2053c96f902.mokky.dev/items`,
      {
        params
      });
    items.value = data;
  } catch (error) {
    console.log(error);
  }
};

onMounted(fetchItems);

watch(filters, fetchItems);
</script>
<template>
  <!-- <Drawer/> -->
  <div class="w-4/5 m-auto bg-white rounded-xl shadow-xl mt-14">
    <Header />
    <div class="p-10">
      <div class="flex items-center justify-between">
        <h2 class="text-3xl font-bold">Все кросовки</h2>
        <div class="flex gap-4">
          <select @change="onChangeSelect" class="py-2 px-3 rounded-md outline-none border border-gray-100">
            <option value="name">По названию</option>
            <option value="price">По цене (дешевле)</option>
            <option value="-price">По цене (дороже)</option>
          </select>
          <div class=" relative">
            <img class="absolute top-3 left-4" src="/search.svg" alt="">
            <input @input="onChangeSearch"
              class="border border-gray-100 focus:border-gray-500 outline-none rounded-md py-2 pl-10 pr-2 " type="text"
              placeholder="Поиск...">
          </div>
        </div>
      </div>
      <CardList :items="items" />
    </div>
  </div>
</template>