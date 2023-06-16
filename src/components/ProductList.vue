<template>
	<div class="container">
		<h1>Product List</h1>
		<table class="table">
			<thead>
				<tr>
					<th>ID</th>
					<th>Name</th>
					<th>Description</th>
					<th>Price</th>
				</tr>
			</thead>
			<tbody>
				<ProductRow v-for="product in products" :key="product.id" :product="product" />
			</tbody>
		</table>
		<PaginationComponent :current-page="currentPage" :total-pages="totalPages" @page-changed="handlePageChange" />
	</div>
</template>

<script setup>
import { ref, onMounted } from 'vue';
import ProductRow from './ProductRow.vue';
import PaginationComponent from './PaginationComponent.vue';

const products = ref([]);
const currentPage = ref(1);
const pageSize = ref(10);
const totalPages = ref(0);

const fetchProducts = async () => {
	let skip = (currentPage.value - 1) * pageSize.value;
	let url = 'https://dummyjson.com/products';
	let urlParams = `?limit=${pageSize.value}&skip=${skip}`;
	const fullUrl = url + urlParams;
	fetch(fullUrl)
		.then((response) => response.json())
		.then((data) => {
			products.value = data.products;
			totalPages.value = Math.ceil(data.total / pageSize.value);
		})
		.catch((error) => {
			console.error('Error fetching products:', error);
		});
};

function handlePageChange(page) {
	console.log(page);
	currentPage.value = page;
	fetchProducts();
}

onMounted(() => {
	fetchProducts();
});
</script>
