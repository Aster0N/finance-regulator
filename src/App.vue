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
							<input class="answer" type="text" />
						</div>
					</div>
					<div class="question">
						<div class="question-block">
							<span class="question-number">2.</span>&nbsp;
							<span class="question-title"
								>Enter your monetary expenditures</span
							>
						</div>
						<div class="answer-block">
							<input class="answer" type="text" />
						</div>
					</div>
					<div class="question">
						<div class="question-block">
							<span class="question-number">3.</span>&nbsp;
							<span class="question-title"
								>Money that you can save each month</span
							>
						</div>
						<div class="answer-block">
							<input class="answer" type="text" />
						</div>
					</div>
					<div class="question">
						<div class="question-block">
							<span class="question-number">4.</span>&nbsp;
							<span class="question-title"
								>If you are willing to deposit in a bank with
								interest, please enter the amount and %</span
							>
						</div>
						<div class="answer-block">
							<input class="answer" type="text" />
						</div>
					</div>

					<div class="control-buttons">
						<button
							@click="goToPrevQuestion"
							class="control-btn prev-btn"
						>
							prev
						</button>
						<button
							@click="goToNextQuestion"
							class="control-btn next-btn"
						>
							next
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
			questionsNumber: 0,
			currentQuestionIndex: 0,
			questionDisabledClass: "question-disabled",
			questonActiveClass: "question-active",
		};
	},
	methods: {
		clearQuestionClasses() {
			const questions = document.querySelectorAll(".question");

			questions.forEach((question) => {
				if (question.classList.contains(this.questionDisabledClass)) {
					question.classList.remove(this.questionDisabledClass);
				} else {
					question.classList.remove(this.questonActiveClass);
				}
			});
		},
		setQuestionClasses() {
			const questions = document.querySelectorAll(".question");

			for (let i = 0; i < questions.length; i++) {
				if (i !== this.currentQuestionIndex) {
					questions[i].classList.add(this.questionDisabledClass);
				} else {
					questions[i].classList.add(this.questonActiveClass);
				}
			}
		},
		goToPrevQuestion() {
			this.clearQuestionClasses();

			this.currentQuestionIndex =
				this.currentQuestionIndex - 1 === -1
					? this.questionsNumber - 1
					: this.currentQuestionIndex - 1;

			this.setQuestionClasses();
		},
		goToNextQuestion() {
			this.clearQuestionClasses();

			this.currentQuestionIndex =
				this.currentQuestionIndex + 1 === this.questionsNumber
					? 0
					: this.currentQuestionIndex + 1;

			this.setQuestionClasses();
		},
	},
	mounted() {
		const questions = document.querySelectorAll(".question");
		this.questionsNumber = questions.length;
		this.setQuestionClasses();
	},
};
</script>

<style lang="scss">
@import "./assets/scss/style.scss";
@import "./assets/scss/variables.scss";

.welcome-page {
	height: 100vh;
	background-color: $secondary-color;
	.workflow-body {
		height: 100%;
		width: 100%;
		background-color: $secondary-color;
	}
	.page-title {
		width: 100%;
		padding: 150px 0 0 0;
		text-align: center;
		font-family: "Montserrat", serif;
		font-size: 40px;
		font-weight: regular;
		text-transform: uppercase;
		letter-spacing: 30px;
	}
}

.regulator-core {
	height: 100vh;
	background-color: $primary-color;
	font-family: "Palanquin", sans-serif;
	font-size: 20px;
	color: #fff;

	.question {
		font-weight: bold;
	}
}

.question-disabled {
	display: none;
}
</style>