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
import { computed } from "vue";
import Home from "./pages/Home.vue";

/* корзина(START) */
const cartItems = ref([])

const isCreatingOrder = ref(false)
const drawerOpen = ref(false)

const totalPrice = computed(() => cartItems.value.reduce( (acc, item ) => acc + item.price,0))

const vatPrice = computed(() => Math.round((totalPrice.value * 5) / 100))

const cartIsEmpty = computed( () => cartItems.value.length === 0)

const cartButtonDisabled = computed(() => isCreatingOrder.value || cartIsEmpty.value )

async function addToFavorite (item) {
  try 
  {
    if (!item.isFavorite) {
        item.isFavorite = true
      const obj = {
        item_id: item.id
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

function addToCart (item) {
  item.isAdded = true
    cartItems.value.push(item) 
}

function removeFromCart (item) {
  item.isAdded = false
  cartItems.value = cartItems.value.filter(cartItem => cartItem.id !== item.id);
}

// async function createOrder () {
//   try {
//     isCreatingOrder.value = true
//       const { data } = await axios.post("https://35b9c2053c96f902.mokky.dev/orders", {
//         items: cartItems.value,
//         totalPrice: totalPrice.value,
//       })
//       cartItems.value = []
//       return data
//   }
//   catch (error) {
//     console.log(error) 
//   } finally {
//     isCreatingOrder.value = false
//   }
// }

provide('addToFavorite',addToFavorite)
provide('cart',{
  cartItems,
  openDrawer,
  closeDrawer,
  removeFromCart,
  addToCart,
})
/* корзина(FINISH) */

watch(
cartItems,
 () => {
  localStorage.setItem("cart", JSON.stringify(cartItems.value))
},
{ deep: true}
)

</script>
<template>
  <Drawer 
  :total-price ="totalPrice" 
  :vat-price = "vatPrice" 
  v-if="drawerOpen" 
  @createOrder = "createOrder" 
  />
  <div class="w-4/5 m-auto bg-white rounded-xl shadow-xl mt-14">
    <Header :total-price = "totalPrice" @openDrawer = "openDrawer" />
    <div class="p-10">
      <router-view>

      </router-view>
    </div>
  </div>
</template>