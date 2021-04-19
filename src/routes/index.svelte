<script lang="ts">
	import Header from '$lib/Header.svelte';
	import type { PublicToilet } from 'src/interfaces';

	let toilets: PublicToilet[];
	let filteredToilets: PublicToilet[];
	let municipio: number;

	const loadJSON = async () => {
		try {
			const res = await fetch('static/data/servizi_igienici_pubblici.geojson');
			const json = await res.json();
			toilets = json.features;
			filteredToilets = json.features;
		} catch (error) {
			console.log(error);
		}
	};

	loadJSON();

	let promise = loadJSON();

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
	<input type="number" min=1 max=9 bind:value={municipio} />
	<button on:click={handleClick}>Refresh</button>
	{#if toilets}
		<ul>
			{#each filteredToilets as toilet}
				<li>{toilet.properties.VIA + toilet.properties.LOCALITA}</li>
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
