<template>
	<tr>
		<td>{{ product.id }}</td>
		<td>{{ product.title }}</td>
		<td>{{ product.description }}</td>
		<td>{{ product.price }}</td>
	</tr>
</template>

<script setup>
defineProps({
	product: {
		Type: Object,
		required: true,
	},
});
</script>
