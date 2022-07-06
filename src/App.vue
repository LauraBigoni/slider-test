<template>
	<div id="app">
		<h1 class="text-3xl text-center py-20 text-sky-500">Slider Cards</h1>
		<SliderPage />
	</div>
</template>

<script>
import SliderPage from "@/components/SliderPage";

export default {
	name: "App",
	components: { SliderPage },
};
</script>

<style lang="scss">
@import "./assets/sass/style.scss";
</style>
