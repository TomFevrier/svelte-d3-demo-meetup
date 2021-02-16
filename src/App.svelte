<script>
	import { onMount } from 'svelte';

	import { LoadingCache } from '@echosgraphiques/ui-components/layout';
	import { Header } from '@echosgraphiques/ui-components/sections';
	import { Title, Chapo, Signature } from '@echosgraphiques/ui-components/titraille';
	import { Logo } from '@echosgraphiques/ui-components/logos';
	import { Paragraph } from '@echosgraphiques/ui-components/text';

	import { csv } from 'd3-fetch';

	import LineChart from './LineChart.svelte';

	let loaded = false;
	let data;

	onMount(async () => {
		data = await csv('data-citymapper.csv');
		console.log(data)
		loaded = true;
	});
</script>

<LoadingCache hidden={loaded} />
<Header>
	<Title>Lorem ipsum dolor sit amet</Title>
	<Chapo>
		Lorem ipsum dolor sit amet consectetur adipisicing elit. Adipisci commodi nemo consectetur sequi quas enim mollitia deserunt officiis rerum optio?
	</Chapo>
	<Signature>Par Alan Smithee</Signature>
	<Logo />
</Header>
<main>
	{#if data}
		<LineChart {data} />
	{/if}
	<Paragraph>
		Lorem ipsum dolor sit, amet consectetur adipisicing elit. Quia nihil esse, corrupti soluta necessitatibus tenetur minima unde, sapiente pariatur facilis aliquam enim dolore suscipit. Veritatis ducimus distinctio asperiores aut recusandae.
	</Paragraph>
	<Paragraph>
		Lorem ipsum dolor sit amet, consectetur adipisicing elit. Sint itaque reiciendis neque, quis cumque assumenda repellendus explicabo optio iusto praesentium possimus esse dolorem sed vero magnam temporibus eaque omnis laudantium.
	</Paragraph>
	<Paragraph>
		Lorem ipsum dolor, sit amet consectetur adipisicing elit. Optio nihil accusantium pariatur facilis blanditiis, dolor quos laborum, temporibus doloremque ratione quidem ab alias quasi architecto in dignissimos commodi velit beatae!
	</Paragraph>
</main>

<style lang='scss'>
	@import './global.scss';

	main {
		width: 75%;
		max-width: 75rem;
		margin: 0 auto;

		@include md {
			width: 100%;
		}
	}
</style>
