<script lang="ts">
	import type { JednoPexesoOptions } from 'src/ts/jednoPexesoOptions';
	import { scale } from 'svelte/transition';

	export let options: JednoPexesoOptions;
	export let animationDelay: number = 0;
</script>

<div in:scale={{ delay: animationDelay }} class="relative">
	<div
		on:click
		class="w-20 h-20 rounded-lg transition-all duration-500 relative bg-black backface-hidden"
		class:rotated={options.isOpen || options.isFound}
		style="--barvicka: {options.color}"
	/>

	{#if options.isFound}
		<svg
			class="h-10 w-10 absolute top-0 bottom-0 m-auto left-0 right-0"
			fill="none"
			viewBox="0 0 24 24"
			stroke="currentColor"
			in:scale={{ delay: 500 }}
		>
			<path
				stroke-linecap="round"
				stroke-linejoin="round"
				stroke-width="2"
				d="M9 12l2 2 4-4m6 2a9 9 0 11-18 0 9 9 0 0118 0z"
			/>
		</svg>
	{/if}
</div>

<style>
	div > div:not(.rotated) {
		transform: rotateX(180deg);
	}

	div > div {
		transform-style: preserve-3d;
	}

	div > div::after {
		content: '';
		@apply absolute w-full h-full top-0 left-0 rounded-lg;
		backface-visibility: hidden;
		transform: rotateX(0deg);
		transform-style: preserve-3d;
		background-color: var(--barvicka);
	}
</style>
