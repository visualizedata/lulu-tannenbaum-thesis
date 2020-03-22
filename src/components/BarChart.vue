<template>
  <div>
    <svg />
  </div>
</template>

<script>
import { select, scaleLinear, axisBottom, axisLeft, scaleBand } from 'd3'
import _ from 'lodash'
export default {
  props: ['issues', 'selectedIssue'],
  data() {
    return {
      chart: null,
    }
  },
  watch: {
    issues(val) {
      if (this.chart !== null) this.chart.remove()
      this.renderChart(val, this.selectedIssue)
    },
    selectedIssue(val) {
      if (this.chart !== null) this.chart.remove()
      this.renderChart(this.issues, val)
    },
  },
  methods: {
    renderChart(issuesVal, selectedIssue) {
      const margin = 60
      const svgWidth = 1000
      const svgHeight = 600
      const chartWidth = 1000 - 2 * margin
      const chartHeight = 600 - 2 * margin

      const svg = select('svg')
        .attr('width', svgWidth)
        .attr('height', svgHeight)

      this.chart = svg
        .append('g')
        .attr('transform', `translate(${margin},${margin})`)

      const yScale = scaleLinear()
        .range([chartHeight, 0])
        .domain([0, _.maxBy(issuesVal, 'percentage').percentage])

      this.chart.append('g').call(axisLeft(yScale).ticks(0))

      const xScale = scaleBand()
        .range([0, chartWidth])
        .domain(issuesVal.map(s => s.issue))
        .padding(0.2)

      this.chart
        .append('g')
        .attr('transform', `translate(0, ${chartHeight})`)
        .call(
          axisBottom(xScale)
            .ticks(0)
            .tickValues([])
        )

      const barGroups = this.chart
        .selectAll('rect')
        .data(issuesVal)
        .enter()

      barGroups
        .append('rect')
        .attr('class', 'bar')
        .attr('x', g => xScale(g.issue))
        .attr('y', g => yScale(g.percentage))
        .attr('height', g => chartHeight - yScale(g.percentage))
        .attr('width', xScale.bandwidth())
        .classed('selected', d => {
          console.log(d, selectedIssue)
          return _.isEqual(d.issue, selectedIssue)
        })
    },
  },
}
</script>
<style>
.bar {
  fill: #ffffff;
}

.bar.selected {
  fill: #da3767;
}
</style>
