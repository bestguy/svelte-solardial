<script>
  import SolarDial from './SolarDial.svelte';

  let lat = 33;
  let lon = -117;
</script>

<style>
  :global(body) {
    background-color: black;
  }
</style>

<SolarDial {lat} {lon} />

<div>
  <input type="number" bind:value={lat} />
  <input type="number" bind:value={lon} />
</div>
