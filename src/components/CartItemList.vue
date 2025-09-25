<script setup>
import { inject } from "vue";
import CartItem from "./CartItem.vue";
const { cartItems, removeFromCart } = inject('cart')
</script>
<template>
	<div class="flex flex-col gap-5" v-auto-animate>
		<CartItem 
		v-for="item in cartItems" 
		:key="item.id" 
		:title="item.title" 
		:price = "item.price"
		:image-url = "item.imageUrl"
		@remove-from-cart = "() => removeFromCart(item)"
		/>
	</div>
	</template>