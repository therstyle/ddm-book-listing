<script setup>
import {ref, computed} from 'vue';
import SearchResult from './SearchResult.vue';

const props = defineProps({
	searchResults: Array,
	searchTerm: String,
	loading: Boolean,
	totalPages: Number,
	page: Number
});

const emit = defineEmits(['set-page']);
const container = ref(null);
const resultsLength = computed(() => props.searchResults ? props.searchResults.length : 0);
const termLength = computed(() => props.searchTerm ? props.searchTerm.length : 0);

const decreasePage = () => {
	emit('set-page', props.page - 1);
	scrollToTop();
}

const increasePage = () => {
	emit('set-page', props.page + 1);
	scrollToTop();
}

const scrollToTop = () => {
	window.scroll({
		behavior: 'smooth',
		left: 0,
		top: container.value.offsetTop
	});
}
</script>

<template>
	<section class="search-results" :data-loading="loading" ref="container">
		<small v-if="(termLength > 0 && !loading)" class="search-results__status">Displaying search results for {{searchTerm}}</small>

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

<style lang="scss" scoped>
	@import '../assets/css/vars.scss';
	.search-results {
		display: flex;
		flex-direction: column;
		position: relative;

		&::before {
			--position: center;

			@include mobile {
				--position: top center;
			}

			content: '';
			display: block;
			background-image: url('images/loading.svg');
			background-repeat: no-repeat;
			background-size: 100px 100px;
			background-position: var(--position);
			position: absolute;
			top: 0;
			left: 0;
			right: 0;
			bottom: 0;
			opacity: 0;
			z-index: -1;
			transition: var(--transition-duration) all ease-in-out;
		}

		> * {
			transition: var(--transition-duration) all ease-in-out;
		}

		&[data-loading="true"] {
			&::before {
				opacity: 1;
				z-index: 0;
			}

			> * {
				opacity: 0;
			}
		}

		&__status {
			margin-bottom: var(--space-1);
		}

		&__container {
			--columns: 2;

			@include mobile {
				--columns: 1;
			}

			display: grid;
			grid-template-columns: repeat(var(--columns), 1fr);
			gap: 32px;
		}

		&__pagination {
			display: flex;
			justify-content: center;
			gap: 16px;
			margin-top: var(--space-2);

			> button {
				background: none;
				border: 2px solid var(--black);
				transition: var(--transition-duration) all ease-in-out;
				padding: 12px 24px;
				border-radius: 32px;

				&[disabled] {
					border-color: var(--gray);
					color: var(--gray);
					opacity: 0.5;
					pointer-events: none;
				}

				&:hover,
				&:focus {
					cursor: pointer;
					background: var(--black);
					color: var(--white);
				}
			}
		}
	}
</style>