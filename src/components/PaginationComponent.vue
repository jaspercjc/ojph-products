<template>
	<nav>
		<ul class="pagination">
			<li class="page-item" :class="{ disabled: currentPage === 1 }">
				<a class="page-link" href="#" @click="previousPage">Previous</a>
			</li>
			<li class="page-item" v-for="page in totalPages" :key="page" :class="{ active: currentPage === page }">
				<a class="page-link" href="#" @click="changePage(page)">{{ page }}</a>
			</li>
			<li class="page-item" :class="{ disabled: currentPage === totalPages }">
				<a class="page-link" href="#" @click="nextPage">Next</a>
			</li>
		</ul>
	</nav>
</template>

<script setup>
const props = defineProps({
	currentPage: {
		type: Number,
		required: true,
	},
	totalPages: {
		type: Number,
		required: true,
	},
});

const emits = defineEmits(['pageChange']);
function previousPage() {
	if (props.currentPage > 1) {
		changePage(props.currentPage - 1);
	}
}
function nextPage() {
	if (props.currentPage < props.totalPages) {
		changePage(props.currentPage + 1);
	}
}
function changePage(page) {
	emits('page-changed', page);
}
</script>
