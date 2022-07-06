<template>
	<div id="app">
		<div class="container mx-auto px-15">
			<SliderPage @backgroundChange="changeBgColor" />
		</div>
	</div>
</template>

<script>
import SliderPage from "@/components/SliderPage";

export default {
	name: "App",
	components: { SliderPage },
	methods: {
		changeBgColor(value) {
			let newBackgroundColor = document.getElementById("app");
			newBackgroundColor.style.setProperty("background-color", value);
		},
	},
};
</script>

<style lang="scss">
@import "./assets/sass/style.scss";
#app {
	background-color: #f4eae1;
}
</style>
