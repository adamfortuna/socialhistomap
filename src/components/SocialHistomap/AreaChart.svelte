<script>
export let width
export let xScale
export let yScale
export let stackedSeriesData
export let data
export let years
export let margin

  
import { area } from 'd3'
import colorsJson from '$data/colors.json'

$: areaGenerator = area()
    .x0((d) => xScale(d[0]))
    .x1((d) => xScale(d[1]))
    .y((d, i) => yScale(data[i].year))
</script>

<g transform="translate({margin.left}, {yScale(years[1])+1})" width={width}>
  {#each stackedSeriesData as platformData, index}
    <path
      transform="translate({margin.left}px)"
      stroke="currentColor"
      fill={colorsJson[platformData.key]}
      d={areaGenerator(platformData)}
    />
  {/each}
</g>