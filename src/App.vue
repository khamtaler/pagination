<template>
	<main class="main">
		<Suspense>
			<PagedList></PagedList>
		</Suspense>
	</main>
</template>

<script setup>
import PagedList from './components/PagedList.vue';
</script>

<style scoped lang="scss">
.main {
	width: min(70%, 1200px);
	margin: 100px auto;
}
</style>
