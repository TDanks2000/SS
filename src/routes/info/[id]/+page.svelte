<script lang="ts">
	// import types
	import type { InfoReturn } from '$lib';
	// import components
	import { page } from '$app/stores';
	import IgdbImage from '$lib/components/CustomComponents/IGDBImage.svelte';
	import fetcher from '$lib/utils/fetcher';
	import { writable } from 'svelte/store';
	import Bottom from './components/Bottom.svelte';
	import Middle from './components/Middle.svelte';
	import Top from './components/Top.svelte';

	export const data = writable<InfoReturn>();

	const init = async () => {
		const gameData = await fetcher.igdbInfo($page.params.id);
		data.set(gameData);
	};
	init();
</script>

{#if $data}
	{@const data = $data}
	<div class="pb-20">
		<div>
			<IgdbImage
				imageId={data?.screenshots?.[0]?.image_id ?? ''}
				alt={data?.name}
				class="h-64 w-full overflow-hidden rounded-b-lg object-cover lg:h-72"
				imageSize={'screenshot_med'}
			/>
		</div>
		<div class="mx-auto max-w-6xl px-4 sm:px-6 lg:px-8">
			<Top gameInfo={data} />

			<Middle gameInfo={data} />

			<Bottom gameInfo={data} />
		</div>
	</div>
{/if}

<style>
	* {
		background-color: hsla(222, 83%, 4%, 1);
		background-image: none !important;
	}
</style>
