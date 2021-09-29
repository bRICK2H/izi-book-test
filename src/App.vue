<template>
	<div class="wrapper">

		<TheModal
			:isModal="isModal"
			@get-result="res => result.push(res)"
			@close-modal="isModal = false"
		/>
		
		<button v-if="!isModal && !result.length"
			class="start-btn wrapper__start-btn"
			@click="isModal = true"
		>
			Начать тест
		</button>
		<div v-else-if="!isModal"
			class="table-result wrapper__table-result"
		>
			<h2 class="table-result-title">Результат: </h2>
			<div v-for="({template, isResult}, i) of result"
				:key="i"
				class="table-result__table-result-item table-result-item"
				:class="isResult ? 'table-result-item--true' : 'table-result-item--false'"
			>
				{{ i + 1 }}. {{ template }}
			</div>
		</div>
	</div>
</template>

<script>
import TheModal from '@/components/modal/the-modal'

export default {
	name: 'TheMain',
	components: {
		TheModal
	},
	data: () => ({
		isModal: false,
		result: []
	}),
}
</script>

<style lang="scss">
	* {
		padding: 0;
		margin: 0;
		box-sizing: border-box;
		user-select: none;
	}
	.wrapper {
		width: 100%;
		height: 100vh;
		display: flex;

		&__start-btn,
		&__table-result {
			margin: auto;
		}
	}
	.start-btn {
		padding: 15px 20px;
		border: none;
		background-color: #1f1f1f;
		border-radius: 12px;
		font-size: 18px;
		color: #fff;
		cursor: pointer;
		transition: .1s;

		&:hover {
			background-color: #2f2f2f;
		}
	}

	.table-result {
		max-height: 50%;
		overflow: auto;
		border: 1px solid #1f1f1f;
		border-radius: 8px;
		padding: 20px;
		display: flex;
		flex-direction: column;

		&__table-result-item {
			margin-bottom: 5px;
		}
	}
	.table-result-title {
		margin-bottom: 10px;
	}
	.table-result-item {
		padding: 5px;
		border-radius: 5px;

		&--true {
			background-color: rgba(15, 81, 50, .1);
		}
		&--false {
			background-color: rgba(248, 215, 218, 1);
		}
	}
</style>