<script setup>
import {computed} from 'vue';
import SearchResult from './SearchResult.vue';

const props = defineProps({
	searchResults: Array,
	searchTerm: String,
	loading: Boolean,
	totalPages: Number,
	page: Number
});

const emit = defineEmits(['set-page']);
const resultsLength = computed(() => props.searchResults ? props.searchResults.length : 0);
const termLength = computed(() => props.searchTerm ? props.searchTerm.length : 0);

const decreasePage = () => {
	emit('set-page', props.page - 1);
}

const increasePage = () => {
	emit('set-page', props.page + 1);
}
</script>

<template>
	<section class="search-results">
		<div v-if="(resultsLength === 0 && termLength > 0 && !loading)">Sorry, no results.</div>
		<div v-else class="search-results__container">
			<SearchResult 
				v-for="(searchResult, index) in searchResults"
				:searchResult="searchResult"
				:key="index"
			>
			</SearchResult>
		</div>

		<div v-if="(totalPages > 1)" class="search-results__pagination">
			<button 
				:disabled="(page === 1)"
				@click="decreasePage"
			>
			Prev</button>

			<button 
				:disabled="(page === totalPages)"
				@click="increasePage"
			>
			Next</button>
		</div>
	</section>
</template>

<style lang="scss">
</style>