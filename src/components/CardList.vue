<script setup>
import Card from "./Card.vue";
defineProps({
	items:Array
})
</script>
<template>
	<div class="p-10">
		<div class="grid grid-cols-3 gap-4">
			<Card 
			v-for="item in items"
			:key="item.id"
			:title="item.title"
			:price="item.price" 
			:imageUrl="item.imageUrl"
			 />
		</div>
	</div>
</template>