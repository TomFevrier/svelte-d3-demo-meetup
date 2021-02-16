<script>
	import { fade } from 'svelte/transition';

	export let width;
	export let height;
	export let margin;
	export let scale;
	export let position = 'left';
	export let tickGap = 80;
	export let tickFormat = _ => _;

	$: ticks = scale.ticks(Math.max(1, ~~(width / tickGap)))
		.map(value => ({
			value,
			offset: scale(value)
		}))
		.filter((e, i, array) => {
			if (!(e.value instanceof Date)) return e;
			return e.value.getHours() === 0 && array.slice(0, i).every(d => tickFormat(d.value) !== tickFormat(e.value));
		});
</script>

<g class='axis'>
	{#each ticks as tick(tick.value)}
		{#if position === 'left'}
			<g class='tick' transform='translate({margin.left}, {tick.offset})' transition:fade>
				<line x2={-6} />
				<text x={-10} y={5} font-size={14} text-anchor='end'>
					{@html tickFormat(tick.value)}
				</text>
			</g>
		{:else if position === 'top'}
			<g class='tick' transform='translate({tick.offset}, {margin.top})' transition:fade>
				<line y2={-6} />
				<text y={-12} font-size={14} text-anchor='middle'>
					{@html tickFormat(tick.value)}
				</text>
			</g>
		{:else if position === 'bottom'}
			<g class='tick' transform='translate({tick.offset}, {height - margin.bottom})' transition:fade>
				<line y2={6} />
				<text y={18} font-size={14} text-anchor='middle'>
					{@html tickFormat(tick.value)}
				</text>
			</g>
		{/if}
	{/each}
</g>

<style>
	line {
		stroke: #555555;
	}

	text {
		fill: #555555;
		font-family: 'Source Sans Pro', sans-serif;
	}
</style>
