<script>
  import { onDestroy, onMount } from 'svelte';
  import { differenceInSeconds, startOfDay, endOfDay } from 'date-fns';
  import arc from 'svg-arc';
  import SunCalc from 'suncalc-tz';
  import Clock from './Clock.svelte';
  
  const range = n => [...Array(n).keys()];
  
  let interval;
  export let lat = 0;
  export let lon = 0;
  let now = new Date();
  let sunrise = new Date();
  let sunset = new Date();
  let midnight = new Date();
  let dayLength = 1;
  let sunriseAngle = 0;
  let sunsetAngle = 0;
  let solarNoonAngle = 0;
  let nadirAngle = 0;
  let night = 0;
  let nightEnd = 0;
  let dusk = 0;
  let dawn = 0;
  let nauticalDawn = 0;
  let nauticalDusk = 0;

  $: hourAngle = (now.getHours() + now.getMinutes() / 60) * 15 + 90;
  $: {
    const times = SunCalc.getTimes(now, lat, lon);
    sunrise = times.sunrise;
    sunset = times.sunset;
    midnight = startOfDay(now);
    dayLength = differenceInSeconds(endOfDay(now), midnight);
    sunriseAngle = differenceInSeconds(midnight, sunrise) / dayLength * 360;
    sunsetAngle = differenceInSeconds(midnight, sunset) / dayLength * 360;
    solarNoonAngle = differenceInSeconds(midnight, times.solarNoon) / dayLength * 360;
    nadirAngle = differenceInSeconds(midnight, times.nadir) / dayLength * 360;
    night = differenceInSeconds(midnight, times.night) / dayLength * 360;
    nightEnd = differenceInSeconds(midnight, times.nightEnd) / dayLength * 360;
    dusk = differenceInSeconds(midnight, times.dusk) / dayLength * 360;
    dawn = differenceInSeconds(midnight, times.dawn) / dayLength * 360;
    nauticalDusk = differenceInSeconds(midnight, times.nauticalDusk) / dayLength * 360;
    nauticalDawn = differenceInSeconds(midnight, times.nauticalDawn) / dayLength * 360;
  }
  
  onMount(() => {
    interval = setInterval(() => now = new Date(), 1000);
  });
  onDestroy(() => clearInterval(interval));
</script>

<svg width={400} height={400} viewBox="0 0 200 200" style="color: white;">
  <g transform="translate(100, 100)">
    <circle cx={0} cy={0} r={99} fill="hsl(211,63%,72%)" stroke="black" />
    <path
      d={arc({
        x: 0,
        y: 0,
        R: 99, 
        start: 180 - sunsetAngle,
        end: 180 - sunriseAngle
      })}
      fill="rgb(6,23,57)"
      opacity={0.5}
    />
    <path
      d={arc({
        x: 0,
        y: 0,
        R: 99, 
        start: 180 - dusk,
        end: 180 - dawn
      })}
      fill="rgb(6,23,57)"
      opacity={0.5}
    />
    <path
      d={arc({
        x: 0,
        y: 0,
        R: 99, 
        start: 180 - nauticalDusk,
        end: 180 - nauticalDawn
      })}
      fill="rgb(6,23,57)"
      opacity={0.5}
    />
    <path
      d={arc({
        x: 0,
        y: 0,
        R: 99, 
        start: 180 - night,
        end: 180 - nightEnd
      })}
      fill="rgb(6,23,57)"
      opacity={0.5}
    />
    <g opacity={.5}>
      <circle cx={0} cy={0} r={67} fill="none" stroke="currentColor" />
      <g transform={`rotate(${solarNoonAngle})`}>
        <circle cx={0} cy={67} r={2} fill="currentColor" />
      </g>
      <g transform={`rotate(${nadirAngle})`}>
        <circle cx={0} cy={67} r={2} fill="currentColor" />
      </g>
    </g>
    {#each range(144) as minute}
      <line
        x1={93}
        y1={0}
        x2={97}
        y2={0}
        opacity={.5}
        stroke="currentColor"
        transform={`rotate(${minute * 2.5})`} />
    {/each}
    {#each range(24) as hour}
      {#if !(hour % 2)}
        <g transform={`rotate(${hour * 15})`}>
          <line
            x1={90}
            y1={0}
            x2={97}
            y2={0}
            stroke="currentColor" />
          <text
            x={0}
            y={85}
            fill="currentColor"
            font-family="sans-serif"
            font-size={10}
            alignment-baseline="middle"
            text-anchor="middle">
            {hour}
          </text>
        </g>
      {/if}
    {/each}
    
    <g transform={`rotate(${hourAngle})`}>
      <line
        x1={0}
        y1={0}
        x2={61}
        y2={0}
        stroke="currentColor"
        stroke-linecap="round"
        stroke-width={2} />
      <line
        x1={73}
        y1={0}
        x2={97}
        y2={0}
        stroke="currentColor"
        stroke-linecap="round"
        stroke-width={2} />
      <circle
        cx={67} cy={0} r={6}
        stroke="currentColor"
        stroke-width={2}
        fill="currentColor"
        fill-opacity={.5} />
    </g>
    <g transform={`translate(-50, -50) translate(${-30 * Math.cos(hourAngle * Math.PI / 180)},${-30 * Math.sin(hourAngle * Math.PI / 180)})`}>
      <Clock time={now} />
    </g>
  </g>
</svg>