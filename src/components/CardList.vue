<script setup>
import Card from "./Card.vue";
import CartItem from "./CartItem.vue";

defineProps({
 items: Array
})

const onClickAdd = () => {
	alert('добавитьа')
}
</script>
<template>
	<div class="p-10">
		<div class="grid grid-cols-3 gap-4">
			<Card 
			v-for="item in items"
			:key="item.id"
			:title="item.title" 
			:price="item.price" 
			:image-url="item.imageUrl" 
			:on-click-add ="onClickAdd" 
			/>
		</div>
	</div>
</template>