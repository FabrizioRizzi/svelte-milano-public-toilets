<script lang="ts">
  import Header from '$lib/Header.svelte';

  const loadJSON = async () => {
    try {
      const res = await fetch('static/data/servizi_igienici_pubblici.geojson');
      return await res.json();
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

  <button on:click={handleClick}>Refresh</button>
  {#await promise}
    <p>...loading</p>
  {:then todos}
    <span>{todos?.features[0].properties.MUNICIPIO}</span>
  {:catch error}
    <p style="color: red">{error}</p>
  {/await}
</main>

<style lang="scss">
  main {
    text-align: center;
    margin: 0 auto;
  }
</style>
