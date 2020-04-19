<template>
  <div id="app">
    <Landing />
    <Description />
    <div class="alert alert-info" v-show="loading">Loading...</div>
    <BarChart :issues="issues"></BarChart>
    <UnconsciousEngagment />
    <Analysis
      :content="trumpAnalysis"
      id="trump"
      title="Trump's emphasis on the economy"
      description="The highly inspirational..."
      :onMoreInfoClick="toggleShowHowToRead"
    />
    <div id="tone" class="full-height">
      <h3>
        While the tone of Trump's official advertisement is highlighly inspirational, a
        <strong>fearful tone</strong> is the most engaging.
      </h3>
      <p>From an evolutionary...</p>
    </div>
    <Analysis
      :content="bloombergAnalysis"
      id="bloomberg"
      title="Bloomberg utilized fear in advertising"
      description="Bloomber utilizes..."
      :onMoreInfoClick="toggleShowHowToRead"
    />
    <Explore :onClick="exploreAd" :ads="advertisements" />
    <Methodology />
    <PopUp v-if="showPopup" :content="popupContent" :onClose="toggleShowPopup" />
    <Interpretation v-if="showHowToRead" :onClose="toggleShowHowToRead" />
  </div>
</template>

<script>
import BarChart from './components/BarChart.vue'
import UnconsciousEngagment from './components/UnconsciousEngagment.vue'
import Landing from './components/Landing.vue'
import Description from './components/Description.vue'
import Analysis from './components/Analysis.vue'
import Explore from './components/Explore.vue'
import Methodology from './components/Methodology.vue'
import PopUp from './components/PopUp.vue'
import Interpretation from './components/Interpretation.vue'
import { csv, nest, timeParse } from 'd3'
import _ from 'lodash'

const getPercentDiff = (a, b) => ((a - b) / ((a + b) / 2)) * 100

export default {
  name: 'App',
  components: {
    BarChart,
    UnconsciousEngagment,
    Landing,
    Description,
    Analysis,
    Explore,
    Methodology,
    PopUp,
    Interpretation,
  },
  data() {
    return {
      loading: false,
      errored: false,
      issues: [],
      advertisements: [],
      showPopup: false,
      showHowToRead: false,
      popupContent: null,
    }
  },
  mounted() {
    this.getIssues()
    this.getAdvertisementData()
  },
  computed: {
    trumpAnalysis: function() {
      if (_.size(this.advertisements)) {
        return _.find(
          this.advertisements,
          (v, k) => k.slice(1) === '6gRl8mmKFzqSNJQKBOZRsu'
        )
      }
      return null
    },
    bloombergAnalysis: function() {
      if (_.size(this.advertisements)) {
        return _.find(
          this.advertisements,
          (v, k) => k.slice(1) === '2ZSfXfc4mZ71U4kB17iTIf'
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
    async getAdvertisementData() {
      this.loading = true
      const tags = await csv('data/tagsData.csv')

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
          tags: _.get(
            _.find(tags, t => t.content_id == datum.content_id),
            'issue'
          ),
          offset: date.getSeconds(),
        }
      }).then(data => {
        this.loading = false

        this.advertisements = nest()
          .key(d => d.contentId)
          .map(data)
      })
    },
    toggleShowPopup() {
      this.showPopup = !this.showPopup
    },
    toggleShowHowToRead() {
      this.showHowToRead = !this.showHowToRead
    },
    updatePopupContent: function(contentId) {
      const ad = _.find(this.advertisements, (v, k) => k.slice(1) === contentId)
      console.log('new ad', ad)
      this.popupContent = ad
    },
    exploreAd(contentId) {
      console.log('contentID', contentId)
      this.toggleShowPopup()
      this.updatePopupContent(contentId)
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
  position: relative;
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
#app .full-height {
  height: 100vh;
}
#app .close {
  position: absolute;
  right: 1rem;
  top: 1rem;
  -webkit-appearance: none;
  border-radius: 50%;
  border: 1px solid black;
  height: 2rem;
  width: 2rem;
}
#app #tone {
  padding-top: 400px;
  text-align: left;
}
</style>
