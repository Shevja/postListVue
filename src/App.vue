<script setup>
import Posts from './components/Posts.vue'
</script>

<template>
	<main>
		<Posts />
	</main>
</template>

<style lang="scss" scoped>
main {
	display: flex;
	align-items: center;
	flex-direction: column;

	& ul {
		list-style: none;
	}
}
</style>
