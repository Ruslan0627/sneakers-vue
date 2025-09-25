<script setup>
import { onMounted, ref } from "vue";
import axios from "axios";
import CardList from "@/components/CardList.vue";
const favorites = ref([])

onMounted(async () => {
	try {
		const { data } = await axios.get(
			"https://35b9c2053c96f902.mokky.dev/favorites"
		)

		favorites.value = data.map((fav) => fav.item)
		
	} catch (error) {
		console.log(error);
	}
})
</script>
<template>
      <h2 class="text-3xl font-bold" >Мои закладки</h2>

	<CardList :items = "favorites" :is-favorites = "true"/>
</template>