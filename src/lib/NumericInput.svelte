<script>
  export let value = null;
  export let min;
  export let max;
</script>

<div class="header">
  <input type="number" min={min} max={max} bind:value={value} />
</div>

<style>
  input {
    border: 0;
    border-bottom: 1px solid;
  }
</style>