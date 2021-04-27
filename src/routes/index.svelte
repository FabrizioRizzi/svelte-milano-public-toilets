<script lang="ts">
	import Header from '$lib/Header.svelte';
	import NumericInput from '$lib/NumericInput.svelte';
	import Toilet from '$lib/Toilet.svelte';
	import type { PublicToilet } from 'src/interfaces';

	let municipio: number;

	const loadJSON = async () => {
		try {
			const res = await fetch(
				'http://localhost:9090/https://dati.comune.milano.it/dataset/b36a93df-83fc-4966-babb-e415c47d3ac7/resource/ffd34707-efeb-491b-96c9-c43ef31295ec/download/ds630_servizi_igienici_pubblici_final.geojson'
			);
			const json = await res.json();
			const filteredResults: PublicToilet[] = municipio
				? json.features.filter((toilet) => toilet.properties.MUNICIPIO === municipio.toString())
				: json.features;
			return filteredResults;
		} catch (error) {
			console.log(error);
		}
	};

	let promise = loadJSON();

	function handleClick() {
		promise = loadJSON();
	}
</script>

<svelte:head>
	<title>Milano Public Toilets</title>
</svelte:head>

<main>
	<Header />

	<NumericInput bind:value={municipio} min=0 max=9 />

	<button on:click={handleClick}>Cerca Municipio</button>
	
  {#await promise}
		<p>...waiting</p>
	{:then toilets}
		{#each toilets as toilet}
			<Toilet {toilet} />
		{/each}
	{:catch error}
		<p style="color: red">{error.message}</p>
	{/await}
  
</main>

<style lang="scss">
	main {
		text-align: center;
		margin: 0 auto;
	}
</style>
