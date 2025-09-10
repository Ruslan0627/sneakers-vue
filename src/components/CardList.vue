<script setup>
import { inject } from "vue";
import Card from "./Card.vue";

defineProps({
 items: Array
})

const onClickAdd = () => {
	alert('добавитьа')
}

const emit = defineEmits(['addToFavorite'])

function onClikAdd(params) {
	
}

const addToFavorite = inject('addToFavorite') 
</script>
<template>
	<div class="p-10">
		<div class="grid grid-cols-3 gap-4">
			<Card 
			v-for="item in items"
			:key="item.id"
			:id = "item.id"
			:title="item.title" 
			:price="item.price" 
			:image-url="item.imageUrl" 
			:on-click-add ="onClickAdd" 
			:on-click-favorite = "() => addToFavorite(item)"
			:isFavorite = "item.isFavorite"
			/>
		</div>
	</div>
</template>