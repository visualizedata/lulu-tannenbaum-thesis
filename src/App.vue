<template>
  <div id="app">
    <Landing />
    <Description />
    <div class="alert alert-info" v-show="loading">Loading...</div>

    <BarChart :issues="issues"></BarChart>
    <MainContent />
    <Methodology />
    <PopUp :content="currentAd" />
  </div>
</template>

<script>
import BarChart from './components/BarChart.vue'
import Landing from './components/Landing.vue'
import Description from './components/Description.vue'
import MainContent from './components/MainContent.vue'
import Methodology from './components/Methodology.vue'
import PopUp from './components/PopUp.vue'
import { csv, nest, timeParse } from 'd3'
import _ from 'lodash'

const getPercentDiff = (a, b) => ((a - b) / ((a + b) / 2)) * 100

export default {
  name: 'App',
  components: {
    BarChart,
    Landing,
    Description,
    MainContent,
    Methodology,
    PopUp,
  },
  data() {
    return {
      loading: false,
      errored: false,
      issues: [],
      advertisements: [],
    }
  },
  mounted() {
    this.getIssues()
    this.getAdvertisementData()
  },
  computed: {
    currentAd: function() {
      if (_.size(this.advertisements)) {
        return _.find(
          this.advertisements,
          (v, k) => k.slice(1) === '3l7IQbu3V0q9JKxHvUSDzC'
        )
      }
      return null
    },
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
    getAdvertisementData() {
      this.loading = true

      csv('/data/braindata.csv', datum => {
        const date = timeParse('%M:%S')(datum.offset)
        return {
          contentId: datum.content_id,
          name: datum.name,
          samples: +datum.samples,
          NES: getPercentDiff(
            +datum['NES (Neural Engagement Score)'],
            +datum['Benchmark']
          ),
          issues: datum.Issue.trim().split(','),
          offset: date.getSeconds(),
        }
      }).then(data => {
        this.loading = false

        this.advertisements = nest()
          .key(d => d.contentId)
          .map(data)
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
