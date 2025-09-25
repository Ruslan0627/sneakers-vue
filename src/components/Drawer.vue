<script setup>
import DrawerHead from "./DrawerHead.vue";
import CartItemList from "./CartItemList.vue";
import InfoBlock from "@/InfoBlock.vue";
import { inject, ref, computed } from "vue";
import axios from "axios";

const props = defineProps({
  totalPrice: Number,
  vatPrice: Number,
})

const {
  cartItems,
  closeDrawer,
} = inject('cart')

const isCreating = ref(false)
const orderId = ref(null)
async function createOrder () {
  try {
    isCreating.value = true
    const { data } = await axios.post("https://35b9c2053c96f902.mokky.dev/orders", {
      items: cartItems.value,
      totalPrice: props.totalPrice,
    })
    cartItems.value = []

		orderId.value = data.id
  }
  catch (error) {
    console.log(error) 
  } finally {
    isCreating.value = false
  }
}

const cartIsEmpty = computed(() => cartItems.value.length === 0)
const cartButtonDisabled = computed(() => isCreating.value || cartIsEmpty.value )
</script>

<template>
  <div class="fixed top-0 left-0 h-full w-full bg-black z-10 opacity-70"></div>
  <div class="bg-white w-[384px] h-full fixed right-0 top-0 z-20 p-8 overflow-y-auto">
    <DrawerHead />
    <div v-if="!totalPrice || orderId" class="flex h-full items-center">
			<InfoBlock 
			v-if="!totalPrice && !orderId"
			title="Корзина пустая" 
			description="Добавьте хоть одну пару кроссовок, чтоб сделать заказ" 
			image-url="/package-icon.png"
      />
      <InfoBlock 
			v-if="orderId"
			title="Заказ оформлен" 
			:description="`Ваш заказ #${orderId} скоро будет передан курьерской доставке`"
			image-url="/order-success-icon.png"
      />
    </div>
    <template v-else>
      <CartItemList />

      <div class="flex flex-col gap-4 mb-6 my-7">
        <div class="flex gap-2">
          <span>Итого:</span>
          <div class="flex-1 border-b border-gray-200"></div>
          <b>{{ totalPrice }} ₽</b>
        </div>
        <div class="flex gap-2">
          <span>Налог:</span>
          <div class="flex-1 border-b border-gray-200"></div>
          <b>{{ vatPrice }} ₽</b>
        </div>
        <button
          class="transition bg-lime-500 rounded-xl py-3 text-white hover:bg-lime-600 active:hover:bg-lime-700 cursor-pointer"
          :disabled="cartButtonDisabled"
          @click="createOrder"
        >
          {{ isCreating ? "Идёт оформление заказа" : "Оформить заказ" }}
        </button>
      </div>
    </template>
  </div>
</template>
