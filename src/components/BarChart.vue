<template>
  <div>
    <svg />
  </div>
</template>

<script>
import {
  select,
  selectAll,
  scaleLinear,
  axisBottom,
  axisLeft,
  scaleBand,
} from 'd3'
import _ from 'lodash'
export default {
  props: ['issues'],
  data() {
    return {
      chart: null,
    }
  },
  watch: {
    issues(val) {
      if (this.chart !== null) this.chart.remove()
      this.renderChart(val)
    },
  },
  methods: {
    renderChart(issuesVal) {
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
        .domain([0, _.maxBy(issuesVal, 'issues').issues])

      this.chart
        .append('g')
        .call(axisLeft(yScale).ticks(_.maxBy(issuesVal, 'issues').issues))

      const xScale = scaleBand()
        .range([0, chartWidth])
        .domain(issuesVal.map(s => s.day))
        .padding(0.2)

      this.chart
        .append('g')
        .attr('transform', `translate(0, ${chartHeight})`)
        .call(axisBottom(xScale))

      const barGroups = this.chart
        .selectAll('rect')
        .data(issuesVal)
        .enter()

      barGroups
        .append('rect')
        .attr('class', 'bar')
        .attr('x', g => xScale(g.day))
        .attr('y', g => yScale(g.issues))
        .attr('height', g => chartHeight - yScale(g.issues))
        .attr('width', xScale.bandwidth())
        .on('mouseenter', function(actual, i) {
          select(this)
            .transition()
            .duration(300)
            .attr('opacity', 0.6)
            .attr('x', a => xScale(a.day) - 5)
            .attr('width', xScale.bandwidth() + 10)
          barGroups
            .append('text')
            .attr('class', 'value')
            .attr('x', a => xScale(a.day) + xScale.bandwidth() / 2)
            .attr('y', a => yScale(a.issues) - 20)
            .attr('text-anchor', 'middle')
            .text((a, idx) => {
              return idx !== i ? '' : `${a.issues} issues`
            })
        })
        .on('mouseleave', function() {
          selectAll('.issues').attr('opacity', 1)

          select(this)
            .transition()
            .duration(300)
            .attr('opacity', 1)
            .attr('x', a => xScale(a.day))
            .attr('width', xScale.bandwidth())

          svg.selectAll('.value').remove()
        })

      svg
        .append('text')
        .attr('class', 'label')
        .attr('x', -(chartHeight / 2) - margin)
        .attr('y', margin / 2.4)
        .attr('transform', 'rotate(-90)')
        .attr('text-anchor', 'middle')
        .text('Issues opened')

      svg
        .append('text')
        .attr('class', 'label')
        .attr('x', chartWidth / 2 + margin)
        .attr('y', chartHeight + margin * 1.7)
        .attr('text-anchor', 'middle')
        .text('Days')

      svg
        .append('text')
        .attr('class', 'title')
        .attr('x', chartWidth / 2 + margin)
        .attr('y', 40)
        .attr('text-anchor', 'middle')
        .text('Issues in the past 1 week')
    },
  },
}
</script>
<style>
.bar {
  fill: #319bbe;
}
</style>
