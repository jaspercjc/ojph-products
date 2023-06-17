<template>
	<div>
		<input type="search" v-model="searchTerm" @input="handleSearch" placeholder="Search products..." />
	</div>
</template>

<script setup>
import { ref } from 'vue';

const emits = defineEmits(['search']);

const searchTerm = ref('');
const searchTimer = ref(null);

const handleSearch = () => {
	clearTimeout(searchTimer.value);

	// Delay before initiating the server-side search request
	searchTimer.value = setTimeout(() => {
		emits('search', searchTerm.value);
	}, 500);
};
</script>
