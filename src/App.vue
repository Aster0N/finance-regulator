<template>
	<div id="app">
		<div class="page-title">finance regulator</div>

		<main>
			<div class="container">
				<div class="workflow">
					<div class="workflow-user-input"></div>
					<div class="workflow-output"></div>
				</div>
				<div class="regulator-core">
					<form class="finance-regulator-form" action="">
						<label class="form-label">income:</label>
						<input type="text" class="form-field" />
					</form>
					<div class="regulator-output"></div>
				</div>
			</div>
		</main>
	</div>
</template>

<script>
export default {
	name: "App",
};
</script>

<style lang="scss" scoped>
@import url("./assets/scss/style.scss");
@import url("./assets/scss/variables.scss");
// font-family: 'Palanquin', sans-serif;
// font-family: 'Playfair Display SC', serif;
main {
	font-family: "Palanquin", sans-serif;
	font-size: 20px;
	font-weight: regular;
}
.page-title {
	width: 100%;
	padding: 150px 0 0 0;
	text-align: center;
	font-family: "Playfair Display SC", serif;
	font-size: 40px;
	font-weight: regular;
	text-transform: uppercase;
	letter-spacing: 25px;
}
</style>