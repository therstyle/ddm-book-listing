<script setup>
	import {ref} from 'vue';
	import SearchBox from './SearchBox.vue';

	const props = defineProps({
		searchTerm: String,
		resultsPerPage: Number
	});

	const resultsAmount = ref(parseInt(props.resultsPerPage));

	const emit = defineEmits(['run-search', 'update-results-amount']);

	const runSearch = (value) => {
		emit('run-search', value);
	}

	const updateResultsAmount = () => {
		emit('update-results-amount', resultsAmount.value);
	}
</script>

<template>
	<header class="main-header">
			<div class="main-header__logo">
				<img src="images/ddm-logo.svg" alt="Dotdash Meredith Logo">
			</div>
			
			<SearchBox 
				:searchTerm="searchTerm"
				@run-search="runSearch"
			>
			</SearchBox>

			<div class="main-header__rpp">
				<label for="rpp-selector">Books Per Page</label>
				<select id="rpp-selector" v-model="resultsAmount" @change="updateResultsAmount">
					<option value="2">2</option>
					<option value="6">6</option>
					<option value="10">10</option>
					<option value="20">20</option>
				</select>
			</div>
		</header>
</template>

<style lang="scss">
	.main-header {
		display: flex;
		flex-wrap: wrap;
		align-items: center;
		justify-content: space-between;
		gap: var(--space-2);
		width: 100%;

		&__logo {
			img {
				max-width: 200px;
			}
		}

		&__rpp {
			display: flex;
			flex-direction: column;
			font-size: 12px;
			gap: 8px;
		}
	}
</style>