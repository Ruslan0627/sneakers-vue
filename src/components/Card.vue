<script setup>
defineProps(
	{
		imageUrl:String,
		title:String,
		price:Number,
		isFavorite:Boolean,
		isAdded:Boolean,
		onClickAdd:Function,
		onClickFavorite:Function
	})
	function ClickAdd () {
	alert("добавить")
	}
</script>
<template>
	<div
		class="relative bg-white m-10 border border-slate-100 rounded-3xl p-8 cursor-pointer hover:-translate-y-2 hover:shadow-xl transition">
		<img :src="isFavorite? '/like-2.svg':'/like-1.svg'" alt="like-2" class="absolute top-8 left-8" @click="onClickFavorite">
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
			<button class="cursor-pointer" @click="onClickAdd">
				<img :src="isAdded? '/plus.svg' :'/checked.svg'" alt="Plus">
			</button>
		</div>
	</div>
</template>