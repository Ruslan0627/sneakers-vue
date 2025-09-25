<script setup>
defineProps({
	title: String,
	description: String,
	imageUrl: String
})
</script>
<template>
<div class="w-72 mx-auto flex flex-col items-center text-center">
		<img height="70" width="70" :src="imageUrl" alt="">
	<h2 class="mt-4 text-2xl font-medium">{{ title }}</h2>
	<p class="text-gray-400">{{ description }}</p>
</div>
</template>