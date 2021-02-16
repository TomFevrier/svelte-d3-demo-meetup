<script>
	import { scaleTime, scaleLinear } from 'd3-scale';
	import { extent, max } from 'd3-array';
	import { line } from 'd3-shape';
	import { timeFormat } from 'd3-time-format';

	import { draw } from 'svelte/transition';

	import Axis from './Axis.svelte';

	export let data;

	let width;

	const height = 600;
	const margin = {
		top: 40,
		right: 40,
		bottom: 40,
		left: 40
	};

	$: xScale = scaleTime()
		.domain(extent(data, d => new Date(d.date)))
		.range([margin.left, width - margin.right]);

	$: yScale = scaleLinear()
		.domain([0, max(data, d => +d.value)])
		.range([height - margin.bottom, margin.top]);

	$: lineGenerator = line()
		.x(d => xScale(new Date(d.date)))
		.y(d => yScale(+d.value));
</script>

<div class='chart' bind:clientWidth={width}>
	{#if width}
		<svg {width} {height}>
			<Axis position='bottom' scale={xScale} {width} {height} {margin} tickFormat={timeFormat('%m/%y')} />
			<Axis position='left' scale={yScale} {width} {height} {margin} />
			<path d={lineGenerator(data)} in:draw={{ duration: 3000 }} />
		</svg>
	{/if}
</div>

<style lang='scss'>
	@import './global.scss';

	path {
		stroke: $red;
		stroke-width: 2px;
		fill: none;
	}
</style>
