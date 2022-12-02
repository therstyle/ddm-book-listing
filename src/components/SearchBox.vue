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
				<input type="search" v-model="currentTerm" />

				<button :disabled="(currentTermLength === 0)">
					<span class="visually-hidden">Search</span>
				</button>
			</div>
		</form>
	</div>
</template>

<style lang="scss">
</style>