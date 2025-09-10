<script setup>
import DrawerHead from "./DrawerHead.vue";
import CartItemList from "./CartItemList.vue";
</script>
<template >
	<div class="fixed top-0 left-0 h-full w-full bg-black z-10 opacity-70"></div>
	<div class="bg-white w-[384px] h-full fixed right-0 top-0 z-20 p-8 overflow-y-auto">
		<DrawerHead />
		<CartItemList />
		<div class="flex flex-col gap-4 mb-6 my-7">
			<div class="flex gap-2">
				<span>Итого:</span>
				<div class="flex-1 border-b border-gray-200"></div>
				<b>12999 ₽</b>
			</div>
				<div class="flex gap-2">
				<span>Налог: 5%</span>
				<div class="flex-1 border-b border-gray-200"></div>
				<b>900 ₽</b>
			</div>
			<button class="
			transition
			 bg-lime-500
			  rounded-xl 
				py-3
				 text-white
				  hover:bg-lime-600
					 active:hover:bg-lime-700
					 cursor-pointer">
				Оформить заказ
			</button>
		</div>
	</div>
</template>