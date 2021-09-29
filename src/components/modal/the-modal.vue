<template>
	<div class="modal-wrapper"
		v-if="isModal"
	>

		<TheDialog/>

		<div class="modal-container modal-wrapper__modal-container">
			
			<div class="modal-content modal-container__modal-content">
				<p class="modal-content__modal-question modal-question">
					Чему равна сумма чисел?
				</p>
				<div class="equation-box">
					<span class="equation">
						{{ templateEquation }}
					</span>
					<input class="user-result"
						type="text"
						:value="userResult"
						@input="input($event)"
					>
				</div>
			</div>
			
			<div class="button-box">
				<button class="button-action button-action--close"
					title="Завершить"
					@click="close"
				>
					Завершить
				</button>
				<button class="button-action button-action--next"
					@click="next"
					:class="{ 'button-action--disabled': !userResult }"
					:disabled="!userResult"
					:title="!userResult ? 'Для продолжения вычислите сумму' : 'Продолжить'"
				>
					Продолжить
				</button>
			</div>
		</div>
	</div>
</template>

<script>
import TheDialog from './the-dialog.vue'

export default {
	name: 'TheModal',
	components: {
		TheDialog
	},
	props: {
		isModal: {
			type: Boolean,
			default: false
		}
	},
	data: () => ({
		templateEquation: '',
		userResult: '',
		randomNumbers: []
	}),
	methods: {
		input(e) {
			const { target } = e
			target.value = target.value.replace(/[\D]/, '')
			this.userResult = target.value
		},
		next() {
			const [nf, ns] = this.randomNumbers
			this.$emit('get-result', {
				template: `${nf} + ${ns} = ${+this.userResult}`,
				isResult: nf + ns === +this.userResult
			})

			this.generateTemplate()
		},
		async close() {
			const dialog = this.$children[0]
			const result = await new Promise(resolve => {
				this.$set(dialog.action, 'handler', resolve)
				this.$set(dialog.action, 'isDialog', true)
			})

			if (result) {
				this.$emit('close-modal')
			}

			this.$set(dialog.action, 'isDialog', false)

		},

		generateTemplate() {
			const [nf, ns] = this.randomNumbers = [0, 0].map(() => this.generateNumber(1, 100)) 
			this.templateEquation = `${nf} + ${ns} =`
			this.userResult = ''
		},
		generateNumber(min, max) {
			return Math.floor(Math.random() * (max + 1 - min)) + min
		}
	},
	watch: {
		isModal: {
			immediate: true,
			handler() {
				this.generateTemplate()
			}
		}
	},
}
</script>

<style lang="scss">
	.modal-wrapper {
		width: 100%;
		height: 100vh;
		background-color: rgba(0,0,0, .7);
		display: flex;
		
		position: absolute;
		top: 0;
		left: 0;

		&__modal-container {
			margin: auto;
		}
	}
	.modal-container {
		width: 500px;
		padding: 30px;
		border-radius: 8px;
		background-color: #fff;
		position: relative;

		&__modal-content {
			margin-bottom: 20px;
		}
	}
	.modal-content {
		&__modal-question {
			margin-bottom: 20px;
		}
	}
	.modal-question {
		font-size: 18px;
		font-weight: 400;
		text-align: center;
	}
	.equation-box {
		display: flex;
		justify-content: center;
	}
	.equation {
		display: inline-block;
		margin-right: 10px;
		font-size: 18px;
		font-weight: 900;
		font-style: italic;
	}
	.user-result {
		width: 50px;
		border: none;
		outline: none;
		border-bottom: 2px solid #1f1f1f;
		padding: 0 5px;
		font-size: 16px;
		text-align: center;
	}
	.button-box {
		display: flex;
		justify-content: space-between;
	}
	.button-action {
		flex: 0 1 48%;
		padding: 15px 20px;
		border: none;
		border-radius: 12px;
		cursor: pointer;
		font-size: 16px;
		font-weight: 600;
		transition: .1s;

		&--close {
			background-color: #1f1f1f;
			color: #fff;

			&:hover {
				background-color: #2f2f2f;
			}
		}
		&--next {
			background-color: #fff;
			color: #1f1f1f;
			border: 2px solid #1f1f1f;

			&:hover {
				background-color: #f1f1f1;
			}
		}
		&--disabled {
			background-color: #bfbfbf;
			opacity: .5;
			cursor: no-drop;

			&:hover {
				background-color: #bfbfbf;
			}
		}
	}
</style>