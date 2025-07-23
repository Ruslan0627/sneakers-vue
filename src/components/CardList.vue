<script setup>
import Card from "./Card.vue";
</script>
<template>
	<div class="p-10">
		<div class="grid grid-cols-3 gap-4">
			<Card :price="2000" title="Nike Blazer" image-url="/sneakers/sneakers-3.jpg" />
		</div>
	</div>
</template>