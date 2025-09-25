<script setup>
import Card from "./Card.vue";

defineProps({
 items: Array,
 isFavorites:Boolean
})

const emit = defineEmits(['addToFavorite',"addToCart"])

</script>
<template>
	<div class="p-10">
		<div class="grid grid-cols-3 gap-4" v-auto-animate>
			<Card 
			v-for="item in items"
			:key="item.id"
			:id = "item.id"
			:title="item.title" 
			:price="item.price" 
			:image-url="item.imageUrl" 
			:on-click-add ="isFavorites? null :() => emit('addToCart', item)" 
			:on-click-favorite = "isFavorites? null :() => emit('addToFavorite',item)"
			:isFavorite = "item.isFavorite"
			:isAdded = "item.isAdded"
			/>
		</div>
	</div>
</template>