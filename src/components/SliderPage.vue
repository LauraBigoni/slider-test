<template>
	<div class="slider h-screen flex flex-col justify-center">
		<div class="pb-10 w-full flex justify-between">
			<h1 class="text-4xl pb-10 pl-20 font-bold">Section Cards</h1>
			<div class="input-container flex justify-end gap-20">
				<div class="flex items-center basis-1/2 py-3">
					<label for="cards-range">Number of cards:</label>
					<input
						type="range"
						min="0"
						max="15"
						step="1"
						name="cards-range"
						id="cards-range"
						class="mx-3"
						v-model.number="value"
					/>
					<output v-text="value"></output>
				</div>

				<div class="flex items-center basis-1/2 py-3">
					<label for="color-picker" class="pr-4">Background color:</label>
					<input
						type="color"
						name="color-picker"
						id="color-picker"
						value="#F4EAE1"
						@input="setColor()"
					/>
				</div>
			</div>
		</div>
		<div class="slider-card flex flex-row gap-14 justify-center">
			<SliderCard v-for="index in value" :key="index" />
		</div>
		<div class="navigation-slider">
			<i class="fa-solid fa-arrow-left prev"></i>
			<i class="fa-solid fa-arrow-right next"></i>
			<div class="dots text-center">
				<i v-for="i in 4" :key="i" class="fa-solid fa-circle"></i>
			</div>
		</div>
	</div>
</template>

<script>
import SliderCard from "@/components/SliderCard";

export default {
	name: "SliderPage",
	components: { SliderCard },
	data() {
		return {
			value: 3,
		};
	},
	computed: {},
	methods: {
		setColor() {
			let colorPicker = document.getElementById("color-picker");
			colorPicker.style.setProperty("--color", colorPicker.value);
			this.$emit("backgroundChange", colorPicker.value);
		},
	},
};
</script>

<style lang="scss" scoped>
.input-container {
	#color-picker {
		padding: 0;
		--color: #f4eae1;
		width: 30px;
		height: 14px;
		position: relative;
	}

	#color-picker::before {
		content: "";
		position: absolute;
		top: 50%;
		left: 50%;
		transform: translate(-50%, -50%);
		pointer-events: none;
		width: 40px;
		height: 24px;
		background: var(--color);
		border-radius: 20px;
		border: 2px solid rgba(255, 255, 255, 0.7);
	}

	input[type="range"] {
		position: relative;
		background: transparent;
		overflow: hidden;
		color: #000;
		--thumb-height: 0.725em;
		--track-height: 0.125em;
		--track-color: rgba(0, 0, 0, 0.2);
		--brightness-hover: 180%;
		--brightness-down: 80%;
		--clip-edges: 0.125em;
	}

	input[type="range"],
	input[type="range"]::-webkit-slider-runnable-track,
	input[type="range"]::-webkit-slider-thumb {
		-webkit-appearance: none;
		height: var(--thumb-height);
	}

	input[type="range"]::-webkit-slider-thumb {
		--thumb-radius: calc((var(--thumb-height) * 0.5) - 1px);
		--clip-top: calc((var(--thumb-height) - var(--track-height)) * 0.5 - 0.5px);
		--clip-bottom: calc(var(--thumb-height) - var(--clip-top));
		--clip-further: calc(100% + 1px);
		--box-fill: calc(-100vmax - var(--thumb-width, var(--thumb-height))) 0 0
			100vmax currentColor;

		width: var(--thumb-width, var(--thumb-height));
		background: linear-gradient(currentColor 0 0) scroll no-repeat left center /
			50% calc(var(--track-height) + 1px);
		background-color: currentColor;
		box-shadow: var(--box-fill);
		border-radius: var(--thumb-width, var(--thumb-height));

		filter: brightness(100%);
		clip-path: polygon(
			100% -1px,
			var(--clip-edges) -1px,
			0 var(--clip-top),
			-100vmax var(--clip-top),
			-100vmax var(--clip-bottom),
			0 var(--clip-bottom),
			var(--clip-edges) 100%,
			var(--clip-further) var(--clip-further)
		);
	}

	input[type="range"]:hover::-webkit-slider-thumb {
		filter: brightness(var(--brightness-hover));
		cursor: grab;
	}

	input[type="range"]:active::-webkit-slider-thumb {
		filter: brightness(var(--brightness-down));
		cursor: grabbing;
	}

	input[type="range"]::-webkit-slider-runnable-track {
		background: linear-gradient(var(--track-color) 0 0) scroll no-repeat center /
			100% calc(var(--track-height) + 1px);
	}
}
.navigation-slider {
	.fa-arrow-left {
		font-size: 40px;
		position: absolute;
		top: 50%;
		left: 8%;
	}
	.fa-arrow-right {
		font-size: 40px;
		position: absolute;
		top: 50%;
		right: 8%;
	}
	.dots {
		padding-top: 100px;
		i {
			margin: 0 10px;
		}
	}
}
</style>