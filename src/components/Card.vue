<script setup>
import { inject } from "vue"
const props =	defineProps({
		id:Number,
		imageUrl:String,
		title:String,
		price:Number,
		isFavorite:Boolean,
		onClickAdd:Function,
		onClickFavorite:Function,
		isAdded: Boolean
})

const visibleFavButton = Boolean(props.onClickAdd)

</script>
<template>
	<div
		class="relative bg-white m-10 border border-slate-100 rounded-3xl p-8 cursor-pointer hover:-translate-y-2 hover:shadow-xl transition">
		<img v-if="onClickFavorite" :src="isFavorite? '/like-2.svg':'/like-1.svg'" alt="like-2" class="absolute top-8 left-8" @click="onClickFavorite">
		<img :src="imageUrl" alt="Sneaker">
		<p>{{ title }}</p>
		<div class="flex justify-between">
			<div class="mt-5 flex flex-col">
				<span class="text-slate-400">
					Цена:
				</span>
				<b>
				{{ price }}
				</b>
			</div>
			<button 
			v-if="onClickAdd"
			class="cursor-pointer"
			 @click="onClickAdd">
				<img :src="isAdded? '/checked.svg' :'/plus.svg'" alt="Plus">
			</button>
		</div>
	</div>
</template>