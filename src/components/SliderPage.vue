<template>
	<div class="slider h-screen flex flex-col justify-center">
		<div class="w-full flex justify-between items-center">
			<h1 class="text-4xl pl-20 font-bold grow text-center">Section Cards</h1>
			<div class="input-container flex justify-end gap-20 grow">
				<div class="flex items-center basis-1/2 py-3">
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
		<div class="slider-cards flex flex-row gap-14 items-center">
			<SliderCard
				class="ease-in duration-300 card shrink-0"
				v-for="(item, index) in value"
				:key="index"
				:class="{ active: activeCards(index) }"
				:item="item"
			/>
		</div>
		<div class="navigation-slider">
			<i
				@click="prevCard()"
				class="fa-solid fa-arrow-left prev ease-in duration-300 disabled"
			></i>
			<i
				@click="nextCard()"
				class="fa-solid fa-arrow-right next ease-in duration-300"
			></i>
			<div class="dots text-center">
				<i
					v-for="(icon, index) in value"
					:key="index"
					class="fa-solid fa-circle"
					:class="{ active: activeCards(index) }"
					@click="setCards(index)"
				></i>
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
			value: 15,
			currentIndex: 0,
		};
	},
	computed: {},
	methods: {
		setColor() {
			let colorPicker = document.getElementById("color-picker");
			colorPicker.style.setProperty("--color", colorPicker.value);
			this.$emit("backgroundChange", colorPicker.value);
		},
		activeCards(index) {
			return this.currentIndex === index;
		},
		setCards(index) {
			const cards = document.querySelector(".slider-cards");
			this.currentIndex = index;

			if (this.currentIndex < 2) {
				cards.scrollLeft = index;
			} else cards.scrollLeft = index * 320;
		},
		prevCard() {
			const cards = document.querySelector(".slider-cards");
			const lastCard = document.querySelector(
				`.slider-cards div:nth-child(${this.currentIndex + 3}n)`
			);
			const prevCard = document.querySelector(
				`.slider-cards div:nth-child(${this.currentIndex + 1}n)`
			);

			if (this.currentIndex == 0) {
				return;
			} else {
				this.currentIndex--;

				if (this.currentIndex < this.value - 3) {
					cards.scrollLeft -= 600;
				}

				if (this.currentIndex > 0) {
					lastCard.style.opacity = "0.5";
					prevCard.style.opacity = "1";
					console.log(lastCard.style.opacity);
				}
			}
		},
		nextCard() {
			const cards = document.querySelector(".slider-cards");
			const lastCard = document.querySelector(
				`.slider-cards div:nth-child(${this.currentIndex + 4}n)`
			);
			const prevCard = document.querySelector(
				`.slider-cards div:nth-child(${this.currentIndex + 3}n)`
			);

			if (this.currentIndex == this.value - 1) {
				return;
			} else {
				this.currentIndex++;
				if (this.currentIndex > 2) {
					cards.scrollLeft += 400;
				}

				if (this.currentIndex < this.value - 2) {
					lastCard.style.opacity = "0.5";
					prevCard.style.opacity = "1";
					console.log(lastCard.style.opacity);
				}
			}
		},
	},
	mounted() {},
};
</script>

<style lang="scss" scoped>
.slider {
	width: 100vw;

	.input-container {
		#color-picker {
			padding: 0;
			--color: #f4eae1;
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
			--clip-top: calc(
				(var(--thumb-height) - var(--track-height)) * 0.5 - 0.5px
			);
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
			background: linear-gradient(var(--track-color) 0 0) scroll no-repeat
				center / 100% calc(var(--track-height) + 1px);
		}
	}

	.slider-cards {
		overflow: hidden;
		height: 550px;
		margin-right: -70px;
		scroll-behavior: smooth;

		& > div {
			height: 400px;
			width: 300px;

			&.active {
				transform: scale(1.2);
			}
		}
	}

	.navigation-slider {
		.fa-arrow-left {
			font-size: 40px;
			position: absolute;
			top: 50%;
			left: 6%;
			cursor: pointer;

			&:hover {
				transform: translateX(-10px);
			}
		}

		.fa-arrow-right {
			font-size: 40px;
			position: absolute;
			top: 50%;
			right: 6%;
			cursor: pointer;

			&:hover {
				transform: translateX(10px);
			}
		}

		.dots {
			padding-top: 50px;

			i {
				margin: 0 10px;
				cursor: pointer;
				color: #b3b3b39d;

				&.active {
					color: #000;
					transform: scale(1.3);
				}

				&:hover {
					transform: scale(1.3);
				}
			}
		}
	}
}
</style>