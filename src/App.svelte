<script>
  import SolarDial from './SolarDial.svelte';

  let lat = 33;
  let lon = -117;

  if (navigator.geolocation) {
    navigator.geolocation.getCurrentPosition((position) => {
      lat = position.coords.latitude;
      lon = position.coords.longitude;
    }, console.error);
  }
</script>

<style>
  :global(body) {
    background-color: black;
    display: flex;
    width: 100vw;
    height: 100vh;
    align-items: center;
    justify-content: center;
  }
</style>
<div style="display: inline-flex;">
  <SolarDial {lat} {lon} size="80vmin" />
</div>