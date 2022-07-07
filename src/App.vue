<template>
	<div id="app">
		<SliderPage @backgroundChange="changeBgColor" />
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
