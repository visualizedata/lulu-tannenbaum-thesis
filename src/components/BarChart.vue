<template>
  <div id="flex-container">
    <div>
      <h2>People State</h2>
      <p>Lorem ipsum dolor sit amet, consectetur adipiscing elit. Mauris luctus et nisl ut porttitor. Praesent magna ante, suscipit eget convallis nec, sollicitudin non neque. Duis et tincidunt sem, quis semper elit. Praesent efficitur eleifend mauris a rutrum. In nec sem metus. Mauris dignissim bibendum bibendum. Pellentesque dolor nunc, rhoncus in aliquet vitae, vestibulum vitae nibh. Pellentesque ornare lacus a molestie consequat.</p>
    </div>
    <div id="svg-container">
      <svg />
    </div>
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
      const container = select('#svg-container').node()
      const margin = 60
      const svgWidth = container.clientWidth

      const svgHeight = svgWidth * 0.6
      const chartWidth = svgWidth - 2 * margin
      const chartHeight = svgHeight - 2 * margin

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
          return _.isEqual(d.issue, selectedIssue)
        })
    },
  },
}
</script>
<style>
#flex-container {
  padding-top: 60px;
  padding-left: 63px;
  display: flex;
  text-align: left;
}

#flex-container > div {
  flex: 50%;
}
#flex-container p {
  line-height: 125%;
}

.bar {
  fill: #ffffff;
}

.bar.selected {
  fill: #da3767;
}
</style>
