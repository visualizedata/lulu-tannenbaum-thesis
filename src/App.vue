<template>
  <div id="app">
    <Landing />
    <Description />
    <div class="alert alert-info" v-show="loading">Loading...</div>
    <div class="alert alert-danger" v-show="errored">An error occurred</div>
    <BarChart :issues="issues" :selectedIssue="selectedIssue"></BarChart>
    <MainContent />
    <Methodology />
  </div>
</template>

<script>
import BarChart from './components/BarChart.vue'
import Landing from './components/Landing.vue'
import Description from './components/Description.vue'
import MainContent from './components/MainContent.vue'
import Methodology from './components/Methodology.vue'
import { csv } from 'd3'
import _ from 'lodash'

export default {
  name: 'App',
  components: {
    BarChart,
    Landing,
    Description,
    MainContent,
    Methodology,
  },
  data() {
    return {
      loading: false,
      errored: false,
      issues: [],
    }
  },
  mounted() {
    this.getIssues()
  },
  methods: {
    getIssues() {
      this.loading = true

      csv('/data/surveyData.csv', datum => ({
        issue: datum.Issue,
        percentage: +_.replace(datum.Percentage, '%', '') / 100,
      })).then(data => {
        this.loading = false
        this.issues = _.shuffle(data)
      })
    },
  },
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #ffffff;
  background-color: #305581;
}
#app h1 {
  font-size: 80px;
}
#app h2 {
  font-size: 60px;
}
#app h3 {
  font-size: 48px;
}
</style>
