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
			<!-- Titolo sezione  -->
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
			<!-- Importo il componente con gli input che ascolta gli event emitter per il cambio del range e numero cards -->
			<InputElements
				@backgroundChange="changeBgColor"
				@valueChange="changeValue"
			/>
		</div>
		<div class="flex justify-center basis-3/5">
			<div class="slider-cards flex flex-row items-center justify-start">
				<!-- Ciclo le cards per il valore ricevuto dall'input value -->
				<SliderCard
					class="w-60 h-80 ease-in duration-300 card shrink-0"
					v-for="(item, index) in value"
					:key="index"
					:class="{ active: activeCards(index) }"
					:item="item"
				/>
			</div>
		</div>
		<!-- Frecce e dots per navigazione -->
		<div class="navigation-slider basis-basis-1/5">
			<!-- al click viene : 
		1. disabilitata la freccia se l'index è 0 
		2. mostrata la card precedente se l'index è > 0 -->
			<i
				@click="prevCard()"
				class="
					invisible
					lg:visible
					fa-solid fa-arrow-left
					prev
					ease-in
					duration-300
					mix-blend-difference
					text-gray-100
				"
				:class="{ 'opacity-50 transition-none': currentIndex == 0 }"
			></i>
			<!-- al click viene : 
		1. disabilitata la freccia se l'index è === al value -1
		2. mostrata la card successiva se l'index è < del value -->
			<i
				@click="nextCard()"
				class="
					invisible
					lg:visible
					fa-solid fa-arrow-right
					next
					ease-in
					duration-300
					mix-blend-difference
					text-gray-100
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
				<!-- Ciclo i dots per il valore ricevuto dall'input value
			Passo l'index alle funzioni per dire che index di ogni 
			puntino sia uguale all'index della card -->
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
import InputElements from "@/components/InputElements";

export default {
	name: "SliderPage",
	components: { SliderCard, InputElements },

	data() {
		return {
			value: 3,
			currentIndex: 0,
		};
	},
	computed: {},
	methods: {
		// Emitto ad APP il valore del background ricevuto dal componente InputElements
		changeBgColor(value) {
			this.$emit("backgroundChange", value);
		},
		// Ricevo il valore del range ricevuto dal componente InputElements
		changeValue(value) {
			this.value = value;
		},
		// Creo una funzione per tenere traccia del current index
		activeCards(index) {
			return this.currentIndex === index;
		},
		// Creo una funzione per gestire i dots
		setCards(index) {
			// Prendo dal dom il contenitore delle card
			const cards = document.querySelector(".slider-cards");
			// Utilizzo una media query da utilizzare in mobile version
			const mediaQuery = window.matchMedia("(min-width: 640px)");

			// Recupero l'index
			this.currentIndex = index;

			// Per versione mobile scorro i dots in modo differente
			if (mediaQuery.matches) {
				if (this.currentIndex < 2) {
					// Non scrollo i primi elementi se la mediaquery è superiore a 640px
					cards.scrollLeft = index;
				} else {
					// Altrimenti in versione mobile li scrollo
					cards.scrollLeft = index * 300;
				}
			} else {
				// Per versione mobile, scrollo ad ogni elemento
				cards.scrollLeft = index * 300;
			}
		},
		// Creo una funzione per far funzionare la freccia left
		prevCard() {
			// Prendo il contenitore delle cards
			const cards = document.querySelector(".slider-cards");

			// Prendo la last card per creare un effetto opaco a destra del contenitore
			const lastCard = document.querySelector(
				`.slider-cards div:nth-child(${this.currentIndex + 2}n)`
			);
			// E ad ogni click riporto l'opacità ad 1
			const prevCard = document.querySelector(
				`.slider-cards div:nth-child(${this.currentIndex + 1}n)`
			);

			// Se sono finite le card non fa niente
			if (this.currentIndex == 0) {
				return;
			} else {
				// Altrimenti diminuisce l'index ad ogni click
				this.currentIndex--;

				// Se il currentIndex è minore del valore delle card -3
				if (this.currentIndex < this.value - 3) {
					// Uso scrollLeft per creare un effetto di slider sul contenitore
					cards.scrollLeft -= 350;
				}

				// Se il currentIndex è maggiore di 0 setto l'opacità all'ultima card a destra
				// E rimuovo l'opacità da quella immediatamente prima
				if (this.currentIndex > 0) {
					lastCard.style.opacity = "0.5";
					prevCard.style.opacity = "1";
				}
			}
		},
		// Creo una funzione per far funzionare la freccia right
		nextCard() {
			// Prendo il contenitore delle cards
			const cards = document.querySelector(".slider-cards");
			// Prendo la last card per creare un effetto opaco a destra del contenitore
			const lastCard = document.querySelector(
				`.slider-cards div:nth-child(${this.currentIndex + 4}n)`
			);
			// E ad ogni click riporto l'opacità ad 1
			const prevCard = document.querySelector(
				`.slider-cards div:nth-child(${this.currentIndex + 3}n)`
			);

			// Se sono finite le card non fa niente
			if (this.currentIndex == this.value - 1) {
				return;
			} else {
				// Altrimenti aumenta l'index ad ogni click
				this.currentIndex++;

				// Se il currentIndex è maggiore di 2
				if (this.currentIndex > 2) {
					// Uso scrollLeft per creare un effetto di slider sul contenitore
					cards.scrollLeft += 350;
				}

				// Se il currentIndex è minore del value -1, setto l'opacità all'ultima card a destra
				// E rimuovo l'opacità da quella immediatamente prima
				if (this.currentIndex < this.value - 1) {
					lastCard.style.opacity = "0.5";
					prevCard.style.opacity = "1";
				}
			}
		},
	},
};
</script>

<style lang="scss" scoped>
.slider {
	width: 90vw;
	height: 100vh;
	margin: 0 auto;
	touch-action: auto;

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