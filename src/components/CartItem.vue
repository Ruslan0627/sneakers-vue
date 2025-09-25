<script setup> 
defineProps({
	id:Number,
	title:String,
	imageUrl:String,
	price:Number,
})
const emit = defineEmits(['removeFromCart'])
</script>
<template>
	<div class="flex items-center gap-4 border border-slate-100 p-4 rounded-xl">
				<img :src="imageUrl" class="w-16 h-16" alt="Sneaker">
				<div class="flex flex-col">
					<p>{{ title }}</p>
					<div class="flex justify-between items-center pt-2">
						<b>{{ price }} руб</b>
						<button @click = "emit('removeFromCart')">
							<img class="opacity-40 hover:opacity-100 cursor-pointer transition" src="/close.svg" alt="">
						</button>
					</div>
				</div>
	</div>
</template>