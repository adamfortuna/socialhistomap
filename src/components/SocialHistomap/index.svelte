<script>
export let width
export let height

import { stack, stackOffsetExpand } from 'd3'
import { extent } from 'd3-array'
import { scaleLinear } from 'd3-scale'

import AxisY from '$components/SocialHistomap/AxisY.svelte'
import AreaChart from '$components/SocialHistomap/AreaChart.svelte'
import Labels from '$components/SocialHistomap/Labels.svelte'

import data from '$data/dataArray.json'
import colorsJson from '$data/colors.json'
import platformOrder from '$data/order.json'

const years = data.map((d) => d.year).sort();
const platformArea = stack().keys(platformOrder).offset(stackOffsetExpand)
const stackedSeriesData = platformArea(data)

const margin = { top: 20, right: 0, bottom: 20, left: 62 }

$: yDomain = extent(years)
$: height = years.length * height
$: mini = width < 1000
$: innerWidth = width - margin.left - margin.right;

$: yScale = scaleLinear().range([0, height]).domain(yDomain)
$: xScale = scaleLinear().range([0, innerWidth]).domain([0, 1])

</script>

<svg {width} {height}>
  <AreaChart {yScale} {xScale} {stackedSeriesData} {margin} {width} {years} {data} />
  <AxisY {width} {yScale} {years} />
  <Labels {width} {yScale} {xScale} {margin} {stackedSeriesData} {years} />
</svg>

<style>
:global(.tick text, .axis-title) {
  font-size: 18px; /* How big our text is */
  fill: black; /* The color of our text */
  font-weight: 700;
}
</style>
