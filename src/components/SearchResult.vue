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
		<h3 v-if="searchResult?.title">{{searchResult.title}}</h3>
	</article>
</template>