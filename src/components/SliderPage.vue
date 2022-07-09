<template>
	<div class="slider flex flex-col justify-center">
		<div
			class="
				flex
				md:flex-row
				flex-col
				justify-between
				items-center
				basis-basis-1/5
			"
		>
			<h1
				class="
					mix-blend-difference
					text-gray-100 text-4xl
					font-bold
					grow
					text-center
					pt-2
				"
			>
				Section Cards
			</h1>
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

				<div class="flex items-center basis-1/2 md:py-3">
					<label
						for="color-picker"
						class="pr-4 mix-blend-difference text-gray-100"
						>Background color:</label
					>
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
		<div class="flex justify-center basis-3/5">
			<div class="slider-cards flex flex-row items-center justify-start">
				<SliderCard
					class="w-60 h-80 ease-in duration-300 card shrink-0"
					v-for="(item, index) in value"
					:key="index"
					:class="{ active: activeCards(index) }"
					:item="item"
				/>
			</div>
		</div>
		<div class="navigation-slider basis-basis-1/5">
			<i
				@click="prevCard()"
				class="
					invisible
					lg:visible
					fa-solid fa-arrow-left
					prev
					ease-in
					duration-300
				"
				:class="{ 'opacity-50 transition-none': currentIndex == 0 }"
			></i>
			<i
				@click="nextCard()"
				class="
					invisible
					lg:visible
					fa-solid fa-arrow-right
					next
					ease-in
					duration-300
				"
				:class="{
					'opacity-50 transition-none': currentIndex == this.value - 1,
				}"
			></i>
			<div
				class="
					dots
					flex
					justify-center
					flex-wrap
					gap-2
					px-5
					pb-2
					md:pb-0
					mix-blend-difference
					text-gray-100
				"
			>
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
			value: 3,
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
			const mediaQuery = window.matchMedia("(min-width: 640px)");
			this.currentIndex = index;

			if (mediaQuery.matches) {
				if (this.currentIndex < 2) {
					cards.scrollLeft = index;
				} else {
					cards.scrollLeft = index * 300;
				}
			} else {
				cards.scrollLeft = index * 300;
			}
		},
		prevCard() {
			const cards = document.querySelector(".slider-cards");
			const lastCard = document.querySelector(
				`.slider-cards div:nth-child(${this.currentIndex + 2}n)`
			);
			const prevCard = document.querySelector(
				`.slider-cards div:nth-child(${this.currentIndex + 1}n)`
			);

			if (this.currentIndex == 0) {
				return;
			} else {
				this.currentIndex--;

				if (this.currentIndex < this.value - 3) {
					cards.scrollLeft -= 350;
				}

				if (this.currentIndex > 0) {
					lastCard.style.opacity = "0.5";
					prevCard.style.opacity = "1";
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
					cards.scrollLeft += 350;
				}

				if (this.currentIndex < this.value - 1) {
					lastCard.style.opacity = "0.5";
					prevCard.style.opacity = "1";
				}
			}
		},
	},
	mounted() {},
};
</script>

<style lang="scss" scoped>
.slider {
	width: 90vw;
	height: 100vh;
	margin: 0 auto;
	touch-action: auto;

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
		margin: 0;
		scroll-behavior: smooth;

		& > div.active {
			transform: scale(1.1);
			opacity: 1 !important;
		}
	}

	.navigation-slider {
		.fa-arrow-left {
			font-size: 40px;
			position: absolute;
			top: 50%;
			left: 6%;
			cursor: pointer;

			&:not(.transition-none):hover {
				transform: translateX(-10px);
			}
		}

		.fa-arrow-right {
			font-size: 40px;
			position: absolute;
			top: 50%;
			right: 6%;
			cursor: pointer;

			&:not(.transition-none):hover {
				transform: translateX(10px);
			}
		}

		.dots {
			padding-top: 50px;

			i {
				cursor: pointer;
				color: #b3b3b39d;

				&.active {
					color: #fff;
					transform: scale(1.2);
					transition: transform 0.5s;
				}

				&:hover {
					transform: scale(1.2);
					transition: transform 0.5s;
				}

				&:active {
					transform: scale(0.8);
					transition: transform 0.5s;
				}
			}
		}
	}
}
</style>