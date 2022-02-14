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
						<div class="answer-block answer-block-1">
							<div class="answers-wrapper">
								<div @click="fromIncome" class="first-answer">
									<img
										class="answer-point"
										src="./assets/img/arrow-point.svg"
										alt="arrow"
									/>
									<button
										class="
											answer-from-income answer-btn
											pressed-btn
										"
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
						<div class="answer-block answer-block-2">
							<span class="answer-title" v-if="isFromIncome">
								Enter your income:
							</span>
							<span
								class="asnwer-title"
								v-if="isFromSpecificAmount"
							>
								Enter the amount of money: </span
							><br />
							<span class="answer-section answer-section-1">
								<input
									v-model="inputModels.usersMoney"
									class="answer"
									type="text"
							/></span>
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
									v-model="inputModels.usersExpenditures"
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
								>Money that you can save each month</span
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
									v-model="inputModels.moneyToSave"
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
						<div class="warning-title">
							Considering that interest is accrued on the entire
							current amount, not on the amount of the deposit.
						</div>
						<div class="answer-block answer-block-4">
							<div>
								<span class="answer-title"
									>Amount of money:</span
								><br />
								<img
									class="answer-point"
									src="./assets/img/arrow-point.svg"
									alt="arrow"
								/>
								<span class="answer-section answer-section-4">
									<input
										v-model="
											inputModels.usersInvestments.amount
										"
										class="answer"
										type="text"
									/>
								</span>
							</div>
							<div>
								<span class="answer-title"> Percent:</span
								><br />
								<img
									class="answer-point"
									src="./assets/img/arrow-point.svg"
									alt="arrow"
								/>
								<span class="answer-section answer-section-5">
									<input
										v-model="
											inputModels.usersInvestments.percent
										"
										class="answer"
										type="text"
									/>
								</span>
							</div>
						</div>
					</div>

					<div class="calculate-btn-wrapper">
						<button
							:class="{
								'calculate-disabled':
									isAnyError || emptyInputFields(),
							}"
							class="calculate-btn"
							:disabled="isAnyError || emptyInputFields()"
							@click="calculate"
						>
							calculate
						</button>
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
				usersMoney: "",
				usersExpenditures: "",
				moneyToSave: "",
				usersInvestments: {
					amount: "",
					percent: "",
				},
			},
			emptyInputFields() {
				for (let key in this.inputModels) {
					if (!this.inputModels[key] && key !== "usersInvestments") {
						return true;
					}
				}
				return false;
			},
			isFromIncome: true,
			isFromSpecificAmount: false,
			isAnyError: false,
			maxPeriod: 10,
			usersSavings: {},
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
					this.isAnyError = false;
				}
			} else {
				inputWrapper.classList.add("invalid-input");
				this.isAnyError = true;
			}
		},
		checkPressBtnClass(btn) {
			const anothertBtn = btn.classList.contains("answer-from-income")
				? document.querySelector(".answer-from-amount")
				: document.querySelector(".answer-from-income");
			if (anothertBtn.classList.contains("pressed-btn")) {
				anothertBtn.classList.remove("pressed-btn");
			}

			if (!btn.classList.contains("pressed-btn")) {
				btn.classList.add("pressed-btn");
				this.isFromIncome = !this.isFromIncome;
				this.isFromSpecificAmount = !this.isFromSpecificAmount;
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
		getMoneyAfterPeriod(
			period,
			moneyToSave,
			investments,
			saveAndInvestMoney
		) {
			const percent = parseInt(investments.percent) / 100;

			let justSavedMoney = Math.floor(moneyToSave * period * 12);
			let investmentSavings = Math.floor(
				parseInt(investments.amount) * percent
			);
			let saveAndInvest = Math.floor(
				(saveAndInvestMoney + moneyToSave * 12) * percent
			);

			return { justSavedMoney, investmentSavings, saveAndInvest };
		},
		calculate() {
			let moneyToSave = parseInt(this.inputModels.moneyToSave);
			let investments = this.inputModels.usersInvestments;
			let currentInvestmentsSavings = parseInt(investments.amount);
			let currentSaveAndInvestMoney = currentInvestmentsSavings;
			let currentMoneySavings = 0;

			for (let year = 1; year <= this.maxPeriod; year++) {
				let usersSavings = this.getMoneyAfterPeriod(
					year,
					moneyToSave,
					{
						amount: currentInvestmentsSavings,
						percent: investments.percent,
					},
					currentSaveAndInvestMoney
				);

				currentInvestmentsSavings += usersSavings.investmentSavings;

				currentMoneySavings = usersSavings.justSavedMoney;

				currentSaveAndInvestMoney +=
					usersSavings.saveAndInvest + moneyToSave * 12;
			}
			const output = {
				currentMoneySavings,
				currentInvestmentsSavings,
				currentSaveAndInvestMoney,
			};
			this.usersSavings = output;
			return output;
		},
	},
	watch: {
		"inputModels.usersMoney"(value) {
			this.watchInput(value, 1);
		},
		"inputModels.usersExpenditures"(value) {
			this.watchInput(value, 2);
		},
		"inputModels.moneyToSave"(value) {
			this.watchInput(value, 3);
		},
		"inputModels.usersInvestments.amount"(value) {
			this.watchInput(value, 4);
		},
		"inputModels.usersInvestments.percent"(value) {
			this.watchInput(value, 5);
		},
	},
	mounted() {
		const calculateBtn = document.querySelector(".calculate-btn");
		calculateBtn.addEventListener("mouseenter", () => {
			calculateBtn.classList.add("calculate-hover");
		});
		calculateBtn.addEventListener("mouseleave", () => {
			calculateBtn.classList.remove("calculate-hover");
		});
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
		padding: 160px 0 0 0;
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
			.answer-block-4 {
				display: flex;
				justify-content: flex-start;
				align-items: center;
				column-gap: 50px;
			}
		}

		.calculate-btn {
			text-transform: uppercase;
			color: #000;
			display: inline-block;
			text-align: center;
			padding: 10px 20px;
			margin: 20px 0 0 0;
			font-size: 25px;
			font-weight: light;
			cursor: pointer;
			outline: none;
			border: 1px solid transparent;
			background-color: $light-blue;
			user-select: none;
			transition: all 0.3s ease;
		}
		.calculate-btn.calculate-hover {
			border: 1px solid $yellow-color;
			transform: translateX(10px);
		}
		.calculate-btn.calculate-disabled {
			opacity: 0.8;
			cursor: default;
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
	.answer-section-1 {
		margin: 0 0 0 38px;
	}
	.warning-title {
		padding: 10px;
		background-color: rgba(#4dad45, 0.4);
	}
}
</style>