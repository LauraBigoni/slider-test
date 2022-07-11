<template>
	<!-- Riceve il cambio del background color dal componente SliderPage che a sua volta lo riceve da InputElements -->
	<div id="app" :style="`backgroundColor: ${bgColor}`">
		<SliderPage @backgroundChange="changeBgColor" />
	</div>
</template>

<script>
import SliderPage from "@/components/SliderPage";

export default {
	name: "App",
	components: { SliderPage },
	data() {
		return {
			bgColor: "#F4EAE1",
		};
	},
	methods: {
		changeBgColor(value) {
			this.bgColor = value;
		},
	},
};
</script>

<style lang="scss">
@import "./assets/sass/style.scss";
</style>
