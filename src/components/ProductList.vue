<template>
	<div class="container">
		<h1>Product List</h1>
		<product-search @search="handleSearch"></product-search>
		<table class="table">
			<thead>
				<tr>
					<th>ID</th>
					<th>Name</th>
					<th>Description</th>
					<th>Price</th>
					<th>Actions</th>
				</tr>
			</thead>
			<tbody>
				<ProductRow v-for="product in products" :key="product.id" :product="product">
					<button type="button" class="btn btn-primary" :class="isDisabled(product)" @click="addToSlides(product)">Add to Slide</button>
				</ProductRow>
			</tbody>
		</table>
		<PaginationComponent :current-page="currentPage" :total-pages="totalPages" @page-changed="handlePageChange" />

		<CarouselComponent :slides="slides" @remove-item="removeFromSlides" />
	</div>
</template>

<script setup>
import { ref, onMounted } from 'vue';
import ProductRow from './ProductRow.vue';
import PaginationComponent from './PaginationComponent.vue';
import ProductSearch from './ProductSearch.vue';
import CarouselComponent from './CarouselComponent.vue';

const products = ref([]);
const currentPage = ref(1);
const pageSize = ref(10);
const totalPages = ref(0);
const searchTerm = ref('');
const slides = ref([]);

const fetchProducts = async () => {
	let skip = (currentPage.value - 1) * pageSize.value;
	let url = 'https://dummyjson.com/products';
	let urlParams = `?limit=${pageSize.value}&skip=${skip}`;
	if (searchTerm.value) {
		url += '/search';
		urlParams += `&q=${searchTerm.value}`;
	}
	const fullUrl = url + urlParams;
	await fetch(fullUrl)
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
	currentPage.value = page;
	fetchProducts();
}

function handleSearch(keyword) {
	console.log(keyword);

	searchTerm.value = keyword;
	fetchProducts();
}

const isDisabled = (product) => {
	return slides.value.findIndex((x) => x.id == product.id) != -1 && 'disabled';
};
function addToSlides(product) {
	slides.value = [...slides.value, product];
}
function removeFromSlides(index) {
	slides.value.splice(index, 1);
}
onMounted(() => {
	fetchProducts();
});
</script>
