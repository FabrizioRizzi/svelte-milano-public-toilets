<script>
	export let handleClick;
	const municipi = [1, 2, 3, 4, 5, 6, 7, 8, 9];
</script>

<div class="header">
	{#each municipi as municipio}
		<button on:click={() => handleClick(municipio)}>{municipio}</button>
	{/each}
</div>

<style>
	button {
		border: 0;
		padding: 10px;
    margin: 10px;
    background-color: #ff3e00;
    cursor: pointer;
	}
</style>
