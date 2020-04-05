<template>
  <div id="popup">
    <h3>{{name}}</h3>
    <div class="flex-container">
      <div>
        <video controls :src="`/videos/${name}.mp4`" type="video/mp4" />
        <div id="tags">
          <span v-for="issue in issues" v-bind:key="issue">{{issue}}</span>
        </div>
      </div>
      <div id="svg-container">
        <svg />
        <h5>Topline Scores</h5>
        <p>
          Average
          <span>+17%</span>Max Engagement
          <span>+54%</span>
        </p>
      </div>
    </div>
  </div>
</template>

<script>
import _ from 'lodash'
import {
  select,
  scaleLinear,
  scaleTime,
  extent,
  axisLeft,
  axisBottom,
  line,
} from 'd3'

export default {
  name: 'PopUp',
  props: ['content'],
  watch: {
    content: function() {
      console.log(this.content.length)
      if (this.content.length) {
        this.renderChart(this.content)
      }
    },
  },
  computed: {
    name: function() {
      return _.get(_.head(this.content), 'name')
    },
    issues: function() {
      return _.uniq(
        _.filter(
          _.flatMap(this.content, c => c.issues),
          i => !!i
        )
      )
    },
  },
  methods: {
    renderChart(brainData) {
      const container = select('#svg-container').node()
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

      const xScale = scaleTime()
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
    },
  },
}
</script>

<style>
#popup {
  height: 100vh;
  background-color: white;
  /* background: url('../assets/screenshot.png'); */
  color: black;
  text-align: left;
  padding-left: 63px;
  padding-top: 60px;
}
#popup h3 {
  margin-bottom: 4rem;
}
#popup .flex-container {
  display: flex;
}
#popup video {
  width: 92%;
  height: auto;
  margin-bottom: 2.5rem;
}
#popup #tags span {
  padding: 0.2rem 0.5rem;
  border-radius: 1rem;
  border: black solid 1px;
  margin: 0.25rem;
}
#popup #svg-container span {
  color: #a44b6d;
  font-size: 1.2rem;
  font-weight: 600;
  margin: 0px 1.5rem;
}
</style>
