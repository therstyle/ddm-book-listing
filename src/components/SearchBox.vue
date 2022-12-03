<script setup>
	import {ref, computed} from 'vue';

	const props = defineProps({
		searchTerm: String
	});

	const currentTerm = ref('');
	const currentTermLength = computed(() => currentTerm.value.length);
</script>

<template>
	<div class="search-box">
		<form @submit.prevent="onSubmit" @submit="$emit('run-search', currentTerm)">
			<div class="search-box__input">
				<label for="search" class="visually-hidden">Book Search</label>
				<input id="search" type="search" v-model="currentTerm" />

				<button :disabled="(currentTermLength === 0)">
					<span class="visually-hidden">Submit</span>
				</button>
			</div>
		</form>
	</div>
</template>

<style lang="scss" scoped>
	.search-box {
		flex: 1;

		&__input {
			border: 1px solid var(--light-gray);
			border-radius: 32px;
			display: flex;
			gap: 12px;
			align-items: center;
			overflow: hidden;
			padding: 8px 24px;

			> input {
				flex: 1;
				font-family: var(--main-font);
				font-size: 24px;
				border: none;
				padding: 4px 12px;
				display: flex;
				align-items: center;
			}

			> button {
				background: none;
				border: none;
				width: 32px;
				height: 32px;
				display: flex;
				justify-content: center;
				align-items: center;
				padding: 0;

				&:hover {
					&::before {
						background: var(--orange);
					}
				}

				&::before {
					content: '';
					width: 24px;
					height: 24px;
					display: block;
					background: var(--light-gray);
					mask-image: url('images/search.svg');
					mask-size: cover;
					mask-repeat: no-repeat;
					-webkit-mask-image: url('images/search.svg');
					-webkit-mask-size: cover;
					-webkit-mask-repeat: no-repeat;
					transition: var(--transition-duration) all ease-in-out;
				}
			}
		}
	}
</style>