<script lang="ts">
	import { onMount } from 'svelte';
	import { fetchConfigurations } from '../../utils/ConfigManagment';
	import DhtConfiguration from './DhtConfiguration.svelte';
	import DownloadPathSelector from './DownloadPathSelector.svelte';

	const debounce = <T extends (...args: any[]) => any>(
		func: T,
		delay: number
	): ((...args: Parameters<T>) => void) => {
		let timer: ReturnType<typeof setTimeout>;

		return function (this: ThisParameterType<T>, ...args: Parameters<T>): void {
			const context = this;
			clearTimeout(timer);
			timer = setTimeout(() => func.apply(context, args), delay);
		};
	};

	const debounceConfig = debounce(async () => {
		await fetchConfigurations();
	}, 1300);

	onMount( () => {
		debounceConfig;
	});

	// let updatedConfig: RqbitDesktopConfig = {
	// 	default_download_location: '',
	// 	dht: null,
	// 	tcp_listen: null,
	// 	upnp:null,
	// 	persistence: null,
	// 	peer_opts: null,
	// 	http_api: null,
	// };
	// $: if ($torrentConfigStore) {
	// 	updatedConfig = $torrentConfigStore;
	// }
	//
	//
	// async function save() {
	// 	await saveConfig(updatedConfig);
	// }
</script>

<!--pb class here -->
<h1 class="text-xl text-red-500">DO NOT TOUCH RADIOCTIVE, AND PAINFUL EXPERIENCE AWAITS</h1>
<div class="py-5 pb-20">
	<div class="sm:flex sm:items-center">
		<div class="flex flex-col gap-3 divide-y-2 sm:flex-auto">
			<div>
				<h1 class="text-base font-semibold leading-6">Torrent Client Configuration</h1>
				<p class="mt-2 text-sm text-gray-400/80">
					Configure your torrent client for optimal download performance and user customization.
				</p>
			</div>
			<!--			<form on:submit|preventDefault={save}>-->
			<!--				&lt;!&ndash; Directly bind to the property without optional chaining &ndash;&gt;-->
			<!--				<input type="text" bind:value={updatedConfig.default_download_location} placeholder="Default Download Location">-->
			<!--				<button type="submit">Save</button>-->
			<!--			</form>-->

			<section class="gap-x-18 grid py-12 md:grid-cols-3">
				<div>
					<h2 class="text-base font-semibold leading-7 text-white">Download Directory Setup</h2>
					<p class="mt-1 text-sm leading-6 text-gray-400">
						Set the directory for storing downloaded torrent files.
					</p>
				</div>
				<DownloadPathSelector />
			</section>
			<DhtConfiguration />
		</div>
	</div>
</div>
