<script>
  export let width = 100;
  export let height = 100;
  export let time;
  
  const range = n => [...Array(n).keys()];

  $: h = time.getHours();
  $: m = time.getMinutes();
  $: s = time.getSeconds();
  $: hour = h + m / 60;
  $: minute = m + s / 60;
  $: seconds = s + time.getMilliseconds() / 1000;
</script>

    <svg
      width={width}
      height={height}
      viewBox="0 0 200 200"
      preserveAspectRatio="xMinYMin meet"
      xmlns="http://www.w3.org/2000/svg"
    >
      <g transform="translate(100, 100)">
        <circle
          cx={0}
          cy={0}
          r={99}
          stroke="currentColor"
          stroke-width={2}
          fill="white"
        />
        <line
          x1={0}
          y1={20}
          x2={0}
          y2={-60}
          stroke-width={6}
          stroke="currentColor"
          transform={`rotate(${hour * 30})`}
        />
        <line
          x1={0}
          y1={20}
          x2={0}
          y2={-88}
          stroke-width={3}
          stroke="currentColor"
          transform={`rotate(${minute * 6})`}
        />
        <line
          x1={0}
          y1={20}
          x2={0}
          y2={-93}
          stroke-width={2}
          stroke="red"
          transform={`rotate(${seconds * 6})`}
        />
        <circle
          cx={0}
          cy={0}
          r={3}
          fill="red"
        />
        {#each range(60) as tick}
          <line
            key={tick}
            x1={0}
            y1={tick % 5 ? -92 : -86}
            x2={0}
            y2={-95}
            stroke-width={2}
            stroke="currentColor"
            transform={`rotate(${tick * 6})`}
          />
        {/each}
      </g>
    </svg>