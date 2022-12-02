<script setup>
	const props = defineProps({
		searchResult: Object,
		class: {
			type: String,
			default: ''
		}
	});
</script>

<template>
	<article class="search-results__result" :class="class">
		<div v-if="searchResult?.imageUrl" class="search-results__image">
			<img :src="searchResult.imageUrl" :alt="searchResult?.title" loading="lazy" />
		</div>

		<div class="search-results__detail">
			<h3 v-if="searchResult?.title">{{searchResult.title}}</h3>
			<span v-if="searchResult?.authorName" class="search-results__author">By {{searchResult?.authorName}}</span>
		</div>
	</article>
</template>

<style lang="scss" scoped>
	.search-results {
		&__result {
			display: flex;
			gap: 16px;

			h3 {
				margin: 0;
			}
		}

		&__image {
			display: flex;
			align-items: flex-end;
			
			img {
				border: 1px solid var(--light-gray);
			}
		}

		&__detail {
			display: flex;
			flex-direction: column;
			justify-content: flex-end;
		}

		&__author {
			color: var(--gray);
		}
	}
</style>