<template>
	<div class="paged-list">
		<div class="paged-list--navigation">
			<button type="button" @click="backPage" class="paged-list--button">prev</button>
			<button
				type="button"
				v-for="item in Math.ceil(initialData.length / itemsOnPage)"
				:key="item"
				@click="() => goToPage(item)"
				class="paged-list--button"
			>
				{{ item }}
			</button>
			<button type="button" @click="nextPage" class="paged-list--button">next</button>
		</div>
		<ul class="paged-list--list">
			<li v-for="obj in paginatedData" :key="obj.id" class="paged-list--listItem">
				{{ obj }}
			</li>
		</ul>
	</div>
</template>

<script setup>
import axios from 'axios';
import { ref } from 'vue';

let initialData = ref([]);
const getData = async () => {
	try {
		const unorderedData = await axios
			.get('https://jsonplaceholder.typicode.com/posts')
			.then((res) => {
				localStorage.setItem('data', JSON.stringify(res.data));
				res.data.sort((a, b) => {
					if (a.title < b.title) {
						return -1;
					} else if (a.title > b.title) {
						return 1;
					} else {
						return 0;
					}
				});
				return res.data;
			});
		return unorderedData;
	} catch (err) {
		console.log(err);
	}
};
initialData.value = await getData();

let currentPage = ref(1);
const itemsOnPage = 10;
let paginatedData = ref(
	initialData.value.slice((currentPage.value - 1) * itemsOnPage, currentPage.value * itemsOnPage)
);
const nextPage = () => {
	if (currentPage.value !== Math.ceil(initialData.value.length / itemsOnPage)) {
		currentPage.value += 1;
		paginatedData.value = initialData.value.slice(
			(currentPage.value - 1) * itemsOnPage,
			currentPage.value * itemsOnPage
		);
	}
};

const backPage = () => {
	if (currentPage.value !== 1) {
		currentPage.value -= 1;
		paginatedData.value = initialData.value.slice(
			(currentPage.value - 1) * itemsOnPage,
			currentPage.value * itemsOnPage
		);
	}
};

const goToPage = (numPage) => {
	currentPage.value = numPage;
	paginatedData.value = initialData.value.slice(
		(currentPage.value - 1) * itemsOnPage,
		currentPage.value * itemsOnPage
	);
};
</script>

<style lang="scss" scoped>
.paged-list {
	display: flex;
	flex-direction: column;
	background: #fff;
	padding: 30px;
	border: 1px solid #000;
	border-radius: 30px;
	&--listItem {
		list-style: none;
		padding: 20px 0px;
		font-size: 20px;
		border-bottom: 1px solid #000;
	}
	&--navigation {
		display: flex;
		flex-wrap: wrap;
		align-items: center;
		justify-content: center;
		margin: 20px auto;
	}
	&--button {
		padding: 10px;
		margin: 2px;
		background: rgb(207, 207, 207);
		border: 1px solid rgb(139, 139, 139);
	}
	&--button:hover {
		cursor: pointer;
		background: rgb(173, 173, 173);
	}
}
</style>
