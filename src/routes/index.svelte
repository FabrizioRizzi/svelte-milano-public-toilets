<script lang="ts">
	import Header from '$lib/Header.svelte';
	import Toilet from '$lib/Toilet.svelte';
	import type { PublicToilet } from 'src/interfaces';

	let toilets: PublicToilet[];
	let filteredToilets: PublicToilet[];
	let municipio: number;

	const loadJSON = async () => {
		try {
			const res = await fetch('http://localhost:9090/https://dati.comune.milano.it/dataset/b36a93df-83fc-4966-babb-e415c47d3ac7/resource/ffd34707-efeb-491b-96c9-c43ef31295ec/download/ds630_servizi_igienici_pubblici_final.geojson');
			const json = await res.json();
			toilets = json.features;
			filteredToilets = json.features;
		} catch (error) {
			console.log(error);
		}
	};

	loadJSON();

	function handleClick() {
		if (municipio) {
			filteredToilets = toilets.filter((a) => a.properties.MUNICIPIO === municipio.toString());
		} else {
			filteredToilets = toilets;
		}
	}
</script>

<svelte:head>
	<title>Milano Public Toilets</title>
</svelte:head>

<main>
	<Header />
	<input type="number" min="1" max="9" bind:value={municipio} />
	<button on:click={handleClick}>Cerca Municipio</button>
	{#if toilets}
		<ul>
			{#each filteredToilets as toilet}
				<Toilet {toilet} />
			{/each}
		</ul>
	{/if}
</main>

<style lang="scss">
	main {
		text-align: center;
		margin: 0 auto;
	}
</style>
