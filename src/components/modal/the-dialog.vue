<template>
	<div v-if="action.isDialog"
		class="dialog-container"
	>
		<div class="dialog-container__dialog-content dialog-content">
			<p class="dialog-content__dialog-question dialog-question">
				Вы уверены, что хотите завершить тестирование?
			</p>
			<div class="dialog-button-box">
				<button class="dialog-button-action dialog-button-action--yes"
					@click="action.handler(true)"
				>Да</button>
				<button class="dialog-button-action dialog-button-action--no"
					@click="action.handler(false)"
				>Нет</button>
			</div>
		</div>
	</div>
</template>

<script>
export default {
	name: 'TheDialog',
	props: {
		action: {
			type: Object,
			default: () => ({
				handler: () => null,
				isDialog: false
			})
		}
	}
}
</script>

<style lang="scss">
	.dialog-container {
		width: 100%;
		height: 100%;
		border-radius: 8px;
		background-color: rgba(0,0,0, .9);
		display: flex;
		position: absolute;
		top: 0;
		left: 0;
		z-index: 10;

		&__dialog-content {
			margin: auto;
		}
	}
	.dialog-content {
		width: 300px;
		border-radius: 8px;
		background-color: #fff;
		padding: 15px;

		&__dialog-question {
			margin-bottom: 15px;
		}
	}
	.dialog-question {
		font-size: 18px;
		font-weight: 400;
		text-align: center;
	}
	.dialog-button-box {
		display: flex;
		justify-content: space-between;
	}
	.dialog-button-action {
		flex: 0 1 48%;
		padding: 10px 20px;
		border: none;
		border-radius: 12px;
		cursor: pointer;
		font-size: 16px;
		font-weight: 600;
		transition: .1s;

		&--yes {
			background-color: #1f1f1f;
			color: #fff;

			&:hover {
				background-color: #2f2f2f;
			}
		}
		&--no {
			background-color: #fff;
			color: #1f1f1f;
			border: 2px solid #1f1f1f;

			&:hover {
				background-color: #f1f1f1;
			}
		}
	}
</style>