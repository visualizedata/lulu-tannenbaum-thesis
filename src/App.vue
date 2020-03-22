<template>
  <div id="app">
    <Landing />
    <Description />
    <div class="alert alert-info" v-show="loading">Loading...</div>
    <div class="alert alert-danger" v-show="errored">An error occurred</div>
    <div>
      <p>{{ formattedPercentage }} state that they care about</p>
      <select v-model="selectedIssue">
        <option v-for="issue in issues" v-bind:key="issue.issue">
          {{ issue.issue }}
        </option>
      </select>
    </div>
    <BarChart :issues="issues" :selectedIssue="selectedIssue"></BarChart>
  </div>
</template>

<script>
import BarChart from './components/BarChart.vue'
import Landing from './components/Landing.vue'
import Description from './components/Description.vue'
// import moment from 'moment'
import { csv, format } from 'd3'
// import axios from 'axios'
import _ from 'lodash'
console.log('fromat', format)
export default {
  name: 'App',
  components: {
    BarChart,
    Landing,
    Description,
  },
  data() {
    return {
      loading: false,
      errored: false,
      issues: [],
      selectedIssue: null,
    }
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
        console.log('got issues', data)
        this.loading = false
        this.issues = _.shuffle(data)
        this.selectedIssue = _.first(this.issues).issue
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
</style>
