<script lang="ts">
	import Header from '$lib/Header.svelte';
	import MunicipioButtons from '$lib/MunicipioButtons.svelte';
	import Toilet from '$lib/Toilet.svelte';
	import type { PublicToilet } from 'src/interfaces';
	import { Loader } from '@googlemaps/js-api-loader';

	let map: google.maps.Map;

	const loader = new Loader({
		apiKey: 'apikey',
		version: 'weekly',
		libraries: ['places']
	});

	loader.load().then(() => {
		map = new google.maps.Map(document.getElementById('map') as HTMLElement, {
			zoom: 12,
			center: { lat: 45.464664, lng: 9.18854 }
		});
	});

	const loadJSON = async (municipio) => {
		try {
			const res = await fetch(
				'http://localhost:9090/https://dati.comune.milano.it/dataset/b36a93df-83fc-4966-babb-e415c47d3ac7/resource/ffd34707-efeb-491b-96c9-c43ef31295ec/download/ds630_servizi_igienici_pubblici_final.geojson'
			);
			const json = await res.json();
			const resultsFiltered = json.features.filter(
				(toilet) => toilet.properties.MUNICIPIO === municipio.toString()
			);

			resultsFiltered.forEach((toilet) => {
				new google.maps.Marker({
					position: {
						lat: toilet.properties.LAT_Y_4326,
						lng: toilet.properties.LONG_X_4326
					},
					title: `${toilet.properties.VIA} ${toilet.properties.LOCALITA}`,
					map,
					icon: '/wc-chimico.png'
				});
			});

			return resultsFiltered;
		} catch (error) {
			console.log(error);
		}
	};

	let promise: Promise<PublicToilet[]>;

	function loadToilets(municipio) {
		promise = loadJSON(municipio);
	}
</script>

<svelte:head>
	<title>Milano Public Toilets</title>
</svelte:head>

<main>
	<Header />

	<div class="HomeContainer">
		<div class="Map">
			<img src="/Milano.png" usemap="#workmap" alt="MilanMap" />
		</div>
		<div class="Toilets">
			<MunicipioButtons handleClick={loadToilets} />

			{#await promise}
				<p>...waiting</p>
			{:then toilets}
				{#if toilets}
					{#each toilets as toilet}
						<Toilet {toilet} />
					{/each}
				{/if}
			{:catch error}
				<p style="color: red">{error.message}</p>
			{/await}
		</div>
	</div>
<div>
  <div id="map" class="GoogleMaps" />
</div>
</main>

<style lang="scss">
	main {
		text-align: center;
		margin: 0 auto;
	}
	.HomeContainer {
		display: grid;
		grid-template-columns: repeat(auto-fit, minmax(300px, 1fr));
    overflow: hidden;
	}
	.Map img {
		width: 100%;
	}
  .GoogleMaps {
    width: 99vw;
    height: 100vh;
  }
</style>
