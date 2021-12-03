<script lang="ts">
	import GridContainer from '$lib/PexesoContainer.svelte';
	import JednoPexeso from '$lib/JednoPexeso.svelte';
	import type { JednoPexesoOptions } from '../ts/jednoPexesoOptions';
	import randomColor from 'randomcolor';
	import { shuffle } from 'lodash-es';

	let side = 4;
	let uspesne = 0;
	let neuspesne = 0;

	function reset() {
		pexesos = getPexesos(side);
		uspesne = 0;
		neuspesne = 0;
	}

	function getPexesos(side: number) {
		const pulBarev = Array((side * side) / 2)
			.fill(0)
			.map(() => randomColor());

		const barvicky = pulBarev.concat(pulBarev);

		const arr = Array(side * side)
			.fill(0)
			.map(
				(_, i) =>
					({ id: i, isFound: false, isOpen: false, color: barvicky.pop() } as JednoPexesoOptions)
			);

		return shuffle(arr);
	}

	function otocJednoPexeso(options) {
		options.isOpen = true;
		pexesos = pexesos;

		const openPexesos = pexesos.filter((p) => p.isOpen);
		const openNotFoundPexesos = openPexesos.filter((p) => !p.isFound);

		if (openNotFoundPexesos.length === 2) {
			if (openNotFoundPexesos[0].color === openNotFoundPexesos[1].color) {
				openNotFoundPexesos.forEach((p) => (p.isFound = true));
				uspesne++;

				if (pexesos.filter((p) => !p.isFound).length === 0) {
					setTimeout(
						() =>
							alert(`Vyhrál jsi!\nPočet uspesnych: ${uspesne}\nPočet neuspesnych: ${neuspesne}`),
						1000
					);
				}
			} else {
				setTimeout(() => {
					openNotFoundPexesos.forEach((p) => (p.isOpen = false));
					pexesos = pexesos;
				}, 1000);
				neuspesne++;
			}
		}
	}

	let pexesos = [];

	$: {
		side;
		pexesos = [];
	}
</script>

<div class="flex px-32 py-8 gap-x-8 items-center">
	<div class="text-2xl font-bold">Pekekwekeso</div>

	<button class="px-4 py-2 font-bold bg-purple-500 text-white rounded" on:click={reset}>
		Reset
	</button>

	<label for="side-size" class="form-label whitespace-nowrap font-bold"
		>Side size: <span class="text-blue-500">{side}</span></label
	>
	<input
		type="range"
		class="form-range"
		id="side-size"
		bind:value={side}
		min="2"
		max="10"
		step="2"
	/>
</div>

<GridContainer cols={side}>
	{#each pexesos as options, i}
		<JednoPexeso
			{options}
			on:click={() => otocJednoPexeso(options)}
			animationDelay={(i * 1000) / (side * side)}
		/>
	{/each}
</GridContainer>

<div class="grid grid-cols-2 font-semibold max-w-sm mx-32 mt-4">
	<div>Uspesne pokusy:</div>
	<div class="text-green-500">{uspesne}</div>

	<div>Neuspesne pokusy:</div>
	<div class="text-red-500">{neuspesne}</div>
</div>
