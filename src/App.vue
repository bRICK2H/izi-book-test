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
		<template v-else-if="!isModal">

			<div class="wrapper__container-result container-result">
				<div class="table-result container-result__table-result">
					<h2 class="table-result-title">Результат: </h2>
					<div v-for="({template, isResult}, i) of result"
						:key="i"
						class="table-result__table-result-item table-result-item"
						:class="isResult ? 'table-result-item--true' : 'table-result-item--false'"
					>
						{{ i + 1 }}. {{ template }}
					</div>
				</div>
				<div class="total-result">
					<p class="total-result-box">
						<span class="total-result-item">Правильных ответов:</span>
						{{ getAmountResult(true) }}
					</p>
					<p class="total-result-box">
						<span class="total-result-item">Неправильных ответов:</span>
						{{ getAmountResult(false) }}
					</p>
				</div>
			</div>
		</template>
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
	methods: {
		getAmountResult(bool) {
			return this.result.filter(({ isResult }) => isResult === bool).length
		}
	}
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
		&__container-result {
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
	.container-result {

		&__table-result {
			margin-bottom: 20px;
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
	.total-result {
		display: flex;
		flex-direction: column;
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
	.total-result-box {
		display: flex;
		justify-content: space-between;
	}
	.total-result-item {
		display: inline-block;
		font-weight: 600;
		font-style: italic;
		margin-right: 10px;
	}
</style>