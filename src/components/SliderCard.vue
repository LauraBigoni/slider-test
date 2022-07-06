<template>
	<div
		class="
			w-1/4
			p-6
			rounded-3xl
			shadow-lg
			bg-white
			flex flex-col
			justify-center
			items-center
			text-center
		"
	>
		<img
			class="mb-3 w-24 h-24 rounded-full shadow-lg"
			src="https://api.lorem.space/image/fashion?w=150&h=150"
			alt="Bonnie image"
		/>
		<h5 class="text-gray-900 text-xl leading-tight font-medium mb-2">
			A really very long long long card title to test
		</h5>
		<p class="text-gray-500 text-base mb-4">
			Some quick example text to build on the card title and make up the bulk of
			the card's content.
		</p>
		<button
			class="
				py-2
				px-4
				text-sm
				font-medium
				rounded-lg
				hover:bg-red-700
				focus:bg-red-300
				active:bg-red-600
				bg-red-500
				text-white
				ease-in
				duration-300
			"
		>
			Button
		</button>
	</div>
</template>

<script>
export default {
	name: "SliderCard",
};
</script>