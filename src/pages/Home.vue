<script setup>
import CardList from "../components/CardList.vue";
import axios from "axios";
import { reactive, watch, ref, onMounted, inject } from "vue";
import debounce from "lodash.debounce";

const { cartItems, addToCart, removeFromCart } = inject("cart");

const items = ref([]);

const filters = reactive({
  sortBy: "title",
  searchTitle: ""
});

function onClickAddPlus(item) {
  if (!item.isAdded) {
    addToCart(item);
  } else {
    removeFromCart(item);
  }
}

function onChangeSelect(e) {
  filters.sortBy = e.target.value;
}

const onChangeSearchTitle = debounce( (e) => {
  filters.searchTitle = e.target.value;
},500)

async function addToFavorite(item) {
  try {
    if (!item.isFavorite) {
      item.isFavorite = true;
      const obj = {
        parentId: item.id,
				item
      };
      const { data } = await axios.post("https://35b9c2053c96f902.mokky.dev/favorites", obj);
      item.favoriteId = data.id;
    } else {
      item.isFavorite = false;
      await axios.delete(`https://35b9c2053c96f902.mokky.dev/favorites/${item.favoriteId}`);
    }
  } catch (error) {
    console.log(error);
  }
}

async function fetchFavorites() {
  const { data: favorites } = await axios.get("https://35b9c2053c96f902.mokky.dev/favorites?_relations=items");
  items.value = items.value.map(item => {
    const favorite = favorites.find(fav => fav.item_id === item.id);
    return favorite
      ? { ...item, isFavorite: true, favoriteId: favorite.id }
      : item;
  });
}

async function fetchItems() {
  try {
    const params = { sortBy: filters.sortBy };
    if (filters.searchTitle) params.title = `*${filters.searchTitle}*`;

    const { data } = await axios.get("https://35b9c2053c96f902.mokky.dev/items", { params });
    items.value = data.map(elem => ({
      ...elem,
      isFavorite: false,
      isAdded: cartItems.value.some(cartItem => cartItem.id === elem.id),
      favoriteId: null
    }));
  } catch (error) {
    console.log(error);
  }
}

onMounted(async () => {
  const localCart = localStorage.getItem("cart");
  cartItems.value = localCart ? JSON.parse(localCart) : [];

  await fetchItems();
  await fetchFavorites();
  items.value = items.value.map(item => ({
    ...item,
    isAdded: cartItems.value.some(cartItem => cartItem.id === item.id)
  }));
});
watch(cartItems, () => {
  items.value = items.value.map(item => ({
    ...item,
    isAdded: cartItems.value.some(cartItem => cartItem.id === item.id)
  }));
}, { deep: true });

// Слежение за фильтрами
watch(filters, fetchItems);
</script>

<template>
  <div>
    <div class="flex items-center justify-between">
      <h2 class="text-3xl font-bold">Все кроссовки</h2>
      <div class="flex gap-4">
        <select
          @change="onChangeSelect"
          class="py-2 px-3 rounded-md outline-none border border-gray-100"
        >
          <option value="name">По названию</option>
          <option value="price">По цене (дешевле)</option>
          <option value="-price">По цене (дороже)</option>
        </select>
        <div class="relative">
          <img class="absolute top-3 left-4" src="/search.svg" alt="" />
          <input
            @change="onChangeSearchTitle"
            class="border border-gray-100 focus:border-gray-500 outline-none rounded-md py-2 pl-10 pr-2"
            type="text"
            placeholder="Поиск..."
          />
        </div>
      </div>
    </div>
    <CardList
      :items="items"
      @add-to-cart="onClickAddPlus"
      @add-to-favorite="addToFavorite"
    />
  </div>
</template>
