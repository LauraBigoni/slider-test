<template>
	<div
		class="
			input-container
			flex
			justify-end
			md:gap-10
			grow
			flex-col
			md:flex-row
			items-center
		"
	>
		<div
			class="
				mix-blend-difference
				text-gray-100
				flex
				items-center
				basis-1/2
				md:py-3
				py-5
			"
		>
			<!-- Input range per il num delle card -->
			<label for="cards-range">Number of cards:</label>
			<input
				type="range"
				min="0"
				max="42"
				step="1"
				name="cards-range"
				id="cards-range"
				class="mx-3"
				v-model.number="value"
				@input="passValue()"
			/>
			<!-- Passo il valore a slider page con un event emitter  -->

			<output v-text="value"></output>
		</div>

		<div class="flex items-center basis-1/2 md:py-3">
			<!-- Input color per cambiare il colore del background -->
			<label for="color-picker" class="pr-4 mix-blend-difference text-gray-100"
				>Background color:</label
			>
			<input
				type="color"
				name="color-picker"
				id="color-picker"
				value="#F4EAE1"
				@input="setColor()"
			/>
      <!-- Passo il valore a slider page con un event emitter  -->

		</div>
	</div>
</template>

<script>
export default {
	name: "InputElements",
	data() {
		return {
			value: 3,
		};
	},
	methods: {
    // Creo una funzione per cambiare il colore del background che verrà ascoltata dal parent
		setColor() {
			let colorPicker = document.getElementById("color-picker");
			colorPicker.style.setProperty("--color", colorPicker.value);
			this.$emit("backgroundChange", colorPicker.value);
		},
    // Creo una funzione per cambiare il value del range al parent
		passValue() {
			this.$emit("valueChange", this.value);
		},
	},
};
</script>

<style lang="scss" scoped>
.input-container {
	#color-picker {
		padding: 0;
		--color: #ffffff;
		width: 30px;
		height: 14px;
		position: relative;
		cursor: crosshair;
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
		color: #fff;
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
</style>