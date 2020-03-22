<template>
  <div id="app">
    <Landing />
    <Description />
    <div class="alert alert-info" v-show="loading">Loading...</div>
    <div class="alert alert-danger" v-show="errored">An error occurred</div>
    <BarChart :issues="issues"></BarChart>
  </div>
</template>

<script>
import BarChart from './components/BarChart.vue'
import Landing from './components/Landing.vue'
import Description from './components/Description.vue'
// import moment from 'moment'
import { csv } from 'd3'
// import axios from 'axios'
import _ from 'lodash'

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
      repository: '',
      startDate: null,
    }
  },
  mounted() {
    this.getIssues()
  },
  methods: {
    getIssues() {
      this.loading = true
      // this.startDate = moment()
      //   .subtract(6, 'days')
      //   .format('YYYY-MM-DD')

      // axios
      //   .get(
      //     `https://api.github.com/search/issues?q=repo:${this.repository}+is:issue+is:open+created:>=${this.startDate}`,
      //     { params: { per_page: 100 } }
      //   )
      //   .then(response => {
      //     const payload = this.getDateRange()

      //     _.each(response.data.items, item => {
      //       const key = moment(item.created_at).format('MMM Do YY')
      //       const obj = payload.filter(o => o.day === key)[0]
      //       obj.issues += 1
      //     })

      //     this.issues = payload
      //     console.log(this.issues)
      //   })
      //   .catch(error => {
      //     console.error(error)
      //     this.errored = true
      //   })
      //   .finally(() => (this.loading = false))

      csv('/data/surveyData.csv', datum => ({
        issue: datum.Issue,
        percentage: +_.replace(datum.Percentage, '%', '') / 100,
      })).then(data => {
        console.log('got issues', data)
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
</style>
