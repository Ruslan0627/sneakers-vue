<script setup>
import Header from "./components/Header.vue";
import CardList from "./components/CardList.vue";
import Drawer from "./components/Drawer.vue";
import { ref } from "vue";
import { reactive } from "vue";
import { onMounted } from "vue";
import axios from "axios";
import { watch } from "vue";
import { provide } from "vue";

const drawerOpen = ref(false)

const items = ref([])

const cartItems = ref([])

const filters = reactive({
  sortBy:'title',
  searchTitle:''
})

function onChangeSelect (e) {
  filters.sortBy = e.target.value
}

function onChangeSearchTitle (e) {
  filters.searchTitle = e.target.value
}

async function fetchFavorites () {
const { data:favorites } = await axios.get("https://35b9c2053c96f902.mokky.dev/favorites")
items.value = items.value.map( item => {
  const favorite = favorites.find( favorite => favorite.parentId === item.id )

  if (!favorite) {
    return item 
  }
  return {
    ...item,
    isFavorite:true,
    favoriteId: favorite.id
    }
} )
}
async function fetchItems () {
try {
  const params = {
    sortBy: filters.sortBy
  }
  if (filters.searchTitle) {
    params.title = `*${filters.searchTitle}*` 
  }
  const { data } = await axios.get("https://35b9c2053c96f902.mokky.dev/items",
    {
      params
    }
  )
items.value = data.map( elem => (
  {
  ...elem,
  isFavorite:false,
  isAdded:false,
  favoriteId:null
  }))
} catch (error) {
  console.log(error);
}
}

async function addToFavorite (item) {
  try 
  {
    if (!item.isFavorite) {
        item.isFavorite = true
      const obj = {
        parentId: item.id
      }
      const { data } = await axios.post("https://35b9c2053c96f902.mokky.dev/favorites",obj)
      item.favoriteId = data.id
      console.log(data);   
    }
    else {
      item.isFavorite = false
      await axios.delete(`https://35b9c2053c96f902.mokky.dev/favorites/${item.favoriteId}`)
    }
    } catch (error) {
      console.log(error);
    }
} 

function openDrawer () {
  drawerOpen.value = true
}

function closeDrawer () {
  drawerOpen.value = false
}

onMounted( async () => {
  await fetchItems()
  await fetchFavorites()
})
provide('addToFavorite',addToFavorite)
provide('cartActions',{
  openDrawer,
  closeDrawer
})
watch(filters,fetchItems)
</script>
<template>
  <Drawer v-if="drawerOpen"/>
  <div class="w-4/5 m-auto bg-white rounded-xl shadow-xl mt-14">
    <Header @open-drawer = "openDrawer" />
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
            <input @change="onChangeSearchTitle" class="border border-gray-100 focus:border-gray-500 outline-none rounded-md py-2 pl-10 pr-2 " type="text"
            placeholder="Поиск...">
          </div>
        </div>
      </div>
      <CardList @add-to-favorite ="addToFavorite" :items = "items" />
    </div>
  </div>
</template>