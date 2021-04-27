<script lang="ts">
	import type { PublicToilet } from 'src/interfaces';

	export let toilet: PublicToilet;
	const url = `https://maps.google.com/?q=${toilet.properties.LAT_Y_4326},${toilet.properties.LONG_X_4326}`;
</script>

<div class="Toilet">
	<div>Location: {toilet.properties.VIA} {toilet.properties.LOCALITA}</div>
	<a href={url} target="_blank">Apri in Google maps</a>
</div>

<style>
  .Toilet {
    display: grid;
    grid-template-columns: repeat(2, 1fr);
    border-bottom: 1px solid;
  }

  .Toilet:hover {
    background-color: lightgrey;
  }
</style>