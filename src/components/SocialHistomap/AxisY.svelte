<script>
export let width;
export let yScale;
export let years;

import { path } from 'd3'

$: console.log({width})

let leftPath = path()

$: {
  leftPath = path()
  leftPath.moveTo(0, 0)
  leftPath.lineTo(0, yScale(years[years.length - 1]))
  leftPath.closePath()
}
</script>


<g class='axis y' transform="translate(0, {yScale(years[1])})">
  <!-- Top line -->
  <line x1={0} x2={width} y1={0} y2={0} stroke="black" stroke-width="3" /> 

  {#each years as year, index}
    <g class='tick' transform="translate(0, {yScale(year)})">
      <line x1={0} x2={width} y1={0} y2={0} stroke="currentColor" stroke-opacity="0.5" stroke-dasharray="2,2" /> 
      <text y={-8} x={8}>{year}</text>
    </g>

    <path stroke="currentColor" d={leftPath.toString()} style="transform: translateX(60px); stroke-width: 3;"></path>
  {/each}
</g>
