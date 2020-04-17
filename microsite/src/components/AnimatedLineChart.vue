<template>
  <div id="linechart-svg-container">
    <svg />
    <h5>Topline Scores</h5>
    <p>
      Average
      <span>+17%</span>Max Engagement
      <span>+54%</span>
    </p>
  </div>
</template>

<script>
import {
  select,
  scaleLinear,
  line,
  axisBottom,
  axisLeft,
  format,
  extent,
} from 'd3'
import _ from 'lodash'
export default {
  props: ['brainData', 'reportXScale'],
  mounted() {
    this.renderChart(this.brainData)
  },
  computed: {
    formattedPercentage: function() {
      if (_.isNil(this.selectedIssue)) {
        return ''
      }
      const issueObj = _.find(this.issues, i => i.issue === this.selectedIssue)
      return format('.0%')(issueObj.percentage)
    },
  },
  methods: {
    renderChart(brainData) {
      console.log(brainData)
      const container = select('#linechart-svg-container').node()
      const margin = 30
      const svgWidth = container.clientWidth

      const svgHeight = svgWidth * 0.6
      const chartWidth = svgWidth - 2 * margin
      const chartHeight = svgHeight - 2 * margin

      const svg = select('#popup svg')
        .attr('width', svgWidth)
        .attr('height', svgHeight)

      const chart = svg
        .append('g')
        .attr('transform', `translate(${margin},${margin})`)

      const yScale = scaleLinear()
        .range([chartHeight, 0])
        .domain([-100, 100])
      // .domain([
      //   _.minBy(this.content, 'NES')['NES'],
      //   _.maxBy(this.content, 'NES')['NES'],
      // ])

      chart.append('g').call(axisLeft(yScale).ticks(0))

      const xScale = scaleLinear()
        .range([0, chartWidth])
        .domain(extent(brainData.map(d => d.offset)))

      chart
        .append('g')
        .attr('transform', `translate(0, ${chartHeight})`)
        .call(
          axisBottom(xScale)
            .ticks(0)
            .tickValues([])
        )

      chart
        .append('path')
        .datum(brainData)
        .attr('class', 'line')
        .attr(
          'd',
          line()
            .x(d => xScale(d.offset))
            .y(d => yScale(d.NES))
        )
        .attr('stroke', '#A44B6D')
        .attr('fill', 'none')
        .attr('stroke-width', 3)

      const progressOverlay = chart
        .append('rect')
        .attr('width', chartWidth)
        .attr('height', chartHeight)
        .attr('fill', 'white')

      // benchmark
      chart
        .append('path')
        .datum(brainData)
        .attr('class', 'benchmark-line')
        .attr(
          'd',
          line()
            .x(d => xScale(d.offset))
            .y(yScale(0))
        )
        .attr('stroke', 'black')
        .attr('stroke-dasharray', '2 1')
        .attr('fill', 'none')

      this.reportXScale(xScale, progressOverlay)
    },
  },
}
</script>
<style>
#popup #linechart-svg-container span {
  color: #a44b6d;
  font-size: 1.2rem;
  font-weight: 600;
  margin: 0px 1.5rem;
}
</style>
