<script lang="ts">
	import type { PublicToilet } from 'src/interfaces';

	export let toilet: PublicToilet;
</script>

<div>Location: {toilet.properties.VIA} {toilet.properties.LOCALITA}</div>
