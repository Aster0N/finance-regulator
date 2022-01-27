<template>
	<div id="app">
		<section class="welcome-page">
			<div class="page-title">finance regulator</div>
			<div class="page-workflow">
				<div class="workflow-body">
					<div class="workflow-user-input"></div>
					<div class="workflow-output"></div>
				</div>
			</div>
		</section>

		<main class="regulator-core">
			<div class="container">
				<div class="calculation-settings">
					<div class="question">
						<div class="question-block">
							<span class="question-number">1.</span>&nbsp;
							<span class="question-title"
								>Count from your income or specific
								amount?</span
							>
						</div>
						<div class="answer-block">
							<div class="answers-wrapper">
								<div @click="fromIncome" class="first-answer">
									<img
										class="answer-point"
										src="./assets/img/arrow-point.svg"
										alt="arrow"
									/>
									<button
										class="answer-from-income answer-btn"
									>
										from income
									</button>
								</div>
								<div @click="fromAmount" class="second-answer">
									<img
										class="answer-point"
										src="./assets/img/arrow-point.svg"
										alt="arrow"
									/>
									<button
										class="answer-from-amount answer-btn"
									>
										from specific amount
									</button>
								</div>
							</div>
						</div>
					</div>

					<div class="question">
						<div class="question-block">
							<span class="question-number">2</span>&nbsp;
							<span class="question-title"
								>Enter your monetary expenditures</span
							>
						</div>
						<div class="answer-block">
							<img
								class="answer-point"
								src="./assets/img/arrow-point.svg"
								alt="arrow"
							/>
							<span class="answer-section answer-section-2">
								<input
									v-model="inputModels.secAnswer"
									class="answer"
									type="text"
								/>
							</span>
						</div>
					</div>

					<div class="question">
						<div class="question-block">
							<span class="question-number">3</span>&nbsp;
							<span class="question-title"
								>Enter your monetary expenditures</span
							>
						</div>
						<div class="answer-block">
							<img
								class="answer-point"
								src="./assets/img/arrow-point.svg"
								alt="arrow"
							/>
							<span class="answer-section answer-section-3">
								<input
									v-model="inputModels.thirdAnswer"
									class="answer"
									type="text"
								/>
							</span>
						</div>
					</div>

					<div class="question">
						<div class="question-block">
							<span class="question-number">4</span>&nbsp;
							<span class="question-title"
								>If you are willing to deposit in a bank with
								interest, please enter the amount and %</span
							>
						</div>
						<div class="answer-block">
							<img
								class="answer-point"
								src="./assets/img/arrow-point.svg"
								alt="arrow"
							/>
							<span class="answer-section answer-section-4">
								<input
									v-model="inputModels.fourthAnswer"
									class="answer"
									type="text"
								/>
							</span>
						</div>
					</div>
				</div>
			</div>
		</main>
	</div>
</template>

<script>
export default {
	name: "App",
	data() {
		return {
			inputModels: {
				secAnswer: "",
				thirdAnswer: "",
				fourthAnswer: "",
			},
		};
	},
	methods: {
		watchInput(inputValue, id) {
			const inputWrapper = document.querySelector(
				`.answer-section-${id}`
			);
			if (/^[0-9]*$/.test(inputValue)) {
				if (inputWrapper.classList.contains("invalid-input")) {
					inputWrapper.classList.remove("invalid-input");
				}
			} else {
				inputWrapper.classList.add("invalid-input");
			}
		},
		checkPressBtnClass(btn) {
			if (btn.classList.contains("pressed-btn")) {
				btn.classList.remove("pressed-btn");
			} else {
				btn.classList.add("pressed-btn");
			}
		},
		fromIncome() {
			const clickedButton = document.querySelector(".answer-from-income");
			this.checkPressBtnClass(clickedButton);
		},
		fromAmount() {
			const clickedButton = document.querySelector(".answer-from-amount");
			this.checkPressBtnClass(clickedButton);
		},
	},
	watch: {
		"inputModels.secAnswer"(value) {
			this.watchInput(value, 2);
		},
		"inputModels.thirdAnswer"(value) {
			this.watchInput(value, 3);
		},
		"inputModels.fourthAnswer"(value) {
			this.watchInput(value, 4);
		},
	},
};
</script>

<style lang="scss">
@import "./assets/scss/style.scss";
@import "./assets/scss/variables.scss";

.welcome-page {
	height: 100vh;
	background: url("./assets/img/welcome-page-bg.svg") no-repeat center center;
	background-color: $light-blue;

	.workflow-body {
		height: 100%;
		width: 100%;
		background-color: $secondary-color;
	}
	.page-title {
		width: 100%;
		padding: 100px 0 0 0;
		text-align: center;
		font-family: "Montserrat", serif;
		font-size: 40px;
		font-weight: regular;
		text-transform: uppercase;
		letter-spacing: 30px;
		color: #fff;
	}
}

.regulator-core {
	padding: 200px 0 200px 0;
	background-color: $light-blue;
	font-family: "Palanquin", sans-serif;
	font-size: 20px;
	color: #fff;

	.calculation-settings {
		width: 990px;
		padding: 50px 60px;
		background-color: $primary-color;

		.question {
			font-weight: bold;

			.question-block {
				margin: 0 0 15px 0;
			}
			.answer-block {
				.answer-point {
					margin: 0 15px 0 0;
				}
			}
		}
	}

	.answer-btn {
		background-color: transparent;
		outline: none;
		border: none;
		color: #fff;
		font-size: 20px;
		cursor: pointer;

		&:hover {
			text-decoration: underline;
		}
	}
	.pressed-btn {
		text-decoration: underline;
	}

	.answers-wrapper {
		display: flex;
		justify-content: flex-start;
		align-items: center;
		column-gap: 70px;
	}
	.question:not(:last-child) {
		margin: 0 0 20px 0;
	}

	.answer-section {
		border-bottom: 1px solid #fff;

		.answer {
			background-color: transparent;
			outline: none;
			border: none;
			position: relative;
			color: #fff;
			font-size: 18px;
		}
	}
	.answer-section.invalid-input {
		border-bottom: 1px solid #e74c3c;
	}
}
</style>