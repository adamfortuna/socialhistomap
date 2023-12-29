<script>
export let xScale
export let yScale
export let margin
export let stackedSeriesData
export let years
export let width


import maxBy from 'lodash/maxBy'
import { forceSimulation, forceX, forceY, forceCollide } from "d3-force"
import { extent } from 'd3-array'
import { scaleSqrt } from "d3-scale";

const lengths = stackedSeriesData.map((d) => d.key.length)
$: radiusScale = scaleSqrt().domain(extent(lengths)) // The same domain passed to xScale

let simulation = forceSimulation(stackedSeriesData);
let nodes = [];
simulation.on("tick", () => {
  nodes = simulation.nodes();
});

$: {
    simulation
      .force(
        "x",
        forceX()
          .x(d => {
            // There really should be a better way to do this?
            const totals = d.map((yearData, index) => {
              return { index, x0: yearData[0], x1: yearData[1], total: yearData[1] - yearData[0], year: yearData.data.year }
            })
            const topYear = maxBy(totals, (yearData) => yearData.total)
            const xPosition = topYear ? xScale(topYear.x0 + (topYear.x1 - topYear.x0) / 2) : 0
            return xPosition;
            
          })
          .strength(0.8)
      )
      .force(
        "y",
        forceY()
          .y(d => {
            const totals = d.map((yearData) => {
              return { total: yearData[1] - yearData[0], year: yearData.data.year }
            })
            const topYear = maxBy(totals, (yearData) => yearData.total)
            const yPosition = topYear ? yScale(d.key === 'Usenet' ? 1981.5 : topYear.year) : 0
            return yPosition;
          })
          .strength(0.2)
      )
      .force("collide", forceCollide().radius(d => {

        return radiusScale(d.key.length);
      }))
      .alpha(0.3) // [0, 1] The rate at which the simulation finishes. You should increase this if you want a faster simulation, or decrease it if you want more "movement" in the simulation.
      .alphaDecay(0.0005) // [0, 1] The rate at which the simulation alpha approaches 0. you should decrease this if your bubbles are not completing their transitions between simulation states.
      .restart();
  }
</script>

<g transform="translate({margin.left}, {yScale(years[1])+1})" width={width}>

  {#each nodes as node, i}
    <text
      x={node.x}
      y={node.y}
    >{node.key}</text>
  {/each}
</g>

<style>
text {
  text-anchor: middle;
  font-weight: 700;
  font-size: 24;
}
</style>