<template>
  <div :id="id" class="linechart-svg-container">
    <svg />
    <h5>Top Neural Engagment Scores</h5>
    <p>
      Current Second
      <span class="time">0%</span>
      Average Engagment
      <span class="avg">+17%</span>Max Engagement
      <span class="max">+54%</span>
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
  timeFormat,
} from 'd3'
import _ from 'lodash'
export default {
  props: ['brainData', 'reportXScale', 'id'],
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
      console.log('BRAIN DATA', brainData)
      select(`[id="${this.id}"].linechart-svg-container svg`)
        .selectAll('*')
        .remove()
      const container = select(
        `[id="${this.id}"].linechart-svg-container`
      ).node()
      const margin = 30
      const svgWidth = container.clientWidth

      const svgHeight = svgWidth * 0.6
      const chartWidth = svgWidth - 2 * margin
      const chartHeight = svgHeight - 2 * margin

      const svg = select(`[id="${this.id}"] svg`)
        .attr('width', svgWidth)
        .attr('height', svgHeight)

      const chart = svg
        .append('g')
        .attr('transform', `translate(${margin},${margin})`)

      const yScale = scaleLinear()
        .range([chartHeight, 0])
        .domain([-60, 90])
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
        .attr('class', 'axis')
        .call(
          axisBottom(xScale).tickFormat(d =>
            timeFormat('%M:%S')(new Date(0).setSeconds(d))
          )
        )
        .selectAll('text')
        .attr('transform', 'rotate(-45)')
        .style('text-anchor', 'end')

      chart
        .append('g')
        .attr('transform', `translate(0, 0)`)
        .attr('class', 'axis')
        .call(
          axisLeft(yScale)
            .tickValues([-60, -30, 0, 30, 60, 90])
            .tickFormat(d => `${d}%`)
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
        .attr('transform', 'translate(1, 0)')
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
        .attr('stroke', '#3A3737')
        .attr('stroke-dasharray', '3 1')
        .attr('stroke-width', '0.75')
        .attr('fill', 'none')

      const first = _.head(brainData)
      const pFormat = format('+.0%')
      select(container)
        .select('.avg')
        .html(pFormat(_.get(first, 'average')))
      select(container)
        .select('.max')
        .html(pFormat(_.get(first, 'max')))

      this.reportXScale(this.id, brainData, xScale, progressOverlay)
    },
  },
}
</script>
<style>
.linechart-svg-container span {
  color: #a44b6d;
  font-size: 1.2rem;
  font-weight: 600;
  margin: 0px 1.5rem;
}

.linechart-svg-container .axis path {
  stroke: #e8ebef;
}

.linechart-svg-container .axis .tick line {
  stroke: #e8ebef;
}
.linechart-svg-container .axis text {
  font-size: 0.5rem;
  fill: #5d697b;
  stroke: none;
}
</style>
