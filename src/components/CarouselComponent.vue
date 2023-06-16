<template>
	<Carousel v-if="slides && slides.length">
		<Slide v-for="(slide, index) in slides" :key="slide.id">
			<div class="carousel__item">
				<button type="button" class="btn-close" aria-label="Close" @click="$emit('removeItem', index)"></button>
				<img :src="slide.thumbnail" />
			</div>
		</Slide>

		<template #addons>
			<Navigation />
			<Pagination />
		</template>
	</Carousel>
</template>

<script setup>
import 'vue3-carousel/dist/carousel.css';
import { Carousel, Navigation, Pagination, Slide } from 'vue3-carousel';

defineProps({
	slides: {
		Type: Object,
		required: true,
	},
});

defineEmits(['removeItem']);
</script>

<style>
.carousel__item {
	min-height: 50vh;
	width: 100%;
	background-color: #ffffff;
	font-size: 20px;
	border-radius: 8px;
	display: flex;
	justify-content: center;
	align-items: center;
}

.carousel__slide {
	padding: 10px;
}

.carousel__prev,
.carousel__next {
	box-sizing: content-box;
	border: 5px solid white;
}

.btn-close {
	position: absolute;
	top: 0;
	right: 20px;
}
</style>
