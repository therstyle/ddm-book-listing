<script setup>
	import {ref, computed} from 'vue';
	import loadData from './methods/loadData';
	import AppHeader from './components/AppHeader.vue';
	import SearchResults from './components/SearchResults.vue';

	const searchTerm = ref('');
	const searchResults = ref([]);
	const page = ref(1);
	const resultsPerPage = ref(10);
	const loading = ref(false);

	const arrayIndex = computed(() => page.value - 1);
	const startValue = computed(() => arrayIndex.value * resultsPerPage.value);
	const endValue = computed(() => startValue.value + resultsPerPage.value);
	const currentResultsSet = computed(() => searchResults.value.slice(startValue.value, endValue.value));
	const resultsLength = computed(() => searchResults.value.length);
	const totalPages = computed(() => Math.floor(resultsLength.value / resultsPerPage.value));

	const resetPage = () => {
		page.value = 1;
	}

	const setPage = (value) => {
		page.value = value;
	}

	const updateResultsAmount = (value) => {
		resetPage();
		resultsPerPage.value = parseInt(value);
	}

	const runSearch = async (value) => {
		loading.value = true;
		searchTerm.value = value;
		resetPage();

		const data = await loadData(`https://goodreads-server-express--dotdash.repl.co/search/${searchTerm.value};`);

		if (!data) {return};
		searchResults.value = data?.list;
		loading.value = false;
	}
</script>

<template>
	<AppHeader
		:searchTerm="searchTerm"
		:resultsPerPage="resultsPerPage"
		@run-search="runSearch"
		@update-results-amount="updateResultsAmount"
	>
	</AppHeader>
	
  <main>
		<SearchResults 
			:searchResults="currentResultsSet"
			:searchTerm="searchTerm"
			:loading="loading"
			:totalPages="totalPages"
			:page="page"
			@set-page="setPage"
			>
		</SearchResults>
  </main>
</template>

<style lang="scss">
	@import './assets/css/vars';
	@import './assets/css/global';
</style>
